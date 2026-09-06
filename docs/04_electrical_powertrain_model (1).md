# 04 — Electrical Powertrain Model

## 1. Purpose

This model compares the electrical behaviour of two Formula Student Electric powertrain architectures under the same vehicle-level constraints.

### Architecture B
- Two rear motors
- Rear-wheel drive
- Independent rear motor torque control
- Two motor/inverter power paths

### Architecture C
- One front motor
- Two rear motors
- Hybrid all-wheel drive
- Front mechanical differential
- Three motor/inverter power paths

The purpose is to quantify the electrical consequences of the architecture choice, not to assume that one architecture is inherently better.

---

## 2. Model boundary

The electrical model begins at the traction battery DC terminals and ends at the mechanical output of each traction motor.

```text
Battery
   ↓
HV bus
   ↓
Motor inverter/controller
   ↓
Motor
   ↓
Mechanical shaft
   ↓
Differential / final drive
   ↓
Wheel
```

The model tracks:

- battery voltage
- battery current
- inverter input power
- inverter losses
- motor electrical power
- motor losses
- motor mechanical power
- HV cable losses
- auxiliary electrical demand
- total electrical efficiency

SOC, cell temperature and operating duration are inputs/constraints here. Their full dynamic evolution is handled in the battery and thermal models.

---

# 3. Fixed comparison assumptions

| Parameter | Architecture B | Architecture C |
|---|---:|---:|
| Total vehicle mass | 214.5 kg | 240.0 kg |
| Battery | 130S4P P45B | 130S4P P45B |
| Nominal battery voltage | 468 V | 468 V |
| Nominal battery capacity | 18 Ah | 18 Ah |
| Nominal battery energy | 8.424 kWh | 8.424 kWh |
| Maximum total TS power | 80 kW | 80 kW |
| Number of traction motors | 2 | 3 |
| Drive layout | Rear | Hybrid AWD |

The 80 kW value is treated as the total traction-system power ceiling for the comparative study. Exact competition-rule interpretation must be checked against the specific ruleset before claiming regulatory compliance.

---

# 4. Battery electrical quantities

For a 130S4P pack:

$$
N_{series}=130
$$

$$
N_{parallel}=4
$$

Nominal cell voltage:

$$
V_{cell,nom}=3.6\text{ V}
$$

Therefore:

$$
V_{pack,nom}=N_{series}V_{cell,nom}
$$

$$
V_{pack,nom}=130(3.6)=468\text{ V}
$$

Cell capacity:

$$
Q_{cell}=4.5\text{ Ah}
$$

Pack capacity:

$$
Q_{pack}=N_{parallel}Q_{cell}
$$

$$
Q_{pack}=4(4.5)=18\text{ Ah}
$$

Nominal energy:

$$
E_{pack}=V_{pack,nom}Q_{pack}
$$

$$
E_{pack}=468(18)=8424\text{ Wh}
$$

$$
\boxed{E_{pack}=8.424\text{ kWh}}
$$

---

# 5. Battery current at the 80 kW limit

Battery-side power is:

$$
P_{batt}=V_{batt}I_{batt}
$$

Therefore:

$$
I_{batt}=\frac{P_{batt}}{V_{batt}}
$$

At nominal voltage and 80 kW:

$$
I_{batt}=\frac{80000}{468}
$$

$$
\boxed{I_{batt}\approx171.0\text{ A}}
$$

With four cells in parallel:

$$
I_{cell}=\frac{I_{batt}}{N_{parallel}}
$$

$$
I_{cell}=\frac{171.0}{4}
$$

$$
\boxed{I_{cell}\approx42.75\text{ A}}
$$

This is below the stated 45 A continuous discharge rating for the P45B under its specified test conditions, but it is not an automatic thermal or endurance pass. Current margin depends on SOC, cell temperature, duration, impedance and cooling.

---

# 6. Total power flow

For a generic motor path:

$$
P_{batt,i}
\rightarrow
P_{inv,i}
\rightarrow
P_{motor,elec,i}
\rightarrow
P_{motor,mech,i}
$$

The battery must supply:

$$
P_{batt,total}$$
=
$$\sum_i P_{batt,i}$$
+
$$P_{aux}
$$

where $P_{aux}$ includes non-traction electrical loads such as VCU, sensors, pumps, fans, instrumentation and low-voltage conversion losses.

---

# 7. Motor mechanical power

For each motor:

$$
P_{mech,i}=T_i\omega_i


where $T_i$ is motor torque in N·m and $\omega_i$ is angular speed in rad/s.

