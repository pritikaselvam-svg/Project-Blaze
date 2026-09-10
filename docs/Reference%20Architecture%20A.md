# Reference Architecture A

## 1. Purpose

Architecture A is the **reference RWD benchmark** for the powertrain architecture study.

It represents a conventional Formula Student Electric layout built around:

- two rear-mounted, inboard electric motors
- independent left/right rear torque control
- mechanical reduction between each motor and rear wheel
- rear-wheel drive
- a centrally located TS accumulator
- rear power electronics
- no front traction motor

The purpose of Architecture A is **not** to claim that one layout is universally optimal or that every successful Formula Student team uses exactly this arrangement.

Instead, it provides a conventional, competition-oriented reference against which the project's Architecture B and Architecture C can be compared.

The 2026 Formula Student rules permit multiple electric-motor configurations and require the motor(s) to be connected to the TS accumulator through motor controllers. They also limit TS power at the TSAC outlet to 80 kW. citeturn0search36

AMK's Formula Student Racing Kit provides an established example of a rear-powertrain component ecosystem. Its DD5-14-10-POW motor is a 3.55 kg PMSM rated at 12.3 kW and capable of 21 Nm maximum torque, with a 20,000 rpm mechanical speed limit. citeturn1search29turn1search34

---

## 2. Architecture Definition

### Architecture A: Dual Inboard Rear Motor RWD

**Power flow:**

`TS accumulator → rear inverters → rear motors → reduction gearboxes → rear wheels`

### Hardware

| Component | Configuration |
|---|---|
| Motors | 2 × rear inboard PMSM |
| Motor placement | Rear-left + rear-right |
| Drive | Rear-wheel drive |
| Torque control | Independent left/right |
| Reduction | One mechanical reduction stage per motor |
| Front motor | None |
| Front differential | None |
| Main inverter system | Rear |
| Battery | Central TS accumulator |
| Power limit | 80 kW TS outlet maximum |
| Primary use | Reference benchmark |

Independent rear torque control allows the reference architecture to represent the conventional two-motor torque-vectoring concept without introducing a front driveline.

---

# 3. Rules and Packaging Basis

The 2026 rules require every TS accumulator container to lie fully within and be attached to the primary structure. In side view, the TSAC may not exceed the height of the impact structure. The surrounding structure must also protect the TSAC from impacts. citeturn0search37

Therefore, Architecture A places the accumulator behind the driver firewall and inside the vehicle's primary-structure envelope.

This is a **study packaging assumption**, not a completed chassis design or technical-inspection certification.

The reference architecture avoids introducing a front traction motor, front reduction unit or front high-voltage motor cabling into the front suspension/steering packaging region.

---

# 4. Coordinate System

The same coordinate system used throughout the project is retained:

- $X$ positive forward
- $Y$ positive to the left
- $Z$ positive upward
- origin at the geometric centre of the wheelbase at ground level

Vehicle dimensions:

- wheelbase = 1.55 m
- front axle = +0.775 m
- rear axle = -0.775 m
- front track = 1.25 m
- rear track = 1.20 m

---

# 5. Component Mass Model

A major correction from the earlier architecture comparison is important here.

The previously suggested **195–200 kg Architecture A mass cannot be used consistently with the existing 45 kg battery, 68 kg driver, 28 kg chassis, 22 kg wheels, 16 kg suspension and 15 kg fixed-mass assumptions**.

Those fixed quantities alone already total:

$M_{fixed}=45+68+28+22+16+15$

$M_{fixed}=194\,kg$

That leaves only 1–6 kg for both motors, inverters, cooling, cables, protection and gearing, which is physically inconsistent.

Therefore, this document does **not** force the 195–200 kg estimate into the model.

Instead, Architecture A uses an internally consistent first-order mass budget.

| Component | Mass (kg) | X (m) | Y (m) | Z (m) |
|---|---:|---:|---:|---:|
| TS accumulator | 45.0 | -0.185 | 0 | 0.180 |
| Rear-left motor | 3.55 | -0.765 | +0.450 | 0.203 |
| Rear-right motor | 3.55 | -0.765 | -0.450 | 0.203 |
| Rear-left reduction unit | 2.0* | -0.765 | +0.450 | 0.190 |
| Rear-right reduction unit | 2.0* | -0.765 | -0.450 | 0.190 |
| Rear inverter system | 6.0* | -0.655 | 0 | 0.250 |
| Cooling system | 3.5* | -0.085 | +0.350 | 0.220 |
| VCU | 0.4* | +0.215 | 0 | 0.300 |
| HV cables | 1.5* | -0.435 | 0 | 0.220 |
| Fuse/protection | 1.5* | -0.185 | 0 | 0.280 |
| Driver | 68.0 | +0.015 | 0 | 0.260 |
| Chassis | 28.0 | 0 | 0 | 0.280 |
| Wheels/tyres | 22.0 | distributed | distributed | 0.203 |
| Suspension | 16.0 | distributed | distributed | 0.250 |
| Other fixed mass | 15.0 | distributed | distributed | 0.320 |

