# Initial Yaw Moment of Inertia Model
## Formula Student Electric Powertrain Architecture Study

**Document status:** Initial analytical model  
**Purpose:** Establish a first-order comparison of yaw moment of inertia between Architecture B and Architecture C before increasing model fidelity.

---

# 1. Purpose

After calculating the centre of gravity, the next question is how the two powertrain architectures distribute mass around the vehicle's vertical axis.

This is quantified using the **yaw moment of inertia**, $I_z$.

Yaw moment of inertia represents the vehicle's resistance to angular acceleration about the vertical axis.

For the architecture study, it provides an initial way to quantify how changing:

- motor count,
- motor location,
- inverter location,
- cooling hardware,
- cabling,
- front powertrain mass

changes the rotational characteristics of the vehicle.

---

# 2. Definition

The physical definition of yaw inertia is:

$$
I_z=\int r^2\,dm
$$

where $r$ is the perpendicular distance between each mass element and the yaw axis.

For an initial discrete mass model:

$$
\boxed{
I_z=
\sum_i m_i
\left[
(X_i-X_{CG})^2+
(Y_i-Y_{CG})^2
\right]
}
$$

The yaw axis is the vertical Z-axis passing through the vehicle CG.

---

# 3. Why the centre of gravity is subtracted

The yaw axis is not necessarily located at the geometric centre of the wheelbase.

It passes through:

$$
(X_{CG},Y_{CG})
$$

Therefore the distance of each component from the yaw axis is:

$$
r_i=
\sqrt{
(X_i-X_{CG})^2+
(Y_i-Y_{CG})^2
}
$$

and its contribution is:

$$
I_{z,i}=m_ir_i^2
$$

This means that moving the CG changes the reference axis for every component.

The CG values from the previous model are therefore used directly.

---

# 4. Initial modelling approach

At this stage, every component is treated as a **point mass located at its estimated centre of mass**.

This is a deliberate first-order simplification.

The objective is to establish:

1. a working mathematical model,
2. a baseline comparison,
3. the dominant contribution of major components,
4. a starting point for model refinement.

The model is not intended to represent the final physical inertia of the vehicle.

---

# 5. Architecture B input

Architecture B uses the CG:

$$
X_{CG,B}=-0.0845\text{ m}
$$

$$
Y_{CG,B}=0.0057\text{ m}
$$

with total mass:

$$
M_B=214.5\text{ kg}
$$

The component coordinates are taken from the baseline vehicle mass model.

---

# 6. Component contribution equation

For each component:

$$
I_{z,i}
=
m_i
\left[
(X_i-X_{CG})^2+
(Y_i-Y_{CG})^2
\right]
$$

The total is:

$$
I_{z,B}=\sum_i I_{z,i}
$$

---

# 7. Architecture B calculation

The resulting first-order point-mass calculation gives:

$$
\boxed{
I_{z,B}\approx9.20\text{ kg}\cdot\text{m}^2
}
$$

This represents the yaw inertia of the **centroidal point-mass model**.

It should not yet be interpreted as the final physical yaw inertia of the vehicle.

---

# 8. Architecture C input

Architecture C uses:

$$
X_{CG,C}=-0.0243\text{ m}
$$

$$
Y_{CG,C}=0
$$

with total mass:

$$
M_C=240.0\text{ kg}
$$

The same point-mass methodology is applied.

---

# 9. Architecture C calculation

The resulting initial point-mass estimate is:

$$
\boxed{
I_{z,C}\approx22.37\text{ kg}\cdot\text{m}^2
}
$$

---

# 10. Initial comparison

| Parameter | Architecture B | Architecture C |
|---|---:|---:|
| Total mass | 214.5 kg | 240.0 kg |
| $X_{CG}$ | -84.5 mm | -24.3 mm |
| $Y_{CG}$ | +5.7 mm | 0 mm |
| Initial point-mass $I_z$ | **~9.20 kg·m²** | **~22.37 kg·m²** |

The initial model predicts a significantly larger yaw inertia for Architecture C.

---

# 11. Initial interpretation

The difference appears counterintuitive at first.

Architecture C moves the CG closer to the centre of the wheelbase.

One might therefore expect it to reduce yaw inertia.

However, yaw inertia is not determined by CG location alone.

It depends on:

$$
I_z=\sum mr^2
$$

Architecture C introduces substantial additional mass at the front of the vehicle.

The front motor and front differential are located approximately:

$$
X=+0.765\text{ m}
$$

from the vehicle origin.

Consequently, these components have a relatively large distance from the CG.

Because the contribution scales with $r^2$, their effect can be significant.

