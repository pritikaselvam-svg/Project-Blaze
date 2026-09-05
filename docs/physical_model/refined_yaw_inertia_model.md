# Refined Yaw Moment of Inertia Model
## Formula Student Electric Powertrain Architecture Study

**Document status:** First refined modelling pass  
**Purpose:** Establish a defensible, reproducible method for comparing the yaw moment of inertia of Architecture B and Architecture C.

---

## 1. Why this analysis exists

Yaw moment of inertia, \(I_z\), describes how strongly a vehicle resists changes in its rotational motion about the vertical axis.

For a Formula Student EV, this matters because the vehicle is constantly making rapid yaw corrections during:

- corner entry,
- transient steering,
- slalom sections,
- direction changes,
- corner exit,
- torque-vectoring interventions,
- recovery from small disturbances.

A lower yaw inertia generally means the vehicle can change yaw rate more readily for a given applied yaw moment. A higher yaw inertia means more rotational inertia must be overcome.

However, **lower \(I_z\) is not automatically better**.

The objective of this project is not to minimize yaw inertia in isolation. The architecture comparison must consider yaw inertia together with:

- mass distribution,
- centre of gravity,
- traction,
- electrical losses,
- thermal behaviour,
- energy consumption,
- powertrain mass,
- packaging,
- dynamic performance.

Therefore, \(I_z\) is treated as one measurable consequence of the powertrain architecture rather than as the sole optimization target.

---

# 2. Research question

> **How does redistributing the same vehicle mass and powertrain capability across different electric powertrain architectures change yaw moment of inertia, and what does that imply for Formula Student EV dynamic performance?**

The comparison is between:

### Architecture B
- 2 × rear motors
- independent left/right rear motors
- rear-wheel drive
- electronic torque vectoring
- common central battery
- rear-mounted inverter system

### Architecture C
- 3 × motors
- 1 front-centre motor
- 2 independent rear motors
- hybrid AWD
- common battery
- additional front differential/mechanical driveline element
- front motor/inverter/cooling packaging

The goal is to keep the underlying vehicle as comparable as possible while changing the powertrain architecture.

---

# 3. Vehicle coordinate system

A fixed vehicle coordinate system is used throughout the project.

| Axis | Positive direction |
|---|---|
| X | Forward, toward the front axle |
| Y | Left side of vehicle |
| Z | Upward |

The origin is placed at:

> **the geometric centre of the wheelbase at ground level**

For a 1550 mm wheelbase:

- Front axle: \(X = +0.775\) m
- Rear axle: \(X = -0.775\) m
- Vehicle centreline: \(Y = 0\)
- Ground plane: \(Z = 0\)

Only X and Y are required for the basic yaw-inertia calculation because yaw is rotation about the Z-axis.

---

# 4. Fundamental equation

The true physical definition is:

\[
I_z = \int r^2\,dm
\]

where \(r\) is the perpendicular distance from the mass element to the vehicle's yaw axis.

For a discrete mass model:

\[
\boxed{
I_z =
\sum_i m_i
\left[
(X_i-X_{CG})^2+
(Y_i-Y_{CG})^2
\right]
}
\]

where:

- \(m_i\) = mass of component \(i\)
- \(X_i,Y_i\) = component position
- \(X_{CG},Y_{CG}\) = vehicle centre-of-gravity coordinates

This equation is the basis of the model.

---

# 5. The important modelling decision

## Every mass must be accounted for

A common mistake in early vehicle modelling is to calculate yaw inertia using only the obvious powertrain components:

- motors,
- battery,
- inverter.

That is not sufficient.

The vehicle's:

- wheels,
- tyres,
- suspension,
- chassis,
- driver,
- cooling system,
- cables,
- protection hardware,
- electronics,
- auxiliary components

also contribute to \(I_z\).

The correct question is therefore **not**:

> "Which components should I include?"

The correct question is:

> "How accurately should each component be represented?"

That distinction is important.

---

# 6. Mass representation strategy

Each component is assigned to one of three modelling classes.

## Class A: Compact point mass

Used when the component is relatively small compared with the vehicle.

Examples:

- inverter,
- VCU,
- fuse/protection,
- compact motor,
- compact electronics.

For these components:

\[
I_z \approx m[(X-X_{CG})^2+(Y-Y_{CG})^2]
\]

---

## Class B: Distributed component

Used when a component has significant physical dimensions.

Examples:

- battery pack,
- chassis.

For a rectangular component, its own centroidal yaw inertia is included:

\[
I_{z,cm}
=
\frac{m(L^2+W^2)}{12}
\]