\* Engineering study assumptions, not manufacturer-certified masses.

Total reference mass:

$M_A=218.0\,kg$

---

# 6. Mass Sanity Check

The mass budget can be separated into fixed vehicle mass and architecture-specific powertrain mass.

Fixed baseline:

$M_{fixed}=194\,kg$

Architecture-specific items:

$M_{architecture}=3.55+3.55+2+2+6+3.5+0.4+1.5+1.5$

$M_{architecture}=24\,kg$

Therefore:

$M_A=194+24$

$M_A=218\,kg$

This result is deliberately transparent.

It also prevents the reference architecture from being artificially made lighter simply to make it look like a benchmark.

---

# 7. Motor Reference

The AMK DD5-14-10-POW is used as a **reference component**, not as a claim that Architecture A must use AMK hardware.

Manufacturer data gives:

- mass = 3.55 kg
- rated torque = 9.8 Nm
- maximum torque = 21 Nm
- rated power = 12.3 kW
- maximum mechanical speed = 20,000 rpm
- theoretical no-load speed = 18,617 rpm

The motor is a synchronous PMSM and the AMK Formula Student Racing Kit documentation specifies liquid cooling for the DD5 configuration. citeturn1search29turn1search34

---

# 8. Motor Power Check

Using the maximum torque value:

$T_{max}=21\,Nm$

and a reference peak mechanical power of:

$P_{max}=35\,kW$

the corresponding transition speed is:

$n_{base}=\frac{60P_{max}}{2\pi T_{max}}$

$n_{base}=\frac{60(35000)}{2\pi(21)}$

$n_{base}\approx15915\,rpm$

Thus the simplified motor model can use:

- approximately constant torque below 15,915 rpm
- approximately constant power above 15,915 rpm

The manufacturer data and AMK documentation should remain the source of truth for the actual motor characteristic curve in the final simulation. citeturn1search34

---

# 9. Total Motor Capability

For two identical motors:

$P_{max,total}=2(35\,kW)$

$P_{max,total}=70\,kW$

Similarly:

$T_{max,total}=2(21\,Nm)$

$T_{max,total}=42\,Nm$

This is an important limitation of using the DD5 as the physical reference motor.

The competition-level vehicle limit is 80 kW at the TSAC outlet, but two DD5 motors at the stated reference peak capability do not themselves provide 80 kW of mechanical output. The final architecture comparison must therefore distinguish between:

1. **reference hardware capability**, and
2. **vehicle-level power ceiling**.

The 80 kW limit is a competition constraint, not a guarantee that every architecture produces 80 kW of mechanical output. citeturn0search36

---

# 10. Centre of Gravity

The longitudinal centre of gravity is:

$X_{CG}=\frac{\sum_i m_iX_i}{\sum_i m_i}$

Using the component locations above:

$X_{CG,A}\approx-0.0957\,m$

Therefore:

$X_{CG,A}\approx-95.7\,mm$

The lateral CG is approximately:

$Y_{CG,A}\approx+5.6\,mm$

The vertical CG is:

$Z_{CG,A}\approx0.2396\,m$

Therefore:

$Z_{CG,A}\approx239.6\,mm$

---

# 11. Static Front/Rear Weight Distribution

For wheelbase $L=1.55\,m$, the front axle load fraction is:

$\frac{W_f}{W}=\frac{0.775+X_{CG}}{1.55}$

Substituting Architecture A:

$\frac{W_f}{W}=\frac{0.775-0.0957}{1.55}$

$\frac{W_f}{W}\approx0.4382$

Therefore:

$W_f\approx43.8\%$

and:

$W_r\approx56.2\%$

For the 218 kg vehicle:

$W=218(9.81)$

$W\approx2138\,N$

Front static load:

$W_f\approx0.4382(2138)$

$W_f\approx937\,N$

Rear static load:

$W_r\approx1201\,N$

These are **static model outputs**, not claims of an optimum handling balance.

---

# 12. Yaw Moment of Inertia

The yaw inertia is calculated about the vehicle CG.

For a point mass:

$I_z=\sum_i m_i[(X_i-X_{CG})^2+(Y_i-Y_{CG})^2]$

Distributed components use intrinsic inertia plus the parallel-axis theorem:

$I_z=I_{z,cm}+md^2$

For the battery, approximated as a 0.55 m × 0.40 m rectangular mass:

$I_{z,batt,cm}=\frac{m(L^2+W^2)}{12}$

$I_{z,batt,cm}=\frac{45(0.55^2+0.40^2)}{12}$

$I_{z,batt,cm}\approx1.734\,kg\,m^2$

For the chassis, approximated as a 1.55 m × 1.20 m rectangular mass:

$I_{z,chassis,cm}=\frac{28(1.55^2+1.20^2)}{12}$

$I_{z,chassis,cm}\approx8.966\,kg\,m^2$

Wheels and suspension are distributed to the physical wheel locations:

- FL = $(+0.775,+0.625)$ m
- FR = $(+0.775,-0.625)$ m
- RL = $(-0.775,+0.600)$ m
- RR = $(-0.775,-0.600)$ m

The current refined first-order model gives:

$I_{z,A}\approx64.87\,kg\,m^2$

This is an engineering estimate based on the stated mass distribution, not a CAD-derived inertia tensor.

---

# 13. Torque Vectoring

Architecture A uses independent rear motor control.

Total rear torque is:

$T_{rear}=T_{RL}+T_{RR}$

During straight-line operation:

$T_{RL}\approx T_{RR}$

During cornering, an intentional torque difference can generate a yaw moment.

For a simplified rear-track model:

$M_z\approx\frac{t_r}{2}(F_{RR}-F_{RL})$

where:

- $t_r$ = rear track
- $F_{RR}$ = right-rear longitudinal tyre force
- $F_{RL}$ = left-rear longitudinal tyre force

This provides the basis for the later torque-vectoring and yaw-moment model.

---

# 14. Rules-Compliance Boundary

Architecture A is intended to be a **rules-aware reference architecture**, not a completed Technical Inspection submission.

The model explicitly respects the major architectural constraints relevant to the study:

- electric motors only
- motors connected through motor controllers
- maximum TS power at the TSAC outlet of 80 kW
- TSAC located within the primary-structure envelope
- TSAC mechanically protected
- accumulator separated from the cockpit by the required safety structure

The 2026 rules also require the TSAC design, calculations/tests and physical configuration to be documented in the ASES. citeturn0search36turn0search37

A final competition car would still require complete structural, electrical, accumulator and safety verification.

---

# 15. Why Architecture A Is the Reference

Architecture A establishes a conventional baseline with:

- rear-only traction
- two independently controlled rear motors
- inboard powertrain mass
- mechanical reduction
- no front traction motor
- no front driveline packaging burden

It is therefore useful as a control architecture.

The comparison becomes:

| Parameter | Architecture A |
|---|---:|
| Drive | RWD |
| Motors | 2 rear |
| Motor placement | Inboard |
| Torque control | Independent rear |
| Front traction motor | No |
| Reference motor | AMK DD5-14-10-POW |
| Vehicle mass | **218.0 kg** |
| $X_{CG}$ | **−95.7 mm** |
| $Z_{CG}$ | **239.6 mm** |
| Front static load | **43.8%** |
| Rear static load | **56.2%** |
| Refined $I_z$ | **64.87 kg·m²** |
| Reference peak motor power | **70 kW total** |
| Competition TS power ceiling | **80 kW** |

---

# 16. Role in the Overall Study

Architecture A is not intended to replace B or C.

It gives the study a third point:

**A = conventional reference**

**B = simplified dual-rear-motor variant**

**C = hybrid/AWD concept**

The research question therefore becomes:

> **How much performance is gained or lost when moving away from a conventional dual-inboard-rear-motor Formula Student architecture, and what vehicle-dynamics, electrical, thermal and packaging penalties accompany that change?**

The final comparison should use the same clearly stated vehicle envelope, battery assumptions and performance constraints wherever possible.

---

# 17. Important Modeling Discipline

Architecture A must not be treated as automatically “better” because it is the reference.

Likewise, Architecture C must not be treated as automatically “better” because it has more driven wheels.

The simulation will determine the trade-off.

The main comparison variables are:

- mass
- centre of gravity
- yaw inertia
- tyre load distribution
- traction utilisation
- torque distribution
- electrical losses
- battery current
- thermal loading
- acceleration
- energy consumption
- cornering behaviour
- lap time

The purpose of Architecture A is to provide a physically interpretable benchmark against which these effects can be measured.