---

# 12. Why the initial result must be treated carefully

The initial result is useful, but there is an important modelling limitation.

Several large masses are represented at their centroids.

For example:

### Wheels and tyres

The complete 22 kg wheel/tyre mass is represented at:

$$
X=0,\qquad Y=0
$$

instead of at the four physical wheel locations.

### Suspension

The complete 16 kg suspension mass is also represented at:

$$
X=0,\qquad Y=0
$$

instead of being distributed around the four corners.

### Chassis

The 28 kg chassis mass is represented by a single centroid.

### Other fixed mass

The 15 kg miscellaneous mass is also represented at a single centroid.

This means the initial model captures the **mass-centroid locations** but not the complete spatial distribution of the vehicle mass.

---

# 13. Why this matters mathematically

Consider a simplified example.

Suppose:

$$
m=10\text{ kg}
$$

is placed at:

$$
r=0.5\text{ m}
$$

Then:

$$
I=10(0.5)^2
$$

$$
I=2.5\text{ kg}\cdot\text{m}^2
$$

If the same mass is incorrectly placed at the centre:

$$
r=0
$$

then:

$$
I=0
$$

The mass has not disappeared.

But its contribution to yaw inertia has.

This illustrates the fundamental issue:

$$
\boxed{I_z\propto r^2}
$$

Distance from the yaw axis therefore matters disproportionately.

---

# 14. What the initial model successfully establishes

Despite its limitations, the first model is useful for several reasons.

### 14.1 It establishes the calculation pipeline

$$
\text{Mass}
\rightarrow
\text{Position}
\rightarrow
\text{CG}
\rightarrow
\text{Distance from CG}
\rightarrow
I_z
$$

### 14.2 It provides an initial architecture comparison

The model indicates that Architecture C has substantially greater rotational inertia under the simplified representation.

### 14.3 It identifies the need for spatial modelling

The calculation exposes which categories cannot reasonably remain centroidal.

This becomes the basis for the next revision.

---

# 15. Initial model limitations

The initial model has four major limitations.

## 15.1 Corner masses are not spatially represented

Wheels and suspension are physically located near the four corners.

Their positions must be represented explicitly.

---

## 15.2 Distributed components have no intrinsic inertia

A battery is not a point.

It occupies a finite volume.

Likewise, the chassis occupies a substantial area.

Their own centroidal moments of inertia are therefore non-zero.

---

## 15.3 Large components are represented only by their centroid

The point-mass equation only captures the component's distance from the vehicle CG.

It does not capture the component's internal mass distribution.

---

## 15.4 "Other fixed mass" is too aggregated

A single 15 kg point does not describe the physical location of:

- electronics,
- bodywork,
- steering,
- brake hardware,
- sensors,
- mounting hardware,
- other vehicle systems.

This category needs to be decomposed or distributed in the refined model.

---

# 16. Engineering decision

The initial model should therefore **not be discarded**.

Instead, it becomes the baseline against which the refined model can be compared.

The modelling progression is:

$$
\boxed{
\text{Initial point-mass model}
\rightarrow
\text{Identify spatial limitations}
\rightarrow
\text{Refined mass representation}
}
$$

This preserves the development history of the analysis.

---

# 17. Initial result

The first analytical model gives:

$$
\boxed{
I_{z,B}\approx9.20\text{ kg}\cdot\text{m}^2
}
$$

and:

$$
\boxed{
I_{z,C}\approx22.37\text{ kg}\cdot\text{m}^2
}
$$

The initial comparison therefore suggests:

$$
I_{z,C}>I_{z,B}
$$

However, these values are classified as **first-order point-mass estimates**, not final vehicle inertia values.

---

# 18. Reason for refinement

The next model will preserve the same:

- vehicle geometry,
- component masses,
- CG methodology,
- architecture definitions,

while improving how the mass is spatially represented.

The refinement will:

1. place wheels at their physical corner coordinates,
2. distribute suspension around the four corners,
3. represent the battery as a finite rectangular mass,
4. represent the chassis as a distributed mass,
5. distribute the miscellaneous fixed mass,
6. use the parallel-axis theorem where appropriate,
7. retain explicit assumptions and uncertainty.

This allows the architecture comparison to move from a mathematical baseline toward a physically meaningful vehicle model.

---

## Version history

| Version | Change |
|---|---|
| v0.1 | Initial point-mass yaw inertia calculation |
| v0.2 | Added CG-relative coordinate formulation |
| v0.3 | Identified limitations of centroid-only mass representation |
| v0.4 | Defined requirements for refined spatial model |
