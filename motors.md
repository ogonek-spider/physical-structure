I have 18 motors from hoverboard 7nm torque (at ~32 A; measured Kt_motor = 0.22 Nm/A).

Motor rotor is 3d printed from PLA:
- 100% infill
- printed at 45 degrees
- PETG is not stiff enough for the rotor

Motors have magnetic absolute encoder and current sensor. 
Weight of one motor is 1.5kg. Full joint weight about 5kg

## Planetary gearbox
Motors have 3d printed two stage planetary 1:36 gearbox.

### Measured output torque (1:36 gearbox output shaft)

| Condition      | Phase current | Output torque |
|----------------|---------------|---------------|
| Holding (stall)  | 15 A        | 98 Nm         |
| Lifting (dynamic)| 25 A        | ≥ 98 Nm       |

**Thermal limit: 15 A. 25 A melts the PLA rotor — do not use.**

Motor torque constant: Kt_motor = 98 / (15 × 36 × 0.81) = **0.22 Nm/A**

Stall torque vs current at 1:36:
- 15 A → 98 Nm  ← max safe operating current

General formula (any ratio N): T_out = N × 0.22 × I × 0.81


Gears printed from PA6-CF. Lubricated with thick grease https://smazka.ru/shop/mc-rubin/

Common params: module 2mm, 3 planets, helical 15°, depth 16mm

### 1:36
Stage ratio 6 per stage, 6² = 36

| Param  | Teeth |
|--------|-------|
| Sun    | 12    |
| Planet | 24    |
| Ring   | 60    |

### 1:100
Stage ratio 10 per stage, 10² = 100

| Param  | Teeth |
|--------|-------|
| Sun    | 8     |
| Planet | 32    |
| Ring   | 72    |

Check: Ring = Sun + 2×Planet → 72 = 8 + 64 ✓

## Components
### esp32 s3
### motor shield
ZH-X11H v2 (https://aliexpress.ru/item/1005009517426707.html?shpMethod=EMS_ZX_ZX_US&sku_id=12000049344648268&spm=a2g2w.productlist.search_results.4.6d185074fB6baZ)

controls BLDC motor with internal velocity pid

velocity is sent over PWM

### ACS712 current sensor 
FIXME: learn how to callibrate, not working correclty

Needed for torque monitoring and limiting

### mt6701 absolute encoder
Sets on planetary gearbox output

TODO:
* learn how to set offest (its hard to mount zero like in a spider model)
* learn how to not switch angle when we jump below zero or up 2*PI. Correct behaviour would be when below zero go negative degree, when more that 2 * PI go above

### twai transiever TJA1051T
Communicates with central esp32 (central firmware), central firmware communicates with ros2 over serial port

Uses two cables twai lo and twai hi

#### CAN topology
STAR with Central s3 in the center. Each leg has three motor connected in line

## PID

Angle is set by twai. Motor is controlled by velocity, PID calculate velocity

### FIXME
moves too fast if send big angle change will break itself

only save way to controll by angle sin wave now

switch to angle + velocity control, to let ros2 trajectory controller to say on which trajectory step we are know

### TODO
Convert to virtual spring
