# CubionLink Protocol — v0 Specification DRAFT

> The software protocol that runs over the CFC connector.  
> This protocol CAN evolve — it just must stay backward compatible.

---

## Overview

CubionLink is the protocol by which cubes:
1. Discover each other when connected
2. Share resources (CPU, RAM, GPU, sensors, storage)
3. Route data across a cube mesh
4. Handle cube insertion and removal gracefully

---

## Layer stack

```
[ Application / SDK ]
[ Resource Sharing Layer ]
[ Routing Layer ]
[ Discovery Layer ]
[ Physical: CFC TX/RX pins — RS-485 half-duplex per face ]
```

**Why RS-485:** CubionLink requires a peer-to-peer bus with no master/slave hierarchy. I²C is fundamentally a master/slave protocol and is incompatible with Cubion's decentralized philosophy. RS-485 allows any node to initiate, handles multi-point, and is battle-tested for 40+ years. Each CFC face has its own independent RS-485 pair (TX/RX) — 6 independent buses per cube, one per face.

---

## Discovery (Layer 1)

### On connection
When a new face connection is detected (voltage on ID pins):

1. Both cubes send `HELLO` packet simultaneously
2. `HELLO` contains: cube UID (128-bit), CFC version, protocol version, resource table
3. Each cube stores neighbor info for that face
4. If both sides support higher protocol version → negotiate up

### HELLO packet format (v0)
```
[MAGIC: 0x3141592653589793] [PI_DEPTH: 1 byte] [CFC_VERSION: 1 byte] [PROTOCOL_VERSION: 1 byte] [UID: 16 bytes] [RESOURCES: 8 bytes] [CHECKSUM: 2 bytes]
```

### Magic number — π

The magic constant is the first 16 significant digits of π, stored as a fixed 8-byte field:
```
MAGIC = 0x3141592653589793
```

The `PI_DEPTH` field (1 byte) declares how many digits of π the sending cube verifies during handshake:

```
PI_DEPTH = 7  → verifies 3141592              (year 1)
PI_DEPTH = 8  → verifies 31415926             (year 2)
PI_DEPTH = 16 → verifies 3141592653589793     (full precision)
```

**One digit per year.** Each annual CubionLink spec release increments the expected PI_DEPTH by 1. Two cubes negotiate down to the lowest common PI_DEPTH — a 2027 cube and a 2087 cube can still talk. The older cube is simply less precise.

**Why π:**
A constant that never changes. That exists independent of any human convention, any standard body, any company.
Every cube that has ever existed, and every cube that will ever exist, opens its first message with π.
The network converges toward infinite precision — exactly like π itself.

### Resource table (8 bytes, v0)
```
bit 0-3:   CPU cores (0-15)
bit 4-7:   RAM tier (0=none, 1=<1MB, 2=<1GB, 3=<8GB, 4=<64GB, ...)
bit 8-11:  Storage tier (same scale)
bit 12:    GPU present
bit 13:    Display present
bit 14:    Sensor present
bit 15:    Power source capable
bit 16-63: Reserved (must be 0 in v0, ignored by v0 receivers)
```

---

## Routing (Layer 2)

Each cube maintains a **face routing table**: for each of its 6 faces, what UIDs are reachable through it.

Packets include destination UID. Each cube forwards to the face whose routing table contains the destination.

This is intentionally simple — no central router, no configuration.

---

## Resource Sharing (Layer 3)

Resources are **advertised, not assigned**.  
Any cube can request to use another cube's resource.  
The owning cube accepts or rejects.

### Request packet
```
[TYPE: RESOURCE_REQ] [TARGET_UID: 16 bytes] [RESOURCE_TYPE: 1 byte] [AMOUNT: 4 bytes]
```

### Resource types (v0)
- `0x01` — RAM (amount in KB)
- `0x02` — CPU time (amount in % × 10)
- `0x03` — Storage (amount in KB)
- `0x04` — GPU (reserved)
- `0xFF` — Power (amount in mA)

---

## Disconnection

Two cases:

**Graceful disconnection** (cube is still powered, intentional removal):
1. Cube sends `GOODBYE [UID]` on all connected faces before powering down
2. Neighbors propagate `GOODBYE` through the mesh
3. Claimed resources released, routing tables updated

**Abrupt disconnection** (cable pulled, power loss — the common case):
The disconnected cube **cannot** send anything — it has no power. Instead:
1. Each neighbor detects signal loss on that face (timeout: 500ms of no heartbeat)
2. The neighbor that detects the loss originates `GOODBYE [lost_UID]` on all its other faces
3. Mesh propagates and recovers

**Heartbeat:** every cube sends a `HEARTBEAT [UID]` on each connected face every 100ms. Missing 5 consecutive heartbeats (500ms) triggers abrupt-disconnect handling.

**Goal:** zero crash on hot-unplug.

---

## Loop Prevention

In a mesh, packets can loop indefinitely (A→B→C→A). CubionLink uses a **TTL field** in every packet header:

```
[MAGIC] [TYPE] [TTL: 1 byte] [SRC_UID: 16 bytes] [DST_UID: 16 bytes] [PAYLOAD...]
```

- TTL starts at `64` on packet creation
- Each cube that forwards a packet decrements TTL by 1
- TTL = 0 → packet dropped, not forwarded
- Additionally: each cube keeps a **seen packet cache** (SRC_UID + packet sequence number, last 64 entries). Duplicate packets are dropped immediately.

---

## UID Security

The ID bus uses a read-only ROM (e.g. DS28E07 or equivalent). UIDs are 128-bit and assigned at manufacture. Since the ROM is passive and readable, UIDs can technically be cloned.

**v0 position:** UIDs are identifiers, not authentication tokens. Do not use UID alone to grant trust or access. For security-sensitive applications, authentication must happen at a higher layer (e.g. challenge-response over the data channel).

A future CFC version may include a cryptographic ID chip (e.g. ATECC608). This must remain backward compatible — a v0 cube without crypto must still be able to join a mesh, with the understanding that it is unverified.

---

## Multi-face Resource Arbitration

A cube connected on multiple faces may receive conflicting resource requests simultaneously (two cubes both requesting 80% CPU at the same time).

**Rule:** first valid `RESOURCE_REQ` received wins. The cube processes requests in arrival order per face. If total requested exceeds available, the cube responds `RESOURCE_NACK [AVAILABLE_AMOUNT]` with what's left.

Requesting cubes must handle `NACK` gracefully — retry with lower amount or find another cube.

No global arbitration layer exists. This is intentional — decentralized, no single point of failure.

---

## Timing Specification

| Event | Timeout |
|-------|---------|
| HELLO response | 200ms — if no response, face is considered unconnected |
| Heartbeat interval | 100ms |
| Abrupt disconnect detection | 500ms (5 missed heartbeats) |
| RESOURCE_REQ response | 100ms — if no response, treat as NACK |
| Power negotiation response | 50ms |

---

## CFC Version vs Protocol Version

These are independent versioning axes:

- **CFC version** = physical connector spec (hardware)
- **CubionLink version** = software protocol

A CFC v2 connector can run CubionLink v0. A CFC v1 connector can run CubionLink v3.

The `HELLO` packet declares both independently:
```
[MAGIC: 0x3141592653589793] [PI_DEPTH: 1 byte] [CFC_VERSION: 1 byte] [PROTOCOL_VERSION: 1 byte] [UID: 16 bytes] [RESOURCES: 8 bytes] [CHECKSUM: 2 bytes]
```

When two cubes connect, they negotiate down to the lowest common version on each axis independently.

---

*CubionLink v0 — Cubion Project — MIT License*