# Architecture C — 3-Motor AWD

## Concept

Architecture C investigates a three-motor all-wheel-drive Formula Student Electric powertrain while maintaining the same 80 kW total power limit used for the other architectures.

The architecture consists of:

* One centrally mounted front motor
* Two independently controlled rear motors
* A mechanical front differential
* Independent inverter control
* A low-central battery
* An automotive-grade VCU

The objective is to determine whether distributing the same total available power across both axles can improve vehicle performance enough to justify the additional motor, inverter, cabling, cooling and control complexity.

---

## Architecture

### Front Axle

* 1 × axial-flux PMSM
* Target power: 30 kW
* Mechanical differential
* Front-left and front-right wheels driven through the differential

### Rear Axle

* 2 × independent PMSMs
* Target power: 25 kW per motor
* Independent left/right torque control
* Rear torque-vectoring capability

### Total Power

$$
P_{total}=30+25+25=80\text{ kW}
$$

The 80 kW value represents the vehicle-level power allocation and does not imply that each selected motor must continuously produce its allocated value.

---

## Battery

Architecture C retains the same battery technology and reference configuration as Architecture B to prevent the battery technology from becoming a confounding variable.

### Reference battery

* Chemistry: NMC
* Cell: Molicel P45B
* Configuration: 130S4P
* Total cells: 520
* Nominal voltage: 468 V
* Maximum voltage: 546 V
* Capacity: 18 Ah
* Nominal energy: 8.42 kWh
* Cell-only mass: approximately 36.4 kg

The 130S4P configuration remains a candidate and requires current, thermal and operating-point validation.

---

## Battery Position

The battery is positioned low and centrally within the vehicle envelope.

The position is intended to:

* Maintain balanced mass distribution
* Reduce centre of gravity
* Reduce yaw moment of inertia
* Keep HV cable distances manageable
* Support good overall vehicle dynamics

The suitability of this position will be evaluated through calculation and simulation.

---

## Vehicle Control

A dual-core NXP S32K3-based VCU is selected for the Architecture C control system.

The VCU coordinates:

* Front motor torque
* Rear-left motor torque
* Rear-right motor torque
* Accelerator input
* Wheel-speed information
* Vehicle-state information
* Torque distribution
* Power limitation
* Safety/interlock states

The control system therefore has three independently commanded powertrain channels.

---

## CAN Architecture

A three-bus CAN architecture is proposed:

* CAN 1 — Front powertrain
* CAN 2 — Rear powertrain
* CAN 3 — BMS and telemetry

Bus loading, message timing and communication requirements will be evaluated before the architecture is finalized.

---

## Cooling

Architecture C initially uses a zoned liquid-cooling strategy.

The front motor is treated as a separate thermal zone from the rear powertrain.

The objective is to investigate whether separating the front and rear thermal systems can reduce coolant-routing length and packaging complexity.

The thermal model will determine whether independent cooling loops are actually advantageous.

---

## Experimental Hypothesis

Architecture C hypothesizes that adding a front motor to the dual-rear-motor architecture can improve traction and vehicle dynamics by distributing the available 80 kW across both axles.

However, the additional motor and associated hardware also introduce:

* Additional mass
* Additional electrical losses
* Additional cooling requirements
* Additional HV cabling
* Additional control complexity

Therefore, Architecture C will only be considered superior if its dynamic-performance gains outweigh these penalties.

---

## Primary Evaluation Parameters

The architecture will be evaluated using:

1. Vehicle speed / lap performance
2. Acceleration
3. Energy consumption
4. Electrical efficiency
5. Voltage drop
6. Electrical losses
7. Thermal behaviour
8. Centre of gravity
9. Mass distribution
10. Yaw moment of inertia
11. Control complexity

---

## Current Status

Architecture C has been conceptually defined.

Component validation, packaging, mass modelling, electrical calculations, thermal analysis and vehicle-level simulation remain to be completed.