The parallel-axis theorem then shifts this inertia to the vehicle CG:

\[
\boxed{
I_z = I_{z,cm}+md^2
}
\]

where:

\[
d^2=(X-X_{CG})^2+(Y-Y_{CG})^2
\]

This prevents a large battery or chassis from being incorrectly treated as a single mathematical point.

---

## Class C: Corner/distributed masses

Used for components that physically sit near the four corners of the vehicle.

Examples:

- wheels/tyres,
- suspension/hub masses.

These are represented separately at their physical corner coordinates.

This is particularly important because yaw inertia scales with the **square** of distance:

\[
I_z \propto r^2
\]

Therefore, a relatively small mass at a large radius can contribute more to yaw inertia than a larger mass close to the CG.

---

# 7. Fixed vehicle geometry used in this model

The current first-order model uses:

| Parameter | Value |
|---|---:|
| Wheelbase | 1.55 m |
| Front track | 1.25 m |
| Rear track | 1.20 m |
| Front axle X | +0.775 m |
| Rear axle X | -0.775 m |
| Front wheel Y positions | ±0.625 m |
| Rear wheel Y positions | ±0.600 m |

Thus the wheel locations are:

| Wheel | X (m) | Y (m) |
|---|---:|---:|
| Front-left | +0.775 | +0.625 |
| Front-right | +0.775 | -0.625 |
| Rear-left | -0.775 | +0.600 |
| Rear-right | -0.775 | -0.600 |

These dimensions should eventually be replaced by the final chassis geometry if the project moves from the first-order model to CAD-level validation.

---

# 8. Architecture B mass model

Architecture B uses the following current masses and positions.

| Component | Mass (kg) | X (m) | Y (m) | Representation |
|---|---:|---:|---:|---|
| Battery | 45.0 | -0.185 | 0 | Distributed |
| Rear-left motor | 3.55 | -0.765 | +0.450 | Point |
| Rear-right motor | 3.55 | -0.765 | -0.450 | Point |
| Rear inverter | 6.50 | -0.655 | 0 | Point |
| Cooling system | 3.50 | -0.085 | +0.350 | Point |
| VCU | 0.40 | +0.215 | 0 | Point |
| HV cables | 1.50 | -0.435 | 0 | Point |
| Fuse/protection | 1.50 | -0.185 | 0 | Point |
| Driver | 68.0 | +0.015 | 0 | Point |
| Chassis | 28.0 | 0 | 0 | Distributed |
| Wheels/tyres | 22.0 | corner locations | | Corner |
| Suspension | 16.0 | corner locations | | Corner |
| Other fixed mass | 15.0 | distributed longitudinally | | Distributed |

### Total mass

\[
\boxed{M_B=214.5\text{ kg}}
\]

---

# 9. Architecture C mass model

Architecture C uses the following current masses and positions.

| Component | Mass (kg) | X (m) | Y (m) | Representation |
|---|---:|---:|---:|---|
| Battery | 45.0 | -0.185 | 0 | Distributed |
| Front motor | 13.5 | +0.765 | 0 | Point |
| Rear-left motor | 3.55 | -0.765 | +0.450 | Point |
| Rear-right motor | 3.55 | -0.765 | -0.450 | Point |
| Rear inverter / multi-inverter box | 11.0 | -0.655 | 0 | Point |
| Front differential | 4.5 | +0.765 | 0 | Point |
| Cooling system | 5.0 | +0.065 | 0 | Point |
| VCU | 0.60 | +0.215 | 0 | Point |
| HV cables | 2.80 | +0.055 | 0 | Point |
| Fuse/protection | 1.50 | -0.185 | 0 | Point |
| Driver | 68.0 | +0.015 | 0 | Point |
| Chassis | 28.0 | 0 | 0 | Distributed |
| Wheels/tyres | 22.0 | corner locations | | Corner |
| Suspension | 16.0 | corner locations | | Corner |
| Other fixed mass | 15.0 | distributed longitudinally | | Distributed |

### Total mass

\[
\boxed{M_C=240.0\text{ kg}}
\]

**Important:** the front motor is currently represented by its assumed 13.5 kg mass. The exact motor model/datasheet must be locked before this number is presented as a verified component specification.

---

# 10. Centre of gravity calculation

Before calculating yaw inertia, the vehicle CG must be calculated.

For the longitudinal direction:

\[
X_{CG}=
\frac{\sum m_iX_i}{\sum m_i}
\]

For the lateral direction:

\[
Y_{CG}=
\frac{\sum m_iY_i}{\sum m_i}
\]

The vertical coordinate is not required for basic yaw inertia:

\[
Z_{CG}\quad\text{does not enter the 2D yaw equation}
\]

---

# 11. Architecture B CG

Using the current mass distribution:

\[
\boxed{X_{CG,B}=-0.08455\text{ m}}
\]

or:

\[
\boxed{X_{CG,B}=-84.5\text{ mm}}
\]

The lateral CG is:

\[
\boxed{Y_{CG,B}=+0.00571\text{ m}}
\]

or approximately:

\[
\boxed{Y_{CG,B}=+5.7\text{ mm}}
\]

The small positive Y value is caused by the asymmetric placement of the current cooling-system mass.

This is a useful modelling observation:

> If the final physical design places the cooling hardware symmetrically, this lateral CG offset should disappear.

---

# 12. Architecture C CG

For Architecture C:

\[
\boxed{X_{CG,C}=-0.02434\text{ m}}
\]

or:

\[
\boxed{X_{CG,C}=-24.3\text{ mm}}
\]

Because the major components are laterally symmetric:

\[
\boxed{Y_{CG,C}=0}
\]

This moves the vehicle CG significantly forward relative to Architecture B.

The reason is primarily the additional front powertrain mass.

---

# 13. Static front/rear mass distribution

For a wheelbase of 1.55 m and CG located at \(X_{CG}\):

\[
F_{front}
=
\frac{0.775+X_{CG}}{1.55}
\]

\[
F_{rear}
=
1-F_{front}
\]

### Architecture B

\[
F_{front,B}\approx44.5\%
\]

\[
F_{rear,B}\approx55.5\%
\]

### Architecture C

\[
F_{front,C}\approx48.4\%
\]

\[
F_{rear,C}\approx51.6\%
\]

Therefore, Architecture C produces a substantially more balanced longitudinal mass distribution.

---

# 14. Why the earlier point-mass result was not sufficient

An initial calculation treated every component as a point mass at its listed coordinate.

That produced approximately:

| Architecture | Preliminary point-mass \(I_z\) |
|---|---:|
| B | 9.20 kg·m² |
| C | 22.37 kg·m² |

These numbers are **not the final values**.

The problem was not that components were arbitrarily ignored. The problem was that physically distributed masses were collapsed onto their centroids.

For example, 22 kg of wheels/tyres cannot physically occupy the exact centre of the car.

Likewise:

- the battery has finite dimensions,
- the chassis has a finite footprint,
- suspension is distributed around four corners.

Since:

\[
I_z\propto r^2
\]

collapsing those masses to the centre artificially removes a large portion of their yaw contribution.

Therefore, the earlier values should be regarded only as a **first-order centroidal point-mass estimate**.

---

# 15. Refined treatment of the battery

The current battery envelope is approximately:

- Length = 0.55 m
- Width = 0.40 m
- Mass = 45 kg

For a rectangular planform:

\[
I_{z,battery,cm}
=
\frac{45(0.55^2+0.40^2)}{12}
\]

Therefore:

\[
\boxed{
I_{z,battery,cm}=1.734\text{ kg·m}^2
}
\]

The battery contribution about the vehicle CG is then:

\[
I_{z,battery}
=
1.734
+
45d^2
\]

where \(d\) is the horizontal distance from the battery centroid to the vehicle CG.

This is more physically meaningful than treating the entire battery as a mathematical point.

---

# 16. Refined treatment of the chassis

For the current first-order model, the chassis is approximated as a rectangular distributed mass.

Assumed footprint:

- Length = 1.55 m
- Width = 1.20 m
- Mass = 28 kg

Its centroidal yaw inertia is:

\[
I_{z,chassis,cm}
=
\frac{28(1.55^2+1.20^2)}{12}
\]

giving:

\[
\boxed{
I_{z,chassis,cm}=8.966\text{ kg·m}^2
}
\]

The final vehicle model then shifts this inertia from the chassis centroid to the vehicle CG using the parallel-axis theorem.

### Limitation

A Formula Student chassis is not a uniform rectangular plate.

It is a spatial tubular/monocoque structure with:

- tubes,
- bulkheads,
- mounts,
- floor,
- side structures,
- roll structure,
- suspension pickups.

Therefore, the rectangular approximation is a **first-order engineering approximation**, not a CAD-validated inertia.

A future CAD model should replace this assumption.

---

# 17. Wheels and tyres

The 22 kg total wheel/tyre mass is split equally:

\[
m_{wheel}=22/4=5.5\text{ kg}
\]

Each wheel is placed at its actual corner location.

This matters greatly.

For example, the front-left wheel is located at:

\[
(X,Y)=(0.775,0.625)
\]

