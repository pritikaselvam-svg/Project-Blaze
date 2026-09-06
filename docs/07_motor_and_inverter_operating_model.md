# 07. Motor + Inverter Operating Model

## 1. Purpose

This document converts electrical power available from the battery into usable motor torque and mechanical power for the two Formula Student Electric architectures.

We compare:

- **Architecture B:** two rear motors
- **Architecture C:** one front motor + two rear motors

The same vehicle-level constraints are retained:

- Battery: 130S4P Molicel P45B baseline
- Nominal pack voltage: 468 V
- Maximum total traction power: 80 kW
- Same vehicle, tyres, driver and chassis assumptions
- Architecture-specific motor count and torque distribution

---

## 2. Model Boundary

The model begins with DC power available from the battery and ends with motor mechanical output.

**Battery DC power → inverter DC input → motor electrical power → motor mechanical power → motor torque**

The vehicle longitudinal model will use the resulting wheel torque.

---

## 3. Motor Speed

Motor angular speed is obtained from motor rotational speed.

$\omega_i=\frac{2\pi n_i}{60}$

where $\omega_i$ is motor angular speed in rad/s and $n_i$ is motor speed in rpm.

Mechanical motor power is:

$P_{mech,i}=T_i\omega_i$

or, using rpm:

$P_{mech,i}[kW]=\frac{T_i n_i}{9550}$

---

## 4. Motor Torque-Speed Envelope

A first-order electric motor model uses two operating regions.

### 4.1 Constant-torque region

At low speed, the motor can provide approximately its maximum torque.

$T_i=T_{max,i}$

The corresponding mechanical power increases with speed:

$P_{mech,i}=T_{max,i}\omega_i$

### 4.2 Constant-power region

Above base speed, available torque decreases approximately inversely with speed.

$T_i=\frac{P_{max,i}}{\omega_i}$

Therefore $P_{mech,i}\approx P_{max,i}$.

---

## 5. Base Speed

The transition can be estimated from:

$\omega_{base}=\frac{P_{max}}{T_{max}}$

and:

$n_{base}=\frac{60P_{max}}{2\pi T_{max}}$

when power is expressed in watts.

This value will be calculated from the selected motor data rather than assumed arbitrarily.

---

## 6. Motor Efficiency

Motor efficiency is represented as:

$\eta_{motor,i}=\frac{P_{mech,i}}{P_{motor,elec,i}}$

Therefore:

$P_{motor,elec,i}=\frac{P_{mech,i}}{\eta_{motor,i}}$

A future higher-fidelity model can replace constant efficiency with an efficiency map indexed by torque and speed.

---

## 7. Inverter Model

Inverter efficiency is:

$\eta_{inv,i}=\frac{P_{motor,elec,i}}{P_{inv,dc,i}}$

Therefore:

$P_{inv,dc,i}=\frac{P_{motor,elec,i}}{\eta_{inv,i}}$

Inverter loss is:

$P_{inv,loss,i}=P_{inv,dc,i}-P_{motor,elec,i}$

---

## 8. Electrical Current

The DC current required by each inverter is:

$I_{inv,i}=\frac{P_{inv,dc,i}}{V_{dc}}$

The total battery current is approximately:

$I_{batt}=\sum_i I_{inv,i}+I_{aux}$

For the first traction-only model, auxiliary loads can be set to zero.

---

## 9. Total Power Constraint

For a fair architecture comparison:

$\sum_iP_{mech,i}\leq80\,kW$

The same vehicle-level power ceiling is applied to both architectures.

---

## 10. Architecture B

Architecture B has two rear motors.

Total mechanical power is:

$P_{mech,B}=P_{mech,RL}+P_{mech,RR}$

For symmetric straight-line operation:

$P_{mech,RL}=P_{mech,RR}=\frac{P_{mech,B}}{2}$

Rear axle torque is:

$T_{rear,B}=T_{RL}+T_{RR}$

The torque distribution can later become asymmetric for torque-vectoring studies.

---

## 11. Architecture C

Architecture C has one front motor and two rear motors.

Total mechanical power is:

$P_{mech,C}=P_{mech,F}+P_{mech,RL}+P_{mech,RR}$

For the first straight-line model, requested power is distributed according to motor capability while respecting the 80 kW total limit.

The front mechanical differential does not require a separate longitudinal calculation because both front wheels have approximately equal speed in a straight line.

---

## 12. Requested Torque vs Available Torque

The actual motor torque must satisfy all applicable limits:

$T_{actual,i}=\min(T_{requested,i},T_{motor,max,i},T_{power-limit,i},T_{current-limit,i})$

Regenerative-braking limits will be added when the braking model is introduced.

---

## 13. Gear Ratio and Wheel Torque

If a fixed reduction ratio $G_i$ is used:

$T_{wheel,i}=T_{motor,i}G_i\eta_{gear,i}$

Motor speed is:

$n_i=G_i n_{wheel}$

For wheel radius $r_w$ and vehicle speed $v$:

$\omega_{wheel}=\frac{v}{r_w}$

Therefore:

$\omega_i=G_i\frac{v}{r_w}$

This is the connection between the motor model and vehicle dynamics.

---

## 14. Motor and Inverter Heat

Motor heat is initially:

$P_{motor,heat}=P_{motor,elec}-P_{mech}$

Inverter heat is:

$P_{inv,heat}=P_{inv,dc}-P_{motor,elec}$

These values are passed to the thermal model.

---

## 15. Numerical Implementation Sequence

At each timestep:

1. Read vehicle speed.
2. Convert vehicle speed to wheel speed.
3. Convert wheel speed to motor speed.
4. Determine available motor torque.
5. Apply requested torque distribution.
6. Calculate motor mechanical power.
7. Apply motor efficiency.
8. Calculate inverter DC power.
9. Calculate battery current.
10. Pass battery current into the battery model.
11. Pass motor and inverter heat into the thermal model.
12. Return available wheel torque to the vehicle dynamics model.

---

## 16. Outputs

The model will produce numerical values for:

- Motor rpm
- Motor torque
- Motor mechanical power
- Motor electrical power
- Inverter DC power
- Inverter current
- Motor efficiency
- Inverter efficiency
- Motor heat
- Inverter heat
- Total traction power

---

## 17. Model Limitations

The first version is deliberately reduced-order.

It does not yet claim:

- manufacturer-validated efficiency maps
- exact inverter switching losses
- exact phase-current limits
- exact thermal derating thresholds
- exact magnetic saturation
- exact tyre slip
- exact transient motor dynamics

These can be added after the baseline simulation works.

The priority is a transparent model that can be executed, checked and improved.
