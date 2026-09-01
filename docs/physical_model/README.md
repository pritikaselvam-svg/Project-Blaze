# Physical Vehicle Model

## 1. Purpose

The physical model converts the selected electrical architecture into a vehicle-level mass and packaging model.

Each major subsystem is represented by:

* Mass
* Longitudinal position
* Lateral position
* Vertical position
* Packaging rationale

The model is used to calculate the vehicle centre of gravity, static mass distribution and yaw moment of inertia.

---

## 2. Vehicle Coordinate System

A vehicle-fixed Cartesian coordinate system is used.

| Axis   | Definition                                         |
| ------ | -------------------------------------------------- |
| X      | Longitudinal position, positive toward the front   |
| Y      | Lateral position, positive toward the vehicle left |
| Z      | Vertical position, positive upward                 |
| Origin | Geometric centre of the wheelbase at ground level  |

For the 1550 mm wheelbase:

* Front axle: X = +775 mm
* Rear axle: X = −775 mm
* Vehicle centreline: Y = 0 mm
* Ground plane: Z = 0 mm

All component locations are expressed relative to this coordinate system.

---

## 3. Mass Model

Each component is represented as a lumped mass located at its estimated centre of mass.

The total vehicle mass is:

$$
M=\sum_i m_i
$$

where \(m_i\) is the mass of component \(i\).

The same reference vehicle parameters are maintained between architectures wherever possible so that the comparison isolates the effect of the powertrain architecture.

---

## 4. Component Placement

### Architecture B

| Component        | Mass (kg) | X (mm) | Y (mm) | Z (mm) |
| ---------------- | --------: | -----: | -----: | -----: |
| Battery          |      45.0 |   −185 |      0 |    180 |
| Rear-left motor  |      3.55 |   −765 |   +450 |    203 |
| Rear-right motor |      3.55 |   −765 |   −450 |    203 |
| Rear inverter    |       6.5 |   −655 |      0 |    250 |
| Cooling system   |       3.5 |    −85 |   +350 |    220 |
| VCU              |       0.4 |   +215 |      0 |    300 |
| HV cables        |       1.5 |   −435 |      0 |    220 |
| Fuse/protection  |       1.5 |   −185 |      0 |    280 |
| Driver           |      68.0 |    +15 |      0 |    260 |
| Chassis          |      28.0 |      0 |      0 |    280 |
| Wheels/tyres     |      22.0 |      0 |      0 |    203 |
| Suspension       |      16.0 |      0 |      0 |    250 |
| Other fixed mass |      15.0 |      0 |      0 |    320 |

### Architecture C

| Component          | Mass (kg) | X (mm) | Y (mm) | Z (mm) |
| ------------------ | --------: | -----: | -----: | -----: |
| Battery            |      45.0 |   −185 |      0 |    180 |
| Front motor        |      13.5 |   +765 |      0 |    203 |
| Rear-left motor    |      3.55 |   −765 |   +450 |    203 |
| Rear-right motor   |      3.55 |   −765 |   −450 |    203 |
| Rear inverter      |      11.0 |   −655 |      0 |    250 |
| Front differential |       4.5 |   +765 |      0 |    170 |
| Cooling system     |       5.0 |    +65 |      0 |    220 |
| VCU                |       0.6 |   +215 |      0 |    300 |
| HV cables          |       2.8 |    +55 |      0 |    210 |
| Fuse/protection    |       1.5 |   −185 |      0 |    280 |
| Driver             |      68.0 |    +15 |      0 |    260 |
| Chassis            |      28.0 |      0 |      0 |    280 |
| Wheels/tyres       |      22.0 |      0 |      0 |    203 |
| Suspension         |      16.0 |      0 |      0 |    250 |
| Other fixed mass   |      15.0 |      0 |      0 |    320 |

---

## 5. Centre of Gravity

The centre of gravity is calculated using the mass-weighted average of component positions.

### Longitudinal CG

$$
X_{CG}=\frac{\sum m_iX_i}{\sum m_i}
$$

### Lateral CG

$$
Y_{CG}=\frac{\sum m_iY_i}{\sum m_i}
$$

### Vertical CG

$$
Z_{CG}=\frac{\sum m_iZ_i}{\sum m_i}
$$

---

## 6. Current Results

### Architecture B

$$
M_B=214.5\ kg
$$

$$
X_{CG,B}=-84.5\ mm
$$

$$
Y_{CG,B}=+5.7\ mm
$$

$$
Z_{CG,B}=240.5\ mm
$$

Static mass distribution:

$$
44.5\%\ Front / 55.5\%\ Rear
$$

---

### Architecture C

$$
M_C=240.0\ kg
$$

$$
X_{CG,C}=-24.6\ mm
$$

$$
Y_{CG,C}=0\ mm
$$

$$
Z_{CG,C}=237.0\ mm
$$

Static mass distribution:

$$
48.4\%\ Front / 51.6\%\ Rear
$$

---

## 7. Architecture-Level Effect

The addition of the front motor and front differential in Architecture C shifts the vehicle CG forward.

Compared with Architecture B:

$$
\Delta X_{CG}=59.9\ mm
$$

The vertical CG also decreases by approximately:

$$
3.5\ mm
$$

Architecture C therefore produces a more balanced front/rear static mass distribution while maintaining a similar CG height.

The next stage is to determine whether this redistribution produces a measurable dynamic benefit.

---

## 8. Model Limitations

The current model represents complex vehicle subsystems as lumped masses.

The following effects are not yet modelled in detail:

* Exact component geometry
* Distributed chassis mass
* Individual wheel locations
* Suspension mass distribution
* Battery cell-level mass distribution
* Aerodynamic mass/load distribution
* Dynamic load transfer
* Suspension kinematics
* Tire deformation

These will be introduced only where necessary for higher-fidelity analysis.

---

## 9. Next Analysis

The next physical-model calculation is the vehicle yaw moment of inertia.

The yaw moment of inertia will determine how the different mass distributions affect the vehicle's resistance to rotational acceleration about the vertical axis.

This allows Architecture B and Architecture C to be compared not only by static CG location, but also by their expected transient handling characteristics.
