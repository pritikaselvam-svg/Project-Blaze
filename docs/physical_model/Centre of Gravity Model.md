# Centre of Gravity Model
## Formula Student Electric Powertrain Architecture Study

**Document status:** Baseline analytical model  
**Purpose:** Determine the centre of gravity and static longitudinal mass distribution of the two proposed EV powertrain architectures.

---

## 1. Purpose

The centre of gravity (CG) is one of the first vehicle-level quantities calculated in this project because the physical placement of the powertrain directly changes how vehicle mass is distributed.

For this architecture study, the CG is used to evaluate how changing from a two-motor rear-wheel-drive architecture to a three-motor AWD architecture affects:

- longitudinal mass distribution,
- front/rear static axle loading,
- vehicle balance,
- powertrain packaging,
- subsequent yaw-inertia calculations,
- vehicle-dynamics behaviour.

The CG calculation is therefore not treated as an isolated specification. It forms the foundation for the later dynamic analysis.

---

# 2. Architectures being compared

### Architecture B

Architecture B uses:

- 2 × rear electric motors
- independent rear-left and rear-right motor control
- rear-wheel drive
- rear-mounted inverter system
- central battery
- single cooling system

### Architecture C

Architecture C uses:

- 1 × front-centre electric motor
- 2 × independent rear electric motors
- hybrid AWD
- front mechanical differential
- combined rear inverter system
- central battery
- additional front powertrain hardware and cabling

The purpose is to determine how the additional front powertrain changes the overall mass distribution.

---

# 3. Vehicle coordinate system

A fixed vehicle coordinate system is used for all calculations.

| Axis | Positive direction |
|---|---|
| X | Forward, toward front axle |
| Y | Left side of vehicle |
| Z | Upward |

The origin is defined as:

> **the geometric centre of the wheelbase at ground level**

For the current vehicle model:

\[
L=1550\text{ mm}=1.55\text{ m}
\]

Therefore:

\[
X_{front}=+775\text{ mm}
\]

\[
X_{rear}=-775\text{ mm}
\]

The vehicle centreline is:

\[
Y=0
\]

and the ground plane is:

\[
Z=0
\]

---

# 4. CG calculation

For a collection of discrete masses, the centre of gravity is calculated using the weighted average:

\[
\boxed{
X_{CG}=\frac{\sum m_iX_i}{\sum m_i}
}
\]

\[
\boxed{
Y_{CG}=\frac{\sum m_iY_i}{\sum m_i}
}
\]

\[
\boxed{
Z_{CG}=\frac{\sum m_iZ_i}{\sum m_i}
}
\]

where:

- \(m_i\) = mass of component \(i\)
- \(X_i,Y_i,Z_i\) = position of component \(i\)

This treats the complete vehicle as a collection of component masses.

---

# 5. Architecture B mass model

The current Architecture B model is:

| Component | Mass (kg) | X (mm) | Y (mm) | Z (mm) |
|---|---:|---:|---:|---:|
| Battery | 45.0 | -185 | 0 | 180 |
| Rear-left motor | 3.55 | -765 | +450 | 203 |
| Rear-right motor | 3.55 | -765 | -450 | 203 |
| Rear inverter | 6.5 | -655 | 0 | 250 |
| Cooling system | 3.5 | -85 | +350 | 220 |
| VCU | 0.4 | +215 | 0 | 300 |
| HV cables | 1.5 | -435 | 0 | 220 |
| Fuse/protection | 1.5 | -185 | 0 | 280 |
| Driver | 68 | +15 | 0 | 260 |
| Chassis | 28 | 0 | 0 | 280 |
| Wheels/tyres | 22 | 0 | 0 | 203 |
| Suspension | 16 | 0 | 0 | 250 |
| Other fixed mass | 15 | 0 | 0 | 320 |

The current total vehicle mass is:

\[
\boxed{M_B=214.5\text{ kg}}
\]

The wheels, suspension, chassis and other fixed mass are initially represented at their centroidal locations in this CG model.

This simplification is acceptable for the first CG calculation because the CG depends on the **mass centroid**, not on the detailed distribution of the mass around that centroid.

The same simplification will later be revisited for yaw inertia, where spatial distribution becomes much more important.

---

# 6. Architecture B longitudinal CG

Using:

\[
X_{CG}=\frac{\sum m_iX_i}{M}
\]

the calculated longitudinal CG is:

\[
\boxed{
X_{CG,B}=-84.5\text{ mm}
}
\]

