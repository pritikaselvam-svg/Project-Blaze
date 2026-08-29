| Component         | Initial selection                                              | Engineering rationale                                                  |
| ----------------- | -------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Battery chemistry | NMC Li-ion                                                     | High specific energy and power capability                              |
| Pack architecture | ~400–600 V class                                               | Compatible with the study's defined system-voltage range               |
| Motor             | AMK dual-motor configuration                                   | Enables independent rear-wheel torque control                          |
| Inverter          | Matched AMK inverter(s)                                        | Motor/inverter compatibility and integrated control                    |
| Battery position  | **Low-central**                                                | Balance between CG, mass distribution, yaw inertia and HV cable length |
| Cooling           | Single liquid-cooling loop                                     | Reduces subsystem count while providing thermal management             |
| Radiator          | Sidepod-mounted                                                | Packaging and aerodynamic integration                                  |
| HV cabling        | 25 mm² copper cable, **subject to current/thermal validation** | Initial low-resistance, flexible HV connection                         |
| HV protection     | 200 A-class fuse, **subject to fault/current analysis**        | Initial protection concept                                             |
| HV connectors     | HVSL-class connector with HVIL                                 | Safe HV connection/disconnection                                       |
| VCU               | Custom controller based on Teensy 4.1                          | Flexible embedded control development                                  |
| CAN               | Dual-bus architecture                                          | Separates powertrain control traffic from telemetry/BMS traffic        |
| CAN wiring        | Linear CAN bus + twisted pair + termination                    | Standard robust CAN physical-layer approach                            |
