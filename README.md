# Cubion ⬛

> **One standard. Infinite scale. Forever.**

Cubion is an open-source modular computing standard built around a single idea:  
**every cube must work with every other cube — now, and in 10'000 years.**

---

## What is a Cubion?

A Cubion is a self-contained computing cube with:
- A CPU, RAM, and storage inside (or specialized: GPU cube, sensor cube, screen cube...)
- **6 universal connector faces** — power + data on every face
- Magnetic attachment with electrical contacts
- A standard protocol so any cube talks to any other cube

Stack cubes together → resources are shared automatically.  
More cubes = more power, distributed and decentralized.

```
[CPU cube] + [RAM cube] + [GPU cube] = one system
[sensor cube] + [logic cube] + [motor cube] = one robot
[cube] + [cube] + ... = anything
```

---

## Core Philosophy

### 1. Eternal Backward Compatibility
A cube made in 2027 **must** work with a cube made in 2127.  
The **Cubion Face Connector (CFC) standard** is frozen once ratified.  
Internal hardware evolves. The interface never breaks.

This is the #1 rule. No exceptions.

### 2. Open Source Forever
MIT License. No exceptions. No "community edition" vs "pro".  
Anyone can build cubes. Anyone can sell cubes. The standard stays free.

### 3. Physical = Logical
When you connect two cubes physically, they connect logically.  
No configuration. No drivers. No setup.  
Plug → works.

### 4. Modularity by Design
Every cube is complete on its own.  
Every cube is better with others.  
There is no "main" cube.

### 5. Distributed Energy
Every cube contains a micro-battery (enough to maintain its clock and identity).  
Dedicated battery cubes feed the mesh via CFC faces.  
One power plug on any cube charges the entire network.

### 6. Security as a Foundation
End-to-end encryption is native to CubionLink — not optional.  
Your personal cube holds your cryptographic identity (GPG key).  
If you have your cube, you can read your data. Nobody else can.  
The security standard is designed to be upgradeable without breaking compatibility — algorithms age, the negotiation mechanism doesn't.

---

## The Cubion Face Connector (CFC)

The CFC is the soul of the project. It defines:
- **Physical dimensions** — connector layout on a 50×50mm face
- **Pin assignments** — power rails, data lines, identification
- **Protocol** — how cubes discover each other and share resources
- **Versioning rules** — how future versions stay backward compatible

> See [`hardware/connector-spec/CFC-v0.1.md`](hardware/connector-spec/CFC-v0.1.md)

---

## CubionLink Protocol

CubionLink is the software protocol that runs over CFC. It handles:
- **Discovery** — cubes detect each other on connection, exchange capabilities
- **Routing** — data finds its way through the mesh without any central router
- **Resource sharing** — CPU, RAM, GPU, storage, power advertised and requested across the mesh
- **Hot-plug** — graceful and abrupt disconnection handled cleanly

Every CubionLink session opens with a `HELLO` packet whose magic number is π:
```
MAGIC = 0x3141592653589793
```
One additional digit of π is verified per year. The network gets more precise over time — exactly like π itself.

> See [`software/protocol/CubionLink-v0.md`](software/protocol/CubionLink-v0.md)

---

## Repository Structure

```
cubion/
├── hardware/
│   ├── connector-spec/     # CFC standard — the frozen interface
│   └── cube-base/          # Reference PCB design for a base cube
├── software/
│   ├── protocol/           # CubionLink protocol (discovery, routing, power)
│   └── sdk/                # SDK to build cube firmware / software
├── examples/               # Example cube builds
├── docs/
│   ├── VISION.md           # Full ecosystem catalog & convergence vision
│   ├── TEAM.md             # Roles, how to join
│   ├── PHILOSOPHY.md       # Moral commitments
│   └── CONTRIBUTING.md     # Contribution rules
└── README.md
```

---

## Prototype: Cube v0.1

- **Size:** 50×50×50mm
- **Core:** ESP32-S3 (chosen for v0.1 — low cost, Wi-Fi, enough RAM to run CubionLink)
- **Faces:** 6× CFC connector (magnetic + electrical contacts)
- **Power:** shared across faces, any cube can be power source
- **Protocol:** CubionLink v0 over RS-485 (one bus per face)

> A future **Cubion Compute Module (CCM)** standard will define a hot-swappable internal compute board, allowing the internals of a cube to be upgraded without replacing the shell or connectors. CCM is not part of v0.1.

---

## Roadmap

| Milestone | Description |
|-----------|-------------|
| M0 | CFC v0.1 spec published — connector standard frozen |
| M1 | First physical prototype (5×5×5cm, 6 faces wired) |
| M2 | CubionLink protocol v0 (discovery + resource sharing) |
| M3 | SDK v0 (firmware base, face detection, routing) |
| M4 | First cube types: compute, sensor, display, power |
| M5 | Community cube submission standard |

---

## Philosophy

Cubion is built on moral commitments that sit above the license.

- The CFC standard belongs to everyone. No company, no foundation, no individual can own it.
- Cubion will never be forked into a closed version. If someone does it, they've left the project.
- A cube from 2027 must work with a cube from 2127. Backward compatibility is not a feature — it's the contract.
- Privacy is default. Cubes do not phone home. Ever.

> See [`docs/PHILOSOPHY.md`](docs/PHILOSOPHY.md) for the full moral commitments.

---

## License

MIT — because the goal is **maximum spread**.

Companies can build proprietary cubes. Sell them. Make money.  
But the CFC standard stays open — so their cubes must be compatible with everyone else's.  
The standard wins regardless of what anyone builds on top of it.

> See [`LICENSE`](LICENSE)

---

## Contributing

The standard is open for discussion before v1.0 ratification.  
After v1.0 — the CFC connector spec is **frozen**.  
All other parts of the project welcome contributions.

> See [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md)
> See [`docs/TEAM.md`](docs/TEAM.md) if you want to contribute or join the project.


---

## Vision & Ecosystem

Cubion is the physical layer of the [Xerboxion](https://github.com/j0bot) ecosystem.  
Each cube is a B0XI0N node. The CubionLink protocol is the XI0N layer.  
The long-term goal: a single cube that contains everything — and a human (a XERI0N) who just thinks, creates, and lives.

> See [`docs/VISION.md`](docs/VISION.md) for the full ecosystem catalog and convergence vision.  

---

*Cubion is part of the [Xerboxion](https://github.com/j0bot) ecosystem.*