Therefore, the CG lies approximately:

> **84.5 mm behind the geometric centre of the wheelbase.**

This is expected because Architecture B has its major propulsion hardware concentrated toward the rear.

---

# 7. Architecture B lateral CG

The calculated lateral CG is:

\[
\boxed{
Y_{CG,B}=+5.7\text{ mm}
}
\]

The small positive offset results from the current asymmetric placement of the cooling system.

This is not intended to represent a final physical vehicle asymmetry.

If the cooling hardware is ultimately packaged symmetrically around the vehicle centreline, the final design should approach:

\[
Y_{CG}=0
\]

This value is therefore retained as part of the current packaging model rather than artificially forced to zero.

---

# 8. Architecture B vertical CG

The current calculated vertical CG is:

\[
\boxed{
Z_{CG,B}\approx240.5\text{ mm}
}
\]

This is relevant to later vehicle-dynamics analysis because a lower CG generally reduces the magnitude of load-transfer effects for a given acceleration.

However, vertical CG is not required for the basic yaw moment of inertia calculation.

---

# 9. Architecture C mass model

The current Architecture C model is:

| Component | Mass (kg) | X (mm) | Y (mm) | Z (mm) |
|---|---:|---:|---:|---:|
| Battery | 45.0 | -185 | 0 | 180 |
| Front motor | 13.5 | +765 | 0 | 203 |
| Rear-left motor | 3.55 | -765 | +450 | 203 |
| Rear-right motor | 3.55 | -765 | -450 | 203 |
| Rear inverter | 11.0 | -655 | 0 | 250 |
| Front differential | 4.5 | +765 | 0 | 170 |
| Cooling system | 5.0 | +65 | 0 | 220 |
| VCU | 0.6 | +215 | 0 | 300 |
| HV cables | 2.8 | +55 | 0 | 210 |
| Fuse/protection | 1.5 | -185 | 0 | 280 |
| Driver | 68 | +15 | 0 | 260 |
| Chassis | 28 | 0 | 0 | 280 |
| Wheels/tyres | 22 | 0 | 0 | 203 |
| Suspension | 16 | 0 | 0 | 250 |
| Other fixed mass | 15 | 0 | 0 | 320 |

The current total mass is:

\[
\boxed{M_C=240.0\text{ kg}}
\]

The front motor mass is currently an engineering input and must be replaced by the verified final motor specification before this model is considered component-data-locked.

---

# 10. Architecture C longitudinal CG

Applying the same calculation:

\[
X_{CG,C}
=
\frac{\sum m_iX_i}{M_C}
\]

gives:

\[
\boxed{
X_{CG,C}=-24.3\text{ mm}
}
\]

The CG is therefore much closer to the geometric centre of the wheelbase than in Architecture B.

This occurs because Architecture C introduces substantial mass at the front axle.

---

# 11. Architecture C lateral CG

The architecture is laterally symmetric in the current model.

Therefore:

\[
\boxed{
Y_{CG,C}=0
}
\]

This is the desired result for the baseline packaging model.

---

# 12. Architecture C vertical CG

The current model gives:

\[
\boxed{
Z_{CG,C}\approx237.0\text{ mm}
}
\]

The additional front powertrain hardware does not significantly increase the vertical CG in the current assumed packaging.

---

# 13. Static front/rear weight distribution

Once the longitudinal CG is known, the static axle loads can be calculated.

For a wheelbase:

\[
L=1.55\text{ m}
\]

with the origin at the centre of the wheelbase, the distance from the rear axle to the CG is:

\[
d_r=0.775+X_{CG}
\]

The front axle load fraction is therefore:

\[
\boxed{
\frac{F_f}{W}
=
\frac{0.775+X_{CG}}{1.55}
}
\]

and:

\[
\frac{F_r}{W}=1-\frac{F_f}{W}
\]

---

# 14. Architecture B static distribution

For:

\[
X_{CG,B}=-0.0845\text{ m}
\]

\[
\frac{F_f}{W}
=
\frac{0.775-0.0845}{1.55}
\]

giving approximately:

\[
\boxed{F_f=44.5\%}
\]

and:

\[
\boxed{F_r=55.5\%}
\]

Therefore Architecture B has a rear-biased static weight distribution.

---

# 15. Architecture C static distribution

For:

\[
X_{CG,C}=-0.0243\text{ m}
\]

