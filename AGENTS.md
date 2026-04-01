I'm making art project big hexapod robot. You are my engineering advisor

I want natural, not engineering look, no vertical tibia, 

## Memory

At the start of every session, read [MEMORY.md](MEMORY.md) and all files it links to.

@motors.md

@limbs.md

My budget is limited for 3000 dollars (excluding motors and drivers)

Spider would be walking outdoors on a field in Russia near Moscow in august

# Equipment

* two neptune4 max printers
* k1 max printer with heated to 45 degrees chamber

# Femur Geometry (from STL analysis)

| Property | Value |
|----------|-------|
| Overall length | 340mm (Z axis) |
| End connectors (z=0–40, z=300–340) | 136.65mm × 102mm |
| Midsection (z=40–300) | 60mm × 102mm, 260mm long |
| Print settings | 3-5 walls, 15-30% gyroid infill, PETG |

# Decisions

| Decision | Rationale |
|----------|-----------|
| 40mm OD aluminum tube inside each limb | Structural reinforcement — PETG shell alone insufficient for bending loads |
| Bore 40.5-41mm ID in PETG for slide-in fit | Epoxy or set screws to lock tube at ends |
| Reduce midsection infill around tube | Tube carries bending; PETG holds shape and transfers load at joints |
| Export ASCII STL from Onshape for analysis | STEP too hard to parse; screenshots or STL preferred |

# Next Steps

- Design tube-to-joint interface at end connectors (flange, sleeve, or bolted collar)
- Modify Onshape femur model: add 40mm bore through midsection
- Consider clamshell split for midsection printing (easier tube insertion)
- Calculate required tube wall thickness (2mm wall 6061-T6 likely sufficient)
