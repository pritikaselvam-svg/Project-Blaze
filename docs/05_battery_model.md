# 05 — Battery Model

## 1. Purpose

This model evaluates the electrical behaviour of the 130S4P traction battery used as the common battery assumption for the Formula Student EV architecture comparison.

It returns:

- terminal voltage
- battery current
- voltage sag
- SOC
- electrical loss
- heat generation
- cell current
- operating constraints

---

# 2. Battery configuration

Selected cell:

**Molicel P45B**

Baseline data:

| Parameter | Value |
|---|---:|
| Chemistry | NMC |
| Nominal voltage | 3.6 V |
| Nominal capacity | 4.5 Ah |
| Continuous discharge rating | 45 A |
| Nominal energy | 16.2 Wh |
| Maximum stated cell mass | 70 g |

Pack:

$$
130S4P
$$

Total cells:

$$
N_{cell}=130\times4=520
$$

---

# 3. Nominal voltage

$$
V_{pack,nom}=130(3.6)
$$

$$
\boxed{V_{pack,nom}=468\text{ V}}
$$

Nominal voltage is not the instantaneous terminal voltage.

---

# 4. Pack capacity

$$
Q_{pack}=4(4.5)
$$

$$
\boxed{Q_{pack}=18\text{ Ah}}
$$

---

# 5. Nominal energy

$$
E_{nom}=V_{nom}Q_{pack}
$$

$$
E_{nom}=468(18)
$$

$$
\boxed{E_{nom}=8.424\text{ kWh}}
$$

This is nominal stored energy, not necessarily usable race energy.

---

# 6. Cell-only mass audit

Using 70 g maximum cell mass:

$$
m_{cells}=520(0.070)
$$

$$
\boxed{m_{cells}=36.4\text{ kg}}
$$

The vehicle model currently uses:

$$
\boxed{m_{battery}=45.0\text{ kg}}
$$

Therefore:

$$
m_{noncell}=45.0-36.4
$$

$$
\boxed{m_{noncell}=8.6\text{ kg}}
$$

The 8.6 kg is an engineering mass budget, not a verified component-level mass breakdown.

---

# 7. State of charge

Define:

$$
SOC=\frac{Q_{remaining}}{Q_{usable}}
$$

with:

$$
0\leq SOC\leq1
$$

For discharge:

$$SOC_{k+1}$$
=
$$SOC_k$$
-
$$\frac{I_{batt,k}\Delta t}
{Q_{pack}3600}$$

A more complete model can include coulombic efficiency:

$$SOC_{k+1}$$
=
$$SOC_k$$
-
$$\frac{I_{batt,k}\Delta t}
{\eta_{coul}Q_{pack}3600}$$

The sign convention must remain consistent.

---

# 8. Open-circuit voltage

Cell OCV is a function of SOC and temperature:

$$V_{OCV}=f(SOC,T)$$

For a first-order model:

$$V_{OCV}=f(SOC)$$

For the series pack:

$$V_{OCV,pack}$$
=
$$130V_{OCV,cell}$$

The final implementation should use a measured or manufacturer-derived OCV-SOC curve rather than inventing a linear relationship.

---

# 9. Internal resistance

Effective cell resistance varies with operating condition:

$$R_{cell}=f(SOC,T,I)$$

For a first-order model:

$$R_{cell}=R_0$$

For a refined model:

$$R_{cell}=R(SOC,T)$$

Pack resistance is approximately:

$$R_{pack}$$
=
$$\frac{N_{series}}{N_{parallel}}R_{cell}$$

Therefore:

$$R_{pack}$$
=
$$\frac{130}{4}R_{cell}$$

This assumes identical cells and balanced current sharing.

---

# 10. Battery terminal voltage

During discharge:

$$V_{terminal}$$
=
$$V_{OCV,pack}$$
-
$$I_{batt}R_{pack}$$

The actual battery voltage can also depend on polarization, diffusion, temperature, SOC, current history and cell imbalance.

---

# 11. Battery current at 80 kW

At nominal voltage:

$$I_{batt}$$
=
$$\frac{80000}{468}$$

$$\boxed{I_{batt}\approx171.0\text{ A}}$$

Per parallel cell:

$$I_{cell}$$
=
$$\frac{171.0}{4}$$

$$\boxed{I_{cell}\approx42.75\text{ A}}$$

This is below the stated 45 A continuous discharge rating under the manufacturer's specified conditions, but current alone does not establish endurance capability.

---

# 12. Voltage-dependent current

At 400 V:

$$I_{batt}$$
=
$$\frac{80000}{400}$$

$$\boxed{I_{batt}=200\text{ A}}$$

Therefore:

$$I_{cell}$$
=
$$\frac{200}{4}$$

$$\boxed{I_{cell}=50\text{ A}}$$

This exceeds the stated 45 A continuous cell-current rating.

