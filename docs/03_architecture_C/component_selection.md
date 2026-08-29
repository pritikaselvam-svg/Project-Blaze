# Architecture C — Component Selection

| Component / Subsystem | Initial Design Choice    | Engineering Rationale                                                                                          | Status                |
| --------------------- | ------------------------ | -------------------------------------------------------------------------------------------------------------- | --------------------- |
| Total power           | 80 kW                    | Maintains the same vehicle-level power constraint                                                              | 🟢                    |
| Power allocation      | 30 kW front / 50 kW rear | Initial rear-biased allocation to account for acceleration load transfer                                       | 🟢 Initial hypothesis |
| Front motor           | Axial-flux PMSM          | High torque density and compact packaging                                                                      | 🟡 Candidate          |
| Front motor target    | 30 kW                    | Provides front-axle traction without allocating excessive power to the unloaded front axle during acceleration | 🟢                    |
| Front differential    | Mechanical differential  | Required to distribute torque from one front motor to two front wheels                                         | 🟡                    |
| Rear motors           | 2 × independent PMSMs    | Enables independent left/right rear torque control                                                             | 🟢                    |
| Rear power allocation | 25 kW + 25 kW            | Equal initial allocation for experimental comparison                                                           | 🟢                    |
| Battery chemistry     | NMC                      | High specific energy and power capability                                                                      | 🟢                    |
| Battery cells         | Molicel P45B             | High-discharge cylindrical NMC cell                                                                            | 🟢 Candidate          |
| Battery configuration | 130S4P                   | Same reference configuration as Architecture B                                                                 | 🟡                    |
| Battery position      | Low-central              | Supports low CG, balanced mass distribution and low yaw inertia                                                | 🟢                    |
| VCU                   | NXP S32K3 dual-core      | Automotive-grade processing platform for multi-motor control                                                   | 🟢                    |
| CAN                   | Three-bus architecture   | Separates front, rear and BMS/telemetry communication                                                          | 🟡                    |
| Cooling               | Zoned liquid cooling     | Separates front and rear thermal loads                                                                         | 🟡                    |
| HV cabling            | 25 mm² copper            | Common reference cable specification                                                                           | 🟢                    |
| HV protection         | 200 A-class fuse         | Initial protection architecture                                                                                | 🟡                    |
| HV connectors         | HVSL-class connectors    | HV connection and interlock                                                                                    | 🟡                    |

## Important Experimental Principle

Wherever practical, Architecture C retains the same battery, cable, protection and vehicle-level constraints as Architecture B.

This prevents unrelated component changes from influencing the comparison between architectures.

The primary independent variable is therefore the powertrain architecture.
