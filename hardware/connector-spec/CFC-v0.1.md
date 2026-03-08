# Cubion Face Connector (CFC) — Specification v0.1 DRAFT

> This document defines the physical and electrical interface between Cubion cubes.  
> Once ratified as v1.0, the connector pinout and mechanical dimensions are **permanently frozen**.

---

## Status: DRAFT — open for community input before ratification

---

## 1. Mechanical

| Parameter | Value |
|-----------|-------|
| Face size | 50 × 50 mm (prototype) |
| Connector area | 30 × 30 mm centered on face |
| Attachment | Magnetic (N52 neodymium, 4 corners) |
| Orientation | Symmetric — any rotation works (0°, 90°, 180°, 270°) |
| Mating cycles | ≥ 10,000 |

The connector must be **rotationally symmetric** — a cube can be connected in any of 4 rotations on any face and it must work identically.

---

## 2. Pin Layout (30×30mm grid, 5×5 = 25 pins)

```
[ GND ] [ PWR ] [ DAT ] [ PWR ] [ GND ]
[ PWR ] [ ID  ] [ TX  ] [ ID  ] [ PWR ]
[ DAT ] [ RX  ] [ GND ] [ TX  ] [ DAT ]
[ PWR ] [ ID  ] [ RX  ] [ ID  ] [ PWR ]
[ GND ] [ PWR ] [ DAT ] [ PWR ] [ GND ]
```

| Pin type | Count | Description |
|----------|-------|-------------|
| GND | 4 | Ground — one per corner |
| PWR | 8 | Power rail — bidirectional, any cube can source |
| DAT | 4 | High-speed differential pairs (2 pairs = 1 full-duplex lane) |
| TX | 2 | CubionLink serial transmit |
| RX | 2 | CubionLink serial receive |
| ID | 4 | Cube identification bus — read-only ROM |
| GND (center) | 1 | Center ground reference |
| **Total** | **25** | |

**Rotation invariance:** The grid is 4-fold rotationally symmetric. At 0° and 180°, TX→TX and RX→RX. At 90° and 270°, TX and RX swap — this is handled in firmware by the discovery handshake, which detects and corrects polarity automatically. Both cubes attempt to transmit simultaneously on `HELLO`; whichever receives a valid signal on RX knows its orientation and adjusts.

**DAT pairs:** Each pair is a differential line (D+/D−). Protocol over DAT is defined in future CFC versions. In v0, DAT pins are reserved — do not connect internally, do not drive.

---

## 3. Electrical

| Parameter | Value |
|-----------|-------|
| Power voltage | 5V (default) or 12V (negotiated) |
| Max current per face | 3A |
| Total cube power budget | 18A across all 6 faces (3A × 6) |
| Data voltage | 3.3V logic |
| ESD protection | Required on all signal pins |

### Power negotiation

5V and 12V are **never on the same pins simultaneously**. The default state is always 5V. A cube that wants 12V must:

1. Detect neighbor via ID bus (ID bus runs at 3.3V regardless of power state)
2. Send a power negotiation request over TX/RX: `[PWR_REQ] [VOLTAGE: 0x0C] [CURRENT_MA: 2 bytes]`
3. Neighbor responds `[PWR_ACK]` or `[PWR_NACK]`
4. Only after `PWR_ACK` does the sourcing cube switch to 12V

A cube that does not support 12V must respond `PWR_NACK`. A v0 cube receiving an unknown `PWR_REQ` must respond `PWR_NACK` and stay at 5V. This ensures safe backward compatibility.

### ESD and hot-plug

All signal pins must tolerate hot-plug events. GND pins have no special physical priority on a flat connector — ESD protection on signal pins handles transients during connection.

---

## 4. CubionLink Protocol (v0 summary)

Full spec: [`software/protocol/CubionLink-v0.md`](../../software/protocol/CubionLink-v0.md)

### Discovery
1. Cube powers on → broadcasts ID on all connected faces
2. Neighbor receives ID → responds with own ID + resource table
3. Mesh forms automatically — no central authority

### Resource sharing
- Each cube exposes: CPU cores, RAM, storage, GPU, sensors
- Resources are claimed by the mesh, allocated dynamically
- A cube removed from the mesh releases its resources gracefully

### Versioning rule
> A CFC v(N+1) face **must** be able to connect to a CFC v(N) face with full basic functionality.  
> Advanced features of v(N+1) are available only when both sides support it.  
> This rule is mandatory for all future versions.

---

## 5. What is NEVER allowed to change in future versions

- The 50×50mm outer face dimension for CFC Standard
- The 30×30mm connector area center position
- The 4-corner GND pin positions
- The PWR rail default voltage (5V)
- The ID bus — a 2027 cube must always be identifiable by a 2127 cube

Everything else (data protocol speed, additional pins in reserved areas, power negotiation extensions) can evolve — as long as the above is preserved.

### On miniaturization

Internal components of a cube can be miniaturized freely — smaller PCBs, denser chips, less power consumption. The 50×50mm face is about the **interface**, not the internals.

For physically smaller cubes (e.g. a future 20×20mm cube), a separate **CFC Mini** spec must be defined with its own frozen dimensions. CFC Mini cubes must include a backward-compatible mode or a passive adapter to connect with CFC Standard cubes, potentially with reduced bandwidth. Reduced bandwidth is acceptable — broken compatibility is not.

---

## 6. Reserved areas

The outer ring of each face (50×50 minus 30×30) is reserved for:
- Future antenna integration
- Optical data (future version)
- Thermal management contacts
- Custom maker extensions (non-standard, clearly marked)

---

## Ratification process

1. Community review period: open GitHub issues + discussions
2. Reference implementation must pass test suite
3. 3 independent cube makers must validate interoperability
4. v1.0 ratified → spec frozen → no breaking changes ever

---

*CFC Specification — Cubion Project — MIT License*
