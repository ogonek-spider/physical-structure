L1 = 0.25 (coxa)
L2 = 0.35 (femur)
L3 = 0.80 (tibia)

# Nominal pose
Femur angle α = 60° above horizontal (steeply raised, spider-like)
Tibia angle β = 60° below horizontal (30° from vertical, not vertical)
Body height H = 390mm
Foot reach = 575mm from coxa joint
Knee interior angle = 120°

# Rationale

## Torque analysis (corrected)

At 127 kg robot, wave gait (5 legs), 1.2× dynamic factor → F_eff = 299 N per leg.

GRF and limb sub-assembly gravity act in **opposite directions** about the femur joint.
The formula `F_eff × (L2·cosα + L3·cosβ)` counts only GRF — it is conservative by ~33%.

### Femur joint (stance)
```
GRF torque:         299 × 0.575 = 172 Nm  (CCW — collapses leg)
Limb gravity torque:               43 Nm  (CW  — opposes GRF, helps motor)
Net motor torque:   172 − 43    = 129 Nm
```
Limb gravity breakdown (horizontal distances from femur pivot):
- femur tube 0.70 kg × 0.0875 m = 0.6 Nm
- knee joint 5 kg   × 0.175  m = 8.6 Nm
- tibia tube 1.60 kg × 0.375 m = 5.9 Nm
- ankle joint 5 kg  × 0.575  m = 28.2 Nm → total 43.3 Nm

### Tibia joint (knee, stance)
```
GRF torque:         299 × 0.40 = 120 Nm
Limb gravity torque:              23 Nm  (opposes GRF)
Net motor torque:   120 − 23  =  97 Nm
```

### Safety margins (gearbox efficiency 90% per stage = 204 Nm available)
| Joint | Required | Available | Safety |
|-------|----------|-----------|--------|
| Femur | 129 Nm   | 204 Nm    | 1.58×  |
| Tibia |  97 Nm   | 204 Nm    | 2.10×  |
| Coxa  | ~15 Nm   | 204 Nm    | >>1    |

Swing phase: femur 43 Nm, tibia 23 Nm (trivial).

### Key risks
- **Motor thermals**: stance demands ~51% of stall torque continuously. ACS712 current limiting must be calibrated.
- **Battery runtime**: 57–114 min per 2-pack charge. 3–4 h sessions require ~4–6 packs or rotation plan.
- **Sun gear fatigue**: contact stress ~83 MPa vs 75 MPa limit — marginal, carry spares.

Previous values L2=0.669, L3=0.919 exceeded gearbox torque limit (311 Nm calculated).

Limbs from aluminum profile 40x60x4mm

Joint made with 3d printer PETG frame (5 walls 30% gyroid) with two 61910 bearings.

Frame thickness 10mm
Bearing distance 100mm

Motor is mounter on external wall of frame. Motor is connecter to alluminum profile with 4 8mm bolts