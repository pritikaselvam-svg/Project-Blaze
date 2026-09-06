# 08. Vehicle Longitudinal Dynamics Model

## 1. Purpose

This document connects the motor and inverter model to actual vehicle motion.

The objective is to calculate:

- traction force
- aerodynamic drag
- rolling resistance
- acceleration
- vehicle speed
- distance
- 0–60 km/h time
- 0–100 km/h time
- theoretical top speed

This is the point where the project begins producing direct vehicle-performance results.

---

## 2. Model Boundary

The longitudinal model begins with available wheel torque and ends with vehicle position and speed.

**Motor torque → wheel torque → tyre force → net force → acceleration → speed → distance**

The model is initially a straight-line simulation.

---

## 3. Vehicle Mass

Current baseline masses:

- Architecture B: 214.5 kg
- Architecture C: 240.0 kg

These values come from the established component mass model.

---

## 4. Wheel Torque

For motor reduction ratio $G_i$:

$T_{wheel,i}=T_{motor,i}G_i\eta_{gear,i}$

Total available wheel torque is:

$T_{wheel,total}=\sum_iT_{wheel,i}$

For the first straight-line model, torque is distributed symmetrically within each axle.

---

## 5. Tractive Force

Wheel torque becomes longitudinal tyre force through wheel radius:

$F_{traction}=\frac{T_{wheel,total}}{r_w}$

A traction limit is enforced:

$F_{traction,max}=\mu W_{driven}$

Therefore:

$F_{actual}=\min(F_{requested},F_{traction,max})$

This prevents physically impossible acceleration.

---

## 6. Rolling Resistance

Rolling resistance is initially:

$F_{rr}=C_{rr}mg$

where $C_{rr}$ is the rolling-resistance coefficient.

---

## 7. Aerodynamic Drag

Aerodynamic drag is:

$F_{drag}=\frac{1}{2}\rho C_DA v^2$

where $\rho$ is air density, $C_D$ is drag coefficient, $A$ is frontal area and $v$ is vehicle speed.

Downforce can later be included using:

$F_{down}=\frac{1}{2}\rho C_LA v^2$

---

## 8. Net Longitudinal Force

The total longitudinal force is:

$F_{net}=F_{traction}-F_{rr}-F_{drag}-F_{grade}$

For a level track:

$F_{grade}=0$

Therefore:

$F_{net}=F_{traction}-F_{rr}-F_{drag}$

---

## 9. Vehicle Acceleration

Newton's second law gives:

$a=\frac{F_{net}}{m}$

Because B and C have different masses, identical traction force does not produce identical acceleration.

---

## 10. Speed and Distance Update

For timestep $\Delta t$:

$v_{k+1}=v_k+a_k\Delta t$

Distance is:

$x_{k+1}=x_k+v_k\Delta t+\frac{1}{2}a_k\Delta t^2$

The simulation begins with:

$v_0=0$

$x_0=0$

and advances until the selected stopping condition.

---

## 11. Power-Based Force Check

Wheel power and vehicle force are related by:

$P_{wheel}=F_{traction}v$

Therefore:

$F_{power}=\frac{P_{wheel}}{v}$

At very low speed, this expression cannot be used directly because $v$ approaches zero.

The simulation therefore uses motor torque at low speed and naturally transitions toward power limitation as speed rises.

---

## 12. Torque-Limited and Power-Limited Regions

At low speed:

$F_{traction}\approx\frac{T_{wheel,max}}{r_w}$

and:

$a\approx\frac{F_{traction}-F_{loss}}{m}$

At higher speed:

$F_{traction}\approx\frac{P_{wheel}}{v}$

and:

$a\approx\frac{P_{wheel}/v-F_{loss}}{m}$

The simulation should reveal the transition between these regimes.

---

## 13. Top Speed

Top speed occurs when available tractive force equals resistive force:

$F_{traction}=F_{rr}+F_{drag}$

Equivalently:

$P_{wheel}=v(F_{rr}+F_{drag})$

The simulation will find this point numerically.

---

## 14. Battery and Thermal Coupling

At every timestep:

1. Vehicle requests wheel torque.
2. Motor model determines torque and electrical power.
3. Battery model determines terminal voltage and current.
4. Battery SOC changes.
5. Battery temperature changes.
6. Electrical limits are updated.
7. Available motor power may change.
8. Vehicle acceleration is recalculated.

This makes the final model a coupled electro-thermal vehicle simulation rather than a fixed-power calculator.

---

## 15. Architecture B Simulation Path

**Battery → 2 inverters → 2 rear motors → rear wheels → traction force → vehicle acceleration**

The rear motors can later receive different torque commands for torque-vectoring studies.

For the first straight-line simulation, equal torque distribution is used.

---

## 16. Architecture C Simulation Path

**Battery → 3 inverters → front motor + 2 rear motors → four wheels → traction force → vehicle acceleration**

The first straight-line simulation uses a controlled torque/power distribution.

The front differential does not need a separate longitudinal calculation in a straight line.

---

## 17. Primary Simulation Outputs

### Performance

- 0–60 km/h time
- 0–100 km/h time
- top speed
- acceleration vs time
- speed vs time
- distance vs time

### Powertrain

- motor torque vs speed
- motor power vs speed
- battery power vs time
- battery current vs time
- inverter loss
- motor loss

### Battery

- SOC vs time
- terminal voltage vs time
- battery temperature vs time

### Architecture comparison

- B vs C acceleration
- B vs C speed
- B vs C energy consumption
- B vs C battery current
- B vs C thermal behaviour

---

## 18. First Simulation Experiment

The first experiment should deliberately be simple.

Hold fixed:

- level track
- tyre-road friction coefficient
- air density
- aerodynamic coefficients
- initial battery temperature
- initial SOC
- auxiliary load
- wheel radius
- reduction ratio unless architecture-specific gearing is being studied
- total power ceiling

Then simulate:

**0 km/h → 100 km/h**

for both architectures.

This gives the first directly comparable vehicle-performance result.

---

## 19. Second Simulation Experiment

Run the same acceleration simulation at different battery states:

- high SOC
- medium SOC
- low SOC

The purpose is to determine whether battery voltage sag and current limitations materially change acceleration performance.

---

## 20. Third Simulation Experiment

Introduce thermal feedback.

Allow:

- battery temperature to evolve
- motor temperature to evolve
- inverter temperature to evolve
- electrical resistance to vary with temperature
- validated power limits to derate when required

This converts the baseline performance model into a coupled electro-thermal simulation.

---

## 21. What Comes Next

These two documents define the models.

The next step is **not another documentation chapter**.

The next step is to implement the equations in a numerical simulation.

The first simulation should produce actual plots and numerical results for Architecture B and Architecture C.

The intended progression is:

**Model → Code → Run → Plot → Validate → Compare → Improve**

The first target is a working 0–100 km/h simulation. Once that works, the model can be expanded toward lap-time simulation and cornering dynamics.
