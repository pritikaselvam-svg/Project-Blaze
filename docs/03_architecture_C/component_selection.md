# Architecture C — Component Effects

| Component                   | Direct Effect                                 | Secondary Effect                                 | Measurement / Analysis                     |
| --------------------------- | --------------------------------------------- | ------------------------------------------------ | ------------------------------------------ |
| Front motor                 | Adds front-axle propulsion                    | Additional mass, cooling and electrical demand   | Torque, power, efficiency, temperature     |
| Rear motors                 | Provide independent rear propulsion           | Enables torque vectoring                         | Torque, speed, efficiency                  |
| Front differential          | Distributes front motor torque                | Influences front-wheel traction                  | Torque distribution, wheel slip            |
| Battery                     | Supplies total electrical power               | Major contribution to vehicle mass and CG        | Voltage, current, SOC, temperature, mass   |
| Battery position            | Changes mass distribution and HV cable length | Changes CG and yaw inertia                       | CG, yaw inertia, cable length              |
| Front inverter              | Converts DC power to front motor drive        | Adds electrical and thermal losses               | Efficiency, current, temperature           |
| Rear inverters              | Independently control rear motors             | Enable torque vectoring                          | Efficiency, current, temperature           |
| HV cables                   | Transfer power to three inverter channels     | Resistance and heat generation                   | Resistance, voltage drop, I²R losses       |
| Cooling system              | Removes component heat                        | Adds mass, pump power and packaging requirements | Coolant temperature, component temperature |
| VCU                         | Coordinates three torque channels             | Determines control complexity and response       | Execution time, control response, CAN load |
| CAN network                 | Transfers control information                 | Communication latency and bus loading            | Message rate, bus utilization, timing      |
| Additional front powertrain | Adds AWD capability                           | Additional system mass and complexity            | Mass penalty vs dynamic benefit            |

## Key Experimental Question

The additional front powertrain creates a trade-off:

Dynamic benefit

versus

Additional mass + electrical losses + thermal load + control complexity.

Architecture C is successful only if the resulting vehicle-level performance improvement justifies these additional costs.
