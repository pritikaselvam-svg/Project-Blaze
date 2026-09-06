# 06 — Battery and Powertrain Thermal Model

## 1. Purpose

This model estimates how electrical losses become heat and how heat changes the operating condition of the Formula Student EV powertrain.

It covers:

- battery cell heating
- inverter heating
- motor heating
- cooling-system demand
- temperature-dependent electrical behaviour
- thermal limits
- power derating

---

# 2. Thermal energy balance

For a lumped component:

$$mc_p\frac{dT}{dt}$$
=
$$P_{generated}$$
-
$$P_{removed}$$

where $m$ is mass, $c_p$ is specific heat capacity and $T$ is component temperature.

---

# 3. Battery heat generation

First-order battery heat:

$$P_{battery,heat}$$
=
$$I_{batt}^2R_{pack}$$

Using cell quantities:

$$P_{battery,heat}$$
=
$$N_{series}N_{parallel}
I_{cell}^2R_{cell}
$$

The refined model uses:

$$R_{cell}=R(SOC,T,I)$$

therefore:

$$P_{battery,heat}$$
=
$$N_{series}N_{parallel}
I_{cell}^2
R(SOC,T,I)
$$

This is the main ohmic heat source.

---

# 4. Battery lumped thermal model

For a single equivalent battery temperature:

$$m_{batt}c_{p,batt}\frac{dT_{batt}}{dt}$$
=
$$P_{battery,heat}$$
-
$$P_{cool,batt}$$

A simple coolant coupling is:

$$P_{cool,batt}$$
=
$$UA_{batt}(T_{batt}-T_{coolant})$$

Therefore:

$$m_{batt}c_{p,batt}\frac{dT_{batt}}{dt}$$
=
$$P_{battery,heat}$$
-
$$UA_{batt}(T_{batt}-T_{coolant})$$

---

# 5. Coolant temperature rise

For a liquid cooling loop:

$$P_{cool}$$
=
$$\dot{m}c_{p,coolant}\Delta T_{coolant}$$

Therefore:

$$\Delta T_{coolant}$$
=
$$\frac{P_{cool}}{\dot{m}c_{p,coolant}}$$

Outlet temperature:

$$T_{out}$$
=
$$T_{in}+\Delta T_{coolant}$$

The coolant temperature rise must therefore come from the heat balance and flow rate rather than being assumed independently.

---

# 6. Inverter heat generation

For each inverter:

$$P_{inv,heat}$$
=
$$P_{inv,loss}$$

If inverter efficiency is used:

$$P_{inv,heat}$$
=
$$P_{motor,elec}
\left(
\frac{1}{\eta_{inv}}-1
\right)
$$

The thermal model is:

$$m_{inv}c_{p,inv}$$
$$\frac{dT_{inv}}{dt}$$
=
$$P_{inv,heat}$$
-
$$UA_{inv}(T_{inv}-T_{coolant})$$

---

# 7. Motor heat generation

For each motor:

$$P_{motor,heat}$$
=
$$P_{motor,elec}-P_{motor,mech}$$

or:

$$P_{motor,heat}$$
=
$$P_{motor,mech}
\left(
\frac{1}{\eta_{motor}}-1
\right)
$$

The lumped model is:

$$m_{motor}c_{p,motor}
\frac{dT_{motor}}{dt}$$
=
$$P_{motor,heat}$$
-
$$UA_{motor}(T_{motor}-T_{coolant})
$$

A future detailed model can separate stator copper, iron, rotor and housing temperatures.

---

# 8. Copper-loss refinement

If winding resistance is known:

$$P_{cu}=I_{phase,rms}^2R_{phase}(T)$$

Resistance can be approximated as:

$$R(T)$$
=
$$R_{ref}
\left[
1+\alpha(T-T_{ref})
\right]
$$

Therefore:

$$
T\uparrow
\rightarrow
R\uparrow
\rightarrow
P_{cu}\uparrow
\rightarrow
T\uparrow
$$

This is an important electro-thermal feedback mechanism.

---

# 9. Total powertrain heat

$$P_{heat,total}$$
=
$$P_{battery,heat}
+
\sum P_{motor,heat}
+
\sum P_{inv,heat}
+
P_{other}$$

