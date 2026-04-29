# Saved Decisions

## Femur Joint Concept

- Use a double-supported femur pivot, not a femur mounted directly on gearbox output.
- Motor and gearbox stay on the fixed body/coxa bracket.
- Femur load path should be: ground -> leg -> 40x60 tube -> joint axle -> bearings -> body.
- Use a steel joint axle with two widely spaced bearings so the gearbox does not take bending load.
- Gearbox should only provide torque to the joint axle through a coupling or hub.
- Attach the 40x60 aluminum tube to a femur root block inserted into the tube.
- Use through-bolts with internal crush spacers inside the tube.
- Add hard mechanical stops with rubber bumpers.
- Prefer encoder measurement on the real joint axle, not only inside the gearbox.

## Style Constraints

- Keep the femur joint visually natural and organic.
- Avoid a vertical tibia posture.
- Hide the structural metal bracket inside a printed outer shell when possible.

## Robot Mass

Total robot mass: ~122 kg

- 6 legs × 17.8 kg = 107 kg (joints 15 kg + femur 0.70 kg + tibia 1.60 kg + coxa 0.5 kg)
- Body: 2× plywood 780×640×12mm = 7.8 kg
- 2× Xiaomi M365 Pro batteries (36V 12.8Ah 474Wh each) = 5.8 kg
- Electronics: ~1 kg

Aluminum profile 40×60×4mm: 736mm² cross-section, ~2.0 kg/m

Wave gait 5 legs supporting → 240 N per leg static, 288 N with 1.2× dynamic factor.

Power: 2× M365 Pro in parallel (single pack BMS ~1080W, robot peaks ~2000W). Runtime ~57–114 min.

## Limb Geometry

- L1 = 0.25m (coxa), L2 = 0.35m (femur), L3 = 0.80m (tibia)
- Nominal pose: α=60° femur above horizontal, β=60° tibia below horizontal
- Body height H = 390mm, foot reach = 575mm from coxa, knee interior angle = 120°

### Corrected torque model
GRF and limb gravity act in **opposite directions** at femur joint — the formula
`F_eff × (L2·cosα + L3·cosβ)` overstates load by ~33% (GRF-only, ignores gravity relief).

Actual motor torques (dynamic, 90% gearbox efficiency per stage → 204 Nm available):
| Joint | Required | Safety |
|-------|----------|--------|
| Femur | 129 Nm   | 1.58×  |
| Tibia |  97 Nm   | 2.10×  |
| Coxa  | ~15 Nm   | >>1    |

If limb lengths are revisited: verify femur torque = 299×(L2·cosα + L3·cosβ) − 43 Nm stays ≤ 200 Nm.

## Gearbox Analysis

Two-stage planetary 36:1 (6×6), module 2mm, helix 15°, face width 16mm, Z_s=12/Z_p=24/Z_r=60, 3 planets, PA6-CF gears.

At actual 129 Nm femur output (not 172 Nm):
- Sun tooth bending: ~69 MPa → static safety 2.0× (PA6-CF UTS ~140 MPa)
- Contact stress: ~83 MPa → fatigue safety ~0.9× (lubricated limit ~75 MPa) — still marginal

For art project duration (tens of hours) with grease: should survive.
Sun gear (12 teeth) is the weak point — carry spares, inspect after sessions.

## Battery Warning
2× M365 Pro gives 57–114 min per charge at 500–1000 W.
3–4 h outdoor sessions require ~4–6 packs or a charging rotation. This is the tightest constraint.

Grease: MC Rubin EP2 is compatible with PA6-CF. Keep away from PETG housing.