Therefore the model cannot assume that 80 kW is continuously available at every battery voltage.

---

# 13. Resistive battery loss

Pack-level loss:

$$P_{battery,loss}$$
=
$$I_{batt}^2R_{pack}$$

At cell level:

$$P_{cell,loss}$$
=
$$I_{cell}^2R_{cell}$$

Total cell resistive loss:

$$P_{cells,loss}$$
=
$$N_{series}N_{parallel}
I_{cell}^2R_{cell}$$

Equivalent pack expression:

$$P_{cells,loss}$$
=
$$I_{batt}^2
\frac{N_{series}}{N_{parallel}}
R_{cell}$$

This heat generation feeds the thermal model.

---

# 14. Important heat interpretation

The quantity:

$$
I_{cell}^2R_{cell}
$$

is the instantaneous resistive heat generated under the assumed resistance.

It does **not** prove that the battery continuously generates that amount of heat.

Actual heat depends on:

- SOC
- temperature
- current
- pulse duration
- impedance
- current sharing
- thermal gradients

Therefore the refined model uses:

$$
P_{heat}=I^2R(SOC,T,I)
$$

---

# 15. Energy removed from the battery

Electrical energy delivered is:

$$E_{out}$$
=
$$\int P_{batt}(t)\,dt$$

For discrete simulation:

$$E_{out}$$
=
$$\frac{1}{3600}
\sum_kP_{batt,k}\Delta t$$

when power is in watts and time is in seconds.

---

# 16. Usable energy

Theoretical nominal energy:

$$E_{nom}=8.424\text{ kWh}$$

A simplified usable-energy estimate is:

$$E_{usable}$$
=
$$E_{nom}(SOC_{initial}-SOC_{minimum})$$

Actual usable energy is lower or different because of:

- voltage sag
- minimum cell voltage
- temperature
- current-dependent power capability
- cell imbalance
- controller cutback

---

# 17. Cell current constraint

For 4P:

$$I_{cell}$$
=
$$\frac{I_{pack}}{4}$$

Baseline current constraint:

$$I_{cell}\leq45\text{ A}$$

A refined controller constraint is:

$$I_{cell,max}=f(SOC,T)$$

so allowable current can be reduced under adverse operating conditions.

---

# 18. Cell voltage constraint

For 130 series cells:

$$V_{cell}$$
=
$$\frac{V_{pack}}{130}$$

The BMS must ensure that no individual cell violates its permitted voltage range.

The final model should therefore track:

$$V_{cell,min}$$

rather than relying only on average pack voltage.

---

# 19. Cell imbalance

The baseline model assumes:

$$V_1=V_2=\cdots=V_{130}$$

and equal current sharing among parallel cells.

A future refined model can introduce:

$$V_i\neq V_j$$

and:

$$I_{cell,i}\neq I_{cell,j}$$

This allows weakest-cell behaviour to be investigated.

---

# 20. Battery model outputs

| Quantity | Symbol |
|---|---|
| SOC | $SOC$ |
| OCV | $V_{OCV}$ |
| Terminal voltage | $V_{terminal}$ |
| Pack current | $I_{batt}$ |
| Cell current | $I_{cell}$ |
| Pack resistance | $R_{pack}$ |
| Battery loss | $P_{battery,loss}$ |
| Energy delivered | $E_{out}$ |
| Cell temperature | $T_{cell}$ |
| Minimum cell voltage | $V_{cell,min}$ |

---

# 21. Coupling with the electrical model

```text
Power demand
     ↓
Battery current
     ↓
SOC + temperature
     ↓
Battery resistance
     ↓
Voltage sag
     ↓
Available battery power
     ↓
Motor/inverter operating point
     ↓
Updated power demand
```

---

# 22. Validation hierarchy

### Level 1 — First-order

$$V_{battery}=V_{nom}$$

$$R_{cell}=R_0$$

### Level 2 — SOC-dependent

$$V_{OCV}=f(SOC)$$

$$R=R(SOC)$$

### Level 3 — SOC + temperature dependent

$$V_{OCV}=f(SOC,T)$$

$$R=R(SOC,T)$$

### Level 4 — Experimental

Replace assumptions with:

- discharge data
- impedance measurements
- temperature measurements
- pack-level test data

---

# 23. Engineering conclusion

The 130S4P configuration provides:

$$\boxed{468\text{ V nominal}}$$

$$\boxed{18\text{ Ah}}$$

$$\boxed{8.424\text{ kWh nominal energy}}$$

and:

$$\boxed{42.75\text{ A/cell at 80 kW and 468 V}}$$

At 400 V, the same 80 kW demand requires:

$$
\boxed{50\text{ A/cell}}
$$

Therefore the final vehicle simulation must account for voltage, SOC, temperature and current limits rather than treating 80 kW as universally available.
