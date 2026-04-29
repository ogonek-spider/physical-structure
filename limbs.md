L1 = 0.25 (coxa)
L2 = 0.35 (femur)
L3 = 0.80 (tibia)

# Nominal pose
Femur angle α = 30° below horizontal (femur drops outward, knee below body)
Tibia angle β = 65° below horizontal (25° from vertical, not vertical)
Body height H = 900mm
Foot reach = 641mm from coxa joint
Knee interior angle = 145°

# Rationale

## Torque analysis (corrected)

At 127 kg robot, wave gait (5 legs), 1.2× dynamic factor → F_eff = 299 N per leg.

GRF and limb sub-assembly gravity act in **opposite directions** about the femur joint.
The formula `F_eff × (L2·cosα + L3·cosβ)` counts only GRF — it is conservative by ~33%.

With femur DOWN, GRF (upward at foot) still opposes limb gravity (downward at positive-x positions)
about the femur joint — motor assistance direction unchanged.

### Femur joint (stance)
```
GRF torque:         299 × 0.641 = 192 Nm  (CCW — collapses leg)
Limb gravity torque:               55 Nm  (CW  — opposes GRF, helps motor)
Net motor torque:   192 − 55    = 137 Nm
```
Limb gravity breakdown (horizontal distances from femur pivot, α=30°, β=65°):
- femur tube 0.70 kg × 0.152 m =  1.0 Nm
- knee joint 5 kg   × 0.303 m = 14.9 Nm
- tibia tube 1.60 kg × 0.472 m =  7.4 Nm
- ankle joint 5 kg  × 0.641 m = 31.4 Nm → total 54.7 Nm

### Tibia joint (knee, stance)
```
GRF torque:         299 × 0.338 = 101 Nm
Limb gravity torque:               19 Nm  (opposes GRF)
Net motor torque:   101 − 19   =  82 Nm
```

### Safety margins (gearbox efficiency 90% per stage = 204 Nm available)
| Joint | Required | Available | Safety |
|-------|----------|-----------|--------|
| Femur | 137 Nm   | 204 Nm    | 1.49×  |
| Tibia |  82 Nm   | 204 Nm    | 2.49×  |
| Coxa  | ~15 Nm   | 204 Nm    | >>1    |

Swing phase: femur 55 Nm, tibia 19 Nm (trivial).

### Key risks
- **Motor thermals**: stance demands ~51% of stall torque continuously. ACS712 current limiting must be calibrated.
- **Battery runtime**: 57–114 min per 2-pack charge. 3–4 h sessions require ~4–6 packs or rotation plan.
- **Sun gear fatigue**: contact stress ~83 MPa vs 75 MPa limit — marginal, carry spares.

Previous values L2=0.669, L3=0.919 exceeded gearbox torque limit (311 Nm calculated).

Limbs from aluminum profile 40×20×3mm, material АД31т (AA6063-T5)
- 40mm vertical, 20mm horizontal, 3mm wall
- Yield σ₀.₂ = 145 MPa, ultimate = 175 MPa

Section properties (40mm vertical):
- Area: 324 mm²  (40×20 − 34×14)
- I_x strong axis: 60,812 mm⁴ → W_x = 3,041 mm³
- I_y weak axis:   18,892 mm⁴ → W_y = 1,889 mm³
- Euler buckling (tibia 800mm, pin-pin): P_cr = 20,500 N >> actual ~300 N
- Weight: 875 g/m  (femur 0.35m → 306g tube-only, tibia 0.80m → 700g tube-only)

Orient 40mm face vertical to maximise bending resistance against stance loads.

Joint made with 3d printer PETG frame (5 walls 30% gyroid) with two 61910 bearings.

Frame thickness 10mm
Bearing distance 100mm

Motor is mounted on external wall of frame. Motor connects to aluminum profile with 4×M6 grade 8.8 bolts through the 40mm face into planetary gearbox output flange (50mm diameter).

Bolt pattern: 26×24mm rectangle (±13mm along 40mm, ±12mm along profile length), r = 17.7mm
Bearing area per bolt: 6×3×2 walls = 36 mm²

Bolt bearing analysis (АД31т, bearing allowable ~109 MPa):
- Nominal (137 Nm): 137,000/(4×17.7)/36 = 54 MPa   ✓
- Max    (204 Nm): 204,000/(4×17.7)/36 = 80 MPa   ✓
- Use steel nuts, no PETG threads at bolt holes