Motor speed conversion:

$$
\omega_i=\frac{2\pi n_i}{60}
$$

where $n_i$ is motor speed in rpm.

Therefore:

$$
P_{mech,i}$$
=
$$T_i\frac{2\pi n_i}{60}
$$

If power is expressed in kW:

$$
P_{mech,i}[kW]$$
=
$$\frac{T_i n_i}{9550}
$$

---

# 8. Motor electrical efficiency

Define:

$$
\eta_{motor,i}
=
\frac{P_{mech,i}}{P_{motor,elec,i}}
$$

Therefore:

$$
P_{motor,elec,i}
=
\frac{P_{mech,i}}{\eta_{motor,i}}
$$

Motor loss:

$$
P_{motor,loss,i}
=
P_{motor,elec,i}-P_{mech,i}
$$

or:

$$
P_{motor,loss,i}
=
P_{mech,i}
\left(
\frac{1}{\eta_{motor,i}}-1
\right)
$$

For a research-grade implementation, $\eta_{motor}$ should eventually be replaced by a torque-speed efficiency map.

---

# 9. Inverter efficiency

Define:

$$
\eta_{inv,i}
=
\frac{P_{motor,elec,i}}{P_{inv,dc,i}}
$$

Therefore:

$$
P_{inv,dc,i}
=
\frac{P_{motor,elec,i}}{\eta_{inv,i}}
$$

Inverter loss:

$$
P_{inv,loss,i}
=
P_{inv,dc,i}-P_{motor,elec,i}
$$

or:

$$
P_{inv,loss,i}
=
P_{motor,elec,i}
\left(
\frac{1}{\eta_{inv,i}}-1
\right)
$$

A constant efficiency may be used for the first-order model. A measured or manufacturer-provided efficiency map should replace it for final validation.

---

# 10. HV cable losses

For a cable path with resistance $R_{cable,i}$:

$$
P_{cable,loss,i}=I_i^2R_{cable,i}
$$

Voltage drop:

$$
\Delta V_{cable,i}=I_iR_{cable,i}
$$

and:

$$
V_{load,i}=V_{battery}-\Delta V_{cable,i}
$$

The resistance should represent the complete electrical path being modelled, including relevant conductor length and connection resistance.

---

# 11. Architecture B electrical model

Architecture B has two traction paths:

```text
                 Battery
                    │
              HV DC bus
               ┌────┴────┐
               ↓         ↓
          Inverter L  Inverter R
               ↓         ↓
            Motor L    Motor R
               ↓         ↓
            Rear wheels
```

Total mechanical power:

$$
P_{mech,B}
=
P_{mech,RL}+P_{mech,RR}
$$

Total battery-side traction power:

$$
P_{batt,B}
=
\sum_{i=RL,RR}
\frac{P_{mech,i}}
{\eta_{motor,i}\eta_{inv,i}}
+
P_{cable,loss,B}
+
P_{aux}
$$

Battery current:

$$
I_{batt,B}
=
\frac{P_{batt,B}}{V_{batt}}
$$

---

# 12. Architecture C electrical model

Architecture C has three traction paths:

```text
                    Battery
                       │
                 HV DC bus
              ┌────────┼────────┐
              ↓        ↓        ↓
          Front inv  Rear inv L Rear inv R
              ↓        ↓        ↓
          Front motor Rear motor L Rear motor R
              ↓        ↓        ↓
          Front axle   Rear axle
```

Total mechanical power:

$$
P_{mech,C}
=
P_{mech,F}
+
P_{mech,RL}
+
P_{mech,RR}
$$

Total battery-side traction power:

$$
P_{batt,C}
=
\sum_{i=F,RL,RR}
\frac{P_{mech,i}}
{\eta_{motor,i}\eta_{inv,i}}
+
P_{cable,loss,C}
+
P_{aux}
$$

Battery current:

$$
I_{batt,C}
=
\frac{P_{batt,C}}{V_{batt}}
$$

---

# 13. Equal-power comparison

To make the architecture comparison fair, total requested traction power is constrained:

$$
P_{mech,total}\leq80\text{ kW}
$$

For Architecture B:

$$
P_{RL}+P_{RR}=P_{mech,total}
$$

For Architecture C:

$$
P_F+P_{RL}+P_{RR}=P_{mech,total}
$$

This allows different power distributions without changing the total power ceiling.

---

# 14. Electrical efficiency

Overall traction efficiency:

$$
\eta_{total}
=
\frac{P_{mech,total}}
{P_{batt,total}}
$$

Total electrical loss:

$$
P_{loss,total}
=
P_{batt,total}-P_{mech,total}
$$

Expanded:

$$
P_{loss,total}
=
P_{motor,loss}
+
P_{inv,loss}
+
P_{cable,loss}
+
P_{aux}
$$

---

# 15. Architecture effect

Architecture C introduces an additional motor/inverter path.

This does not automatically mean C is less efficient.

The additional path creates potential additional conversion losses, but it may allow:

- better wheel-torque utilization
- lower torque demand per motor
- improved traction
- better acceleration
- different operating points on motor efficiency maps

Therefore efficiency must be calculated from the actual operating point rather than inferred from motor count alone.

---

# 16. Battery terminal voltage and voltage sag

The first-order model may use a fixed battery voltage.

The refined model uses:

$$
V_{terminal}
=
V_{OCV}(SOC,T)
-
I_{batt}R_{pack}(SOC,T)
$$

where:

- $V_{OCV}$ = open-circuit voltage
- $R_{pack}$ = effective pack resistance
- $SOC$ = state of charge
- $T$ = cell temperature

The electrical model should therefore be iterated:

$$
P_{batt}
\rightarrow
I_{batt}
\rightarrow
V_{terminal}
\rightarrow
P_{batt}
$$

until the operating point converges.

---

# 17. Power and current constraints

At every simulation step:

$$
P_{batt}=V_{terminal}I_{batt}
$$

The controller must satisfy:

$$
P_{traction}\leq80\text{ kW}
$$

The model should also check:

$$
I_{cell}\leq I_{cell,max}
$$

and:

$$
T_{cell}\leq T_{cell,max}
$$

The current and temperature limits are not interchangeable.

---

# 18. SOC input

SOC is updated using battery current:

$$
SOC_{k+1}
=
SOC_k
-
\frac{I_{batt,k}\Delta t}{Q_{pack}3600}
$$

for the selected discharge sign convention.

A more complete model can include coulombic efficiency:

$$
SOC_{k+1}
=
SOC_k
-
\frac{I_{batt,k}\Delta t}
{\eta_{coul}Q_{pack}3600}
$$

The usable SOC window is defined separately from the theoretical 0–100% cell capacity.

---

# 19. Electrical model outputs

For every simulation timestep, record:

| Output | Symbol |
|---|---|
| Battery voltage | $V_{batt}$ |
| Battery current | $I_{batt}$ |
| Battery power | $P_{batt}$ |
| SOC | $SOC$ |
| Motor speed | $n_i$ |
| Motor torque | $T_i$ |
| Motor mechanical power | $P_{mech,i}$ |
| Motor electrical power | $P_{motor,elec,i}$ |
| Motor loss | $P_{motor,loss,i}$ |
| Inverter loss | $P_{inv,loss,i}$ |
| Cable loss | $P_{cable,loss,i}$ |
| Auxiliary power | $P_{aux}$ |
| Total loss | $P_{loss,total}$ |
| Overall efficiency | $\eta_{total}$ |

These outputs become inputs to the thermal and vehicle-dynamics models.

---

# 20. Model limitations

This first implementation does not claim:

- exact motor efficiency maps
- exact inverter efficiency maps
- exact cable resistance
- exact battery impedance versus SOC and temperature
- exact cell thermal behaviour
- exact transient inverter current limits
- exact motor torque-speed envelopes
- exact competition-rule compliance

Those are validation stages.

---

# 21. Connection to the next models

The electrical model feeds the battery and thermal models:

$$
P_{loss}
\rightarrow
Q_{heat}
\rightarrow
T_{cell}
\rightarrow
R(SOC,T)
\rightarrow
V_{terminal}
$$

The refined system becomes:

```text
Vehicle demand
      ↓
Motor torque / speed
      ↓
Motor electrical power
      ↓
Inverter electrical power
      ↓
Battery current
      ↓
Battery voltage sag
      ↓
Battery losses
      ↓
Thermal model
      ↓
Temperature-dependent resistance
      ↓
Updated electrical operating point
```

---

# 22. Engineering conclusion

The electrical model establishes the key comparison:

> **Architecture B and Architecture C must be compared at the same total traction-power constraint, while allowing their motor power splits and operating points to differ.**

The number of motors alone does not determine efficiency or performance.

The final question is:

$$
\boxed{
\text{Does the dynamic benefit of Architecture C justify its additional mass and electrical complexity?}
}
$$