Heat should be tracked by subsystem rather than immediately combining everything into one temperature.

---

# 10. Architecture B thermal structure

Architecture B contains:

- battery
- two rear motors
- rear inverter system
- cooling system

Conceptually:

```text
Battery heat ───────→ Battery cooling
                       │
Motor L heat ────────→│
Motor R heat ────────→│
Inverter heat ───────→│
                       ↓
                  Radiator
                       ↓
                  Ambient air
```

The exact physical loop arrangement is a hardware-design input.

---

# 11. Architecture C thermal structure

Architecture C contains:

- battery
- front motor
- two rear motors
- inverter hardware
- cooling system

Conceptually:

```text
Battery zone ─────→ Cooling loop
                     │
Front motor zone ──→│
                     │
Rear motor zone ───→│
                     │
Inverter zone ─────→│
                     ↓
                  Radiator
                     ↓
                 Ambient air
```

Architecture C may require greater cooling capacity because it has an additional traction path, but the actual comparison must use calculated losses.

---

# 12. Thermal resistance model

An alternative steady-state model is:

$$P_{cool}$$
=
$$\frac{T_{component}-T_{ambient}}
{R_{\theta}}
$$

Temperature rise:

$$\Delta T$$
=
$$P_{loss}R_{\theta}$$

This is useful for early feasibility studies.

---

# 13. Thermal capacitance and time constant

Thermal capacitance:

$$C_{th}=mc_p$$

For a first-order component:

$$C_{th}\frac{dT}{dt}$$
=
$$P_{loss}$$
-
$$\frac{T-T_{ambient}}{R_{\theta}}
$$

Time constant:

$$\boxed{\tau=R_{\theta}C_{th}}$$

This matters because a short power burst and continuous power demand are different thermal problems.

---

# 14. Ambient temperature

Ambient temperature is an input:

$$T_{ambient}=T_{amb}$$

The simulation should eventually evaluate multiple representative ambient conditions.

---

# 15. Radiator model

For coolant-to-air heat rejection:

$$Q_{rad}$$
=
$$\dot{m}_{air}c_{p,air}
(T_{air,out}-T_{air,in})
$$

The radiator must satisfy:

$$Q_{rad}\geq Q_{required}$$

Actual radiator performance depends on:

- air mass flow
- coolant mass flow
- radiator geometry
- heat-transfer coefficient
- ambient temperature
- vehicle speed

Therefore a fixed heat-rejection number must be treated as a testable design input.

---

# 16. Battery thermal constraint

The battery temperature must remain inside the selected operating envelope:

$$
T_{cell,min}
\leq
T_{cell}
\leq
T_{cell,max}
$$

Exact limits should come from the selected cell data and accumulator design requirements.

A future spatial model should track the most critical cell rather than only the average pack temperature.

---

# 17. Temperature-dependent power capability

A thermal derating function can be introduced:

$$
P_{available}=f(T,SOC)
$$

Inside the permitted operating region:

$$
P_{available}=P_{max}
$$

As temperature approaches a defined limit:

$$
P_{available}\downarrow
$$

The exact derating curve must come from the selected hardware. It should not be invented as a universal temperature threshold.

---

# 18. Electrical-thermal feedback

The coupled system is:

```text
Power demand
     ↓
Motor / inverter losses
     ↓
Battery current
     ↓
Battery I²R heating
     ↓
Temperature rises
     ↓
Resistance changes
     ↓
Voltage sag changes
     ↓
Available power changes
     ↓
Motor operating point changes
     ↓
New losses
```

Mathematically:

$$
I
\rightarrow
P_{loss}
\rightarrow
T
\rightarrow
R(SOC,T)
\rightarrow
V_{terminal}
\rightarrow
I
$$

This feedback is essential for endurance analysis.

---

# 19. Discrete simulation procedure

At each simulation timestep $\Delta t$:

### Step 1 — Calculate motor torque and speed

$$P_{mech}=T\omega$$

### Step 2 — Calculate motor and inverter losses

$$P_{loss}=P_{motor,loss}+P_{inv,loss}$$

### Step 3 — Calculate battery current

$$I_{batt}$$
=
$$\frac{P_{batt}}{V_{terminal}}
$$

### Step 4 — Calculate battery heat

$$P_{battery,heat}=I_{batt}^2R_{pack}$$

### Step 5 — Update SOC

$$SOC_{k+1}$$
=
$$SOC_k$$
-
$$\frac{I_{batt}\Delta t}
{Q_{pack}3600}$$

### Step 6 — Update component temperature

$$T_{k+1}$$
=
$$T_k+
\frac{\Delta t}{mc_p}
(P_{loss}-P_{cool})
$$

### Step 7 — Update resistance

$$R_{k+1}=R(SOC_{k+1},T_{k+1})$$

### Step 8 — Update terminal voltage

$$V_{k+1}$$
=
$$V_{OCV}(SOC_{k+1},T_{k+1})$$
-
$$I_{k+1}R_{pack,k+1}
$$

### Step 9 — Check limits

Check current, voltage, temperature and power limits.

---

# 20. Limit checks

At every timestep:

### Battery current

$$
I_{cell}\leq I_{cell,max}
$$

### Cell voltage

$$
V_{cell,min}\geq V_{cell,min,allowed}
$$

### Cell temperature

$$
T_{cell}\leq T_{cell,max}
$$

### Motor temperature

$$
T_{motor}\leq T_{motor,max}
$$

### Inverter temperature

$$
T_{inv}\leq T_{inv,max}
$$

### Total traction power

$$
P_{traction}\leq80\text{ kW}
$$

If a limit is violated, the model reduces available torque or power according to the selected control strategy.

---

# 21. Thermal derating logic

```text
                 Calculate requested power
                           ↓
                     Check limits
                           ↓
              ┌────────────┴────────────┐
              ↓                         ↓
          All limits OK             Limit exceeded
              ↓                         ↓
       Deliver requested power     Apply derating
                                        ↓
                                  Recalculate power
```

A combined available-power constraint is:

$$P_{available}$$
=
$$\min
\left(
P_{motor},
P_{inverter},
P_{battery},
P_{thermal},
P_{rules}
\right)
$$

This prevents the vehicle model from requesting physically unavailable power.

---

# 22. Endurance simulation

For endurance analysis, record:

$$
T(t)
$$

$$
SOC(t)
$$

$$
V_{battery}(t)
$$

$$
I_{battery}(t)
$$

$$
P_{loss}(t)
$$

and cumulative energy:

$$
E(t)=\int_0^tP_{battery}(\tau)d\tau
$$

The key question is:

> Can the vehicle maintain the required power throughout the simulated endurance event without violating battery, motor or inverter limits?

---

# 23. What this model does not assume

The model does not assume:

- a universal 40°C derating threshold
- a fixed radiator temperature rise
- a fixed coolant temperature
- guaranteed continuous 80 kW output
- constant cell resistance under all conditions
- perfect cell current sharing
- perfect cooling
- zero thermal gradients

These values must come from hardware data, measurements or explicitly stated engineering assumptions.

---

# 24. Validation plan

### Stage 1 — Analytical

Verify:

$$
P=I^2R
$$

and:

$$
Q=mc_p\Delta T
$$

### Stage 2 — Component data

Replace assumed:

- $R$
- $c_p$
- $UA$
- thermal limits
- efficiency

with manufacturer or measured data.

### Stage 3 — Bench testing

Measure:

- current
- voltage
- component temperature
- coolant inlet temperature
- coolant outlet temperature
- coolant flow

### Stage 4 — Vehicle validation

Compare predicted and measured temperature trajectories during representative driving.

---

# 25. Engineering conclusion

The thermal model changes the question from:

> "Can the vehicle produce 80 kW?"

to:

> **"Can the vehicle sustain the required power without thermal or electrical derating?"**

Architecture C may gain dynamic performance from its additional driven axle, but it also introduces additional motor/inverter losses and thermal loads.

Architecture B may have a simpler thermal system, but that does not automatically make it faster.

The final comparison must therefore evaluate:

$$
\boxed{
\text{Performance}
+
\text{Energy}
+
\text{Thermal sustainability}
}
$$
