I have 18 motors from hoverboard 7nm torque. 

Motor casing is 3d printed from petg

Motors have magnetic absolute encoder and current sensor. 
Weight of one motor is 1.5kg. Full joint weight about 5kg

## Planetary gearbox
Motors have 3d printed two stage planetary 1:36 gearbox (about 250nm torque). 
Gears printed from PA6-CF. Lubricated with thick grease https://smazka.ru/shop/mc-rubin/

Module 2mm
Number of planets 3
Helical 15 degrees
Depth 16mm
Ring 60 teeth
Sun 12 teeth
Planet 24 teeth

## Components
### esp32 s3
### motor shield
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