and therefore its distance from the CG is much greater than the distance of a mass located at the vehicle centre.

The four wheels are therefore modelled individually.

---

# 18. Suspension

The 16 kg suspension mass is represented as:

\[
16/4=4\text{ kg}
\]

per corner.

The four 4 kg masses are placed at the same first-order corner coordinates as the corresponding wheel assemblies.

This is an approximation because real suspension mass is distributed along:

- control arms,
- uprights,
- dampers,
- pushrods/pullrods,
- springs,
- hubs.

The corner model is nevertheless much better than placing the entire 16 kg at the vehicle centre.

---

# 19. Other fixed mass

The 15 kg "other fixed mass" category is not allowed to remain a mysterious central lump in the refined model.

For this first pass it is distributed longitudinally:

| Portion | Mass | X |
|---|---:|---:|
| Front | 4.5 kg | +0.775 m |
| Centre | 6.0 kg | 0 m |
| Rear | 4.5 kg | -0.775 m |

This is explicitly an **assumption**, not a measured property.

The category should eventually be decomposed into real components such as:

- low-voltage electronics,
- brake system hardware,
- steering system,
- sensors,
- mounting hardware,
- bodywork,
- cooling plumbing,
- connectors,
- safety equipment.

Once those components are known, their actual coordinates should replace this three-point approximation.

---

# 20. Refined Architecture B result

After:

1. calculating the vehicle CG,
2. placing wheels at their physical corners,
3. placing suspension at the physical corners,
4. distributing the "other fixed mass" longitudinally,
5. including the battery's intrinsic rectangular inertia,
6. including the chassis' intrinsic rectangular inertia,
7. applying the parallel-axis theorem,

the current refined estimate is:

\[
\boxed{
I_{z,B}\approx62.40\text{ kg·m}^2
}
\]

This is the current **first refined model**, not a CAD-certified value.

---

# 21. Refined Architecture C result

Applying the same modelling methodology to Architecture C gives:

\[
\boxed{
I_{z,C}\approx75.56\text{ kg·m}^2
}
\]

Again, this is a first refined engineering estimate.

---

# 22. Current comparison

| Quantity | Architecture B | Architecture C |
|---|---:|---:|
| Total mass | 214.5 kg | 240.0 kg |
| \(X_{CG}\) | -84.5 mm | -24.3 mm |
| \(Y_{CG}\) | +5.7 mm | 0 mm |
| Front static load | ~44.5% | ~48.4% |
| Rear static load | ~55.5% | ~51.6% |
| Refined \(I_z\) | **~62.40 kg·m²** | **~75.56 kg·m²** |

The current model therefore predicts:

\[
\frac{I_{z,C}}{I_{z,B}}
\approx1.21
\]

or approximately:

\[
\boxed{
I_{z,C}\text{ is }21\%\text{ higher than }I_{z,B}
}
\]

This is an important result because Architecture C adds a front motor while simultaneously moving the overall CG closer to the centre of the wheelbase.

The CG becomes more balanced, but the added front powertrain mass also increases the vehicle's rotational inertia.

---

# 23. Why this result is physically interesting

Architecture C demonstrates an important engineering trade-off.

### Architecture C improves:

- longitudinal mass balance,
- front/rear traction potential,
- AWD capability,
- potential torque-vectoring authority,
- front axle utilization.

### But Architecture C also increases:

- total mass,
- powertrain complexity,
- component count,
- cabling,
- cooling requirements,
- and, in this current model, yaw inertia.

Therefore:

> **A more balanced CG does not automatically mean a lower yaw moment of inertia.**

This is exactly why the project should not optimize only one metric.

The architecture changes several coupled vehicle properties simultaneously.

---

# 24. What the yaw result does NOT prove

The current result:

\[
I_{z,C}>I_{z,B}
\]

does **not** prove that Architecture B will corner faster.

Likewise, it does not prove that Architecture C will corner faster.

The actual vehicle response depends on the relationship between:

- yaw inertia,
- available tyre force,
- longitudinal load transfer,
- lateral load transfer,
- axle loads,
- motor torque,
- torque-vectoring yaw moment,
- steering input,
- tyre characteristics,
- aerodynamic effects,
- vehicle speed.

A higher-inertia vehicle can still produce a larger controllable yaw moment through torque vectoring.

That means the real question is not simply:

> "Which architecture has lower \(I_z\)?"

It is:

> **"Which architecture can generate the required yaw response most effectively for the available tyre and powertrain forces?"**

---

# 25. Critical modelling limitations

This model should be clearly labelled as a **first refined model**.

The largest remaining uncertainties are:

### 25.1 Chassis inertia

The chassis is currently represented as a rectangular distributed mass.

A real chassis should eventually be imported from CAD and its mass properties extracted.

### 25.2 Suspension inertia

Suspension is represented by four point masses.

Actual suspension geometry should eventually be used.

### 25.3 Wheel/tyre inertia

The wheels are treated as point masses for vehicle-body yaw inertia.

Their own rotational inertia about their axle is a separate phenomenon and should not be mixed with vehicle yaw inertia.

### 25.4 Cooling system

Cooling mass is currently represented as a point.

Actual:

- radiator,
- pump,
- reservoir,
- hoses,
- coolant

should eventually be represented separately.

### 25.5 "Other fixed mass"

This is the weakest current category.

It should eventually be decomposed into real components.

### 25.6 Motor geometry

The motor is currently treated as a point mass.

Its own centroidal yaw inertia is likely small compared with its vehicle-level position contribution, but it can be included once the actual motor geometry is available.

---

# 26. Reproducibility rule

Every future change to the model should follow this rule:

> **Never change a component's mass or position without recording the source or assumption.**

Each component should eventually have:

1. mass,
2. dimensions,
3. X position,
4. Y position,
5. Z position,
6. source/datasheet,
7. modelling class,
8. confidence level.

Recommended confidence labels:

| Confidence | Meaning |
|---|---|
| Verified | Directly supported by manufacturer/team/CAD data |
| Derived | Calculated from verified data |
| Assumed | Engineering assumption used because data is unavailable |
| Placeholder | Temporary value requiring replacement |

This prevents the final project from accidentally presenting assumptions as measured facts.

---

# 27. Recommended data structure for the final model

A future calculation sheet should use columns such as:

```text
Component
Architecture
Mass_kg
X_m
Y_m
Z_m
Length_m
Width_m
Representation
Source
Confidence
Notes
```

The calculation engine should then automatically calculate:

```text
Total mass
X_CG
Y_CG
Z_CG
I_z
Front load fraction
Rear load fraction
```

This makes the model auditable and prevents manual arithmetic from becoming a hidden source of error.

---

# 28. Key engineering conclusion

The refined model changes the interpretation of the architecture comparison.

The first point-mass model suggested:

- B ≈ 9.20 kg·m²
- C ≈ 22.37 kg·m²

Those numbers were useful for checking the basic calculation pipeline but were physically incomplete.

After representing major distributed and corner masses more realistically:

- **Architecture B ≈ 62.40 kg·m²**
- **Architecture C ≈ 75.56 kg·m²**

The difference is approximately:

\[
\boxed{21\%}
\]

in favour of the lower-inertia Architecture B.

However, Architecture C simultaneously achieves a more balanced longitudinal mass distribution:

\[
44.5/55.5\%\quad\rightarrow\quad48.4/51.6\%
\]

Therefore the result reveals a genuine architecture trade-off:

> **Adding a front motor moves mass toward the centre of the wheelbase and improves static balance, but the added mass and its placement can still increase total yaw inertia.**

This is exactly the type of coupled engineering trade-off the architecture study is intended to expose.

---

# 29. Next validation stage

Before using these values as final research results, the model should be validated in increasing levels of fidelity:

### Level 1: Current refined analytical model
Complete.

### Level 2: Component-level mass map
Replace "other fixed mass" with actual components.

### Level 3: CAD geometry
Extract:

- total mass,
- CG,
- inertia tensor,

directly from the vehicle CAD assembly.

### Level 4: Dynamic vehicle model
Use the measured/validated \(I_z\) inside the vehicle dynamics simulation.

### Level 5: Sensitivity study
Vary:

- battery position,
- front motor mass,
- rear motor position,
- battery mass,
- wheelbase,
- track width,
- driver position,

and determine which variables have the largest influence on \(I_z\).

---

# 30. Final statement for this stage

This model establishes a defensible basis for the yaw-inertia comparison:

**all significant masses are included, compact components are represented as point masses, distributed components receive intrinsic inertia, and corner masses are placed at their physical locations.**

The model is intentionally transparent about assumptions.

The numerical values are therefore useful as **engineering estimates**, while the final research-grade values should come from a CAD-derived mass-property model once the physical architecture is fully defined.

---

## Version history

| Version | Change |
|---|---|
| v0.1 | Initial point-mass calculation |
| v0.2 | Identified limitations of centroid-only modelling |
| v0.3 | Added distributed battery/chassis inertia |
| v0.4 | Added corner wheel/suspension masses |
| v0.5 | Added explicit assumptions, confidence levels and reproducibility framework |