\[
\frac{F_f}{W}
=
\frac{0.775-0.0243}{1.55}
\]

giving approximately:

\[
\boxed{F_f=48.4\%}
\]

and:

\[
\boxed{F_r=51.6\%}
\]

Architecture C is therefore significantly closer to a balanced front/rear static distribution.

---

# 16. Comparison

| Parameter | Architecture B | Architecture C |
|---|---:|---:|
| Total mass | 214.5 kg | 240.0 kg |
| \(X_{CG}\) | -84.5 mm | -24.3 mm |
| \(Y_{CG}\) | +5.7 mm | 0 mm |
| \(Z_{CG}\) | ~240.5 mm | ~237.0 mm |
| Front static load | 44.5% | 48.4% |
| Rear static load | 55.5% | 51.6% |

---

# 17. Engineering interpretation

The major change between the two architectures is the addition of a front propulsion system in Architecture C.

This has two immediate effects.

### Effect 1: Increased vehicle mass

Architecture C increases the modelled mass from:

\[
214.5\rightarrow240.0\text{ kg}
\]

This additional mass will later affect:

- acceleration,
- energy consumption,
- tyre loading,
- electrical energy required,
- braking requirements.

### Effect 2: More balanced longitudinal mass distribution

The CG moves from:

\[
-84.5\text{ mm}
\]

to:

\[
-24.3\text{ mm}
\]

This shifts the static distribution from approximately:

\[
44.5/55.5
\]

to:

\[
48.4/51.6
\]

Thus the front axle carries a greater share of the vehicle load in Architecture C.

---

# 18. Why the CG calculation matters for the later yaw analysis

The centre of gravity is also required for the yaw moment of inertia calculation.

Yaw inertia is calculated about the vehicle CG:

\[
I_z=
\sum_i m_i
\left[
(X_i-X_{CG})^2+
(Y_i-Y_{CG})^2
\right]
\]

Therefore the CG calculated in this document becomes an input to the next stage of the model.

This creates a direct chain:

\[
\boxed{
\text{Component placement}
\rightarrow
\text{CG}
\rightarrow
\text{Yaw inertia}
\rightarrow
\text{Vehicle dynamics}
}
\]

---

# 19. Modelling assumptions

The current CG model makes the following assumptions:

1. The listed component masses represent the complete vehicle mass model.
2. Component positions represent their centres of mass.
3. Wheel, suspension, chassis and other fixed masses are initially represented by their centroidal locations.
4. The wheelbase is 1550 mm.
5. The vehicle coordinate system remains fixed for both architectures.
6. The driver mass is 68 kg.
7. The same base chassis is used for both architecture comparisons.
8. Component mass values that are not yet datasheet-locked are treated as engineering inputs rather than experimental measurements.

---

# 20. Important distinction: CG vs yaw inertia

The centroidal simplifications used here are acceptable for the first CG calculation.

However, they cannot automatically be carried into the yaw-inertia calculation.

For example, the 22 kg wheel/tyre mass has its centroid represented at:

\[
X=0,\quad Y=0
\]

for the CG calculation.

That is sufficient for calculating the mass centroid if the total wheel mass is symmetrically distributed.

But physically, the wheels are located near the four vehicle corners.

For yaw inertia, their distance from the CG matters:

\[
I_z\propto r^2
\]

Therefore, the yaw model must later use a more spatially accurate representation.

This distinction is intentionally documented so that the modelling process can be improved without changing the underlying CG result.

---

# 21. Conclusion

The baseline CG model establishes the mass-distribution difference between the two proposed powertrain architectures.

Architecture B produces:

\[
\boxed{
X_{CG}=-84.5\text{ mm}
}
\]

with approximately:

\[
\boxed{
44.5/55.5\%\text{ front/rear}
}
\]

Architecture C produces:

\[
\boxed{
X_{CG}=-24.3\text{ mm}
}
\]

with approximately:

\[
\boxed{
48.4/51.6\%\text{ front/rear}
}
\]

The introduction of the front motor and associated hardware therefore moves Architecture C toward a more balanced longitudinal mass distribution while increasing total vehicle mass.

These CG values form the baseline inputs for the subsequent yaw moment of inertia model.

---

## Version history

| Version | Change |
|---|---|
| v0.1 | Initial component-based CG model |
| v0.2 | Added explicit vehicle coordinate system |
| v0.3 | Added front/rear static load calculation |
| v0.4 | Documented distinction between CG and spatial inertia modelling |