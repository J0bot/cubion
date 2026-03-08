# Contributing to Cubion

## The one rule above all rules

**Never propose a change that breaks backward compatibility with existing cubes.**

If your proposal requires a cube from 2027 to stop working — it gets rejected. Always.

---

## What you can contribute

### Before CFC v1.0 ratification (NOW)
- Everything is open for discussion
- Pin layout proposals → open an issue with `[CFC]` tag
- Protocol improvements → open an issue with `[PROTOCOL]` tag
- This is the only window to influence the frozen standard

### After CFC v1.0 ratification
- New cube designs (hardware)
- SDK improvements
- New cube types (examples)
- Documentation
- Protocol extensions (additive only — never breaking)

---

## How to submit a cube design

1. Fork the repo
2. Create `examples/your-cube-name/`
3. Include: schematic, BOM, firmware, photos
4. Open a PR — community reviews for CFC compliance
5. Merged → part of the official cube library

---

## CFC Compliance Checklist

Before submitting any cube design, verify:

- [ ] Face dimensions exactly 50×50mm
- [ ] Connector area exactly 30×30mm centered
- [ ] GND pins at 4 corners (mandatory)
- [ ] PWR at 5V primary
- [ ] ID bus implemented (ROM chip with cube UID)
- [ ] Rotationally symmetric (works in 0/90/180/270° rotation)
- [ ] ESD protection on all signal pins
- [ ] Tested against at least one other CFC-compliant cube

---

## Philosophy

Cubion is not a product. It's a standard.  
The goal is that in 500 years, someone finds an old cube and it still works.  
Build accordingly.
