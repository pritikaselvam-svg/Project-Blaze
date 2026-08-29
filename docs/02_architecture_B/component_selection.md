| Component               | Locked/working selection            | Status                              |
| ----------------------- | ----------------------------------- | ----------------------------------- |
| Battery chemistry       | **NMC**                             | ✅                                   |
| Cell                    | **Molicel P45B 21700**              | ✅ candidate                         |
| Configuration           | **130S4P**                          | ⚠️ candidate                        |
| Nominal voltage         | **468 V**                           | ✅ calculated                        |
| Maximum voltage         | **546 V**                           | ✅ calculated                        |
| Nominal energy          | **8.42 kWh**                        | ✅ calculated                        |
| Cell count              | **520**                             | ✅ calculated                        |
| Cell-only mass          | **36.4 kg**                         | ✅ calculated                        |
| Pack current capability | **180 A nominal cell-limit basis**  | ⚠️ needs operating-point validation |
| Total peak power        | **80 kW**                           | ✅                                   |
| Initial motor split     | **40 + 40 kW**                      | ✅ architecture assumption           |
| Motor                   | **AMK DD5 / exact R25 variant TBD** | ⚠️ datasheet validation             |
| Inverter                | **AMK matched inverter**            | ⚠️ exact model TBD                  |
| Battery position        | **Low-central**                     | ✅ design choice                     |
| Cooling                 | **Single liquid loop**              | ⚠️ thermal validation               |
| Radiator outlet         | **25°C model assumption**           | ⚠️                                  |
| Motor coolant inlet     | **32°C model assumption**           | ⚠️                                  |
| HV cable                | **25 mm²**                          | ✅ calculation verified by you       |
| Fuse                    | **200 A-class Bussmann**            | ⚠️ datasheet validation             |
| HV connectors           | **Amphenol HVSL-class**             | ⚠️ exact part TBD                   |
| VCU                     | **Teensy 4.1-based custom board**   | ✅ concept                           |
| CAN                     | **Dual-bus**                        | ✅ architecture choice               |
