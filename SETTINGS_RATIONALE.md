# Protection Settings Rationale
## Mpumalanga 11 kV Radial Feeder — Overcurrent Relay Coordination Study

**Author:** Hillary Mangena  
**Date:** June 2026  
**Standard:** IEC 60255-151 | NRS 034 | SANS 10292  
**Region:** South Africa — Mpumalanga Coalfields (Eskom Distribution)

---

## 1. Feeder Description and Protection Philosophy

This feeder is a 20-kilometre radial 11 kV overhead line supplied from a 
132/11 kV, 40 MVA bulk supply point in the Mpumalanga coalfields. It serves 
a rural/agricultural load at mid-feeder and a large mine at the end of the 
feeder. The mine operates compressors, winders, and crushers — equipment 
whose loss of supply carries immediate production consequences and financial 
penalty under the supply agreement.

The protection philosophy is simple and deliberately conservative: three 
non-directional IDMT overcurrent relays (R1, R2, R3) graded in series, 
with an instantaneous high-set element at R1 and R2. The scheme is 
time-graded — the relay closest to the fault operates first, and each 
upstream relay provides backup with a minimum 0.3 second coordination 
time interval (CTI). No communication channel is used. This is the 
standard Eskom distribution protection philosophy for radial feeders 
as described in NRS 034.

The scheme protects against three-phase and phase-to-phase faults only. 
Single-phase earth fault protection on this unearthed 11 kV system 
requires a separate sensitive earth fault (SEF) or neutral voltage 
displacement (NVD) scheme and is outside the scope of this study.

---

## 2. Basis for Relay Pickup Settings (I>)

Pickup current must satisfy two constraints simultaneously: it must sit 
above the maximum load current to prevent nuisance tripping on inrush or 
overload, and it must sit well below the minimum fault current so the relay 
reliably picks up for the weakest fault condition (minimum Eskom grid infeed, 
S_sc = 120 MVA).

### R3 — Mine Incomer (Bus 3)
- Maximum load current: 78.4 A (from load flow, Line 2→3)
- Minimum 3-phase fault current at Bus 3: 627 A (min infeed case)
- Required range: above 1.2 × 78.4 = 94.1 A, below 0.8 × 627 = 501.6 A
- **Chosen: I_s = 100 A primary (PSM = 1.0 on 100/1 CT)**
- Sensitivity ratio at minimum fault: 627 / 100 = 6.3× — well above the 
  minimum recommended 1.5× sensitivity

### R2 — Mid-Feeder (Bus 2)
- Maximum load current on Line 2→3: 78.4 A
- Minimum fault current at Bus 2: 979.9 A (min infeed)
- Required range: above 94.1 A, below 783.9 A
- **Chosen: I_s = 100 A primary (PSM = 0.5 on 200/1 CT)**
- Sensitivity at minimum fault: 979.9 / 100 = 9.8×

### R1 — Substation (Bus 1)
- Maximum load current on Line 1→2: 103.6 A (carries both Bus 2 and Bus 3 loads)
- Minimum fault current at Bus 1: 4861.7 A (min infeed)
- Required range: above 124.3 A, below 3889.4 A
- **Chosen: I_s = 300 A primary (PSM = 0.5 on 600/1 CT)**
- The pickup is deliberately set at 300 A rather than the minimum 
  permissible 125 A. With a 600/1 CT, a lower pickup produces a 
  secondary setting below 0.25 A which approaches the minimum reliable 
  setting range of numeric relays. 300 A gives a clean PSM of 0.5 
  and adequate margin above load.
- Sensitivity at minimum fault: 4861.7 / 300 = 16.2×

---

## 3. Basis for TMS Settings and Curve Characteristic Choice

### Why Extremely Inverse (EI)?

The IEC Extremely Inverse curve was selected over Standard Inverse (SI) 
and Very Inverse (VI) for the following reason: mining loads produce 
high motor inrush currents (typically 6–8× full load current) on start-up. 
The EI curve has the steepest slope at low multiples of pickup — it holds 
back longer at inrush-level currents than SI or VI, reducing the risk of 
nuisance trips on motor starts while still clearing genuine faults fast 
at high multiples. This is standard practice on Eskom mining feeders.

The EI equation used throughout: t = TMS × 80 / ((I / I_s)² − 1)

### R3 TMS — Fastest Relay, Sets the Chain

R3 is the most downstream relay and has no relay below it to coordinate 
with. Its TMS is set to give the fastest acceptable operating time at 
the maximum fault current it will see:

- Target: t = 0.1 s at I_k,max = 867.2 A (Bus 3 maximum fault)
- M = 867.2 / 100 = 8.672
- TMS = 0.1 × (8.672² − 1) / 80 = 0.0928 → rounded to **TMS = 0.10**
- Verified operating time: 0.108 s at 867.2 A

### R2 TMS — Graded Above R3

R2 must clear at least CTI = 0.3 s after R3 at the same reference 
fault current (Bus 3 maximum fault, 867.2 A):

- Required R2 time: 0.108 + 0.300 = 0.408 s
- M_R2 = 867.2 / 100 = 8.672
- TMS = 0.408 × 74.2 / 80 = 0.3783 → rounded to **TMS = 0.40**
- Verified: t_R2 = 0.431 s, CTI = 0.323 s ✓

### R1 TMS — Critical Grading Point is Not Where You Expect

The natural instinct is to grade R1 against R2 at the Bus 3 fault current 
(867.2 A). This is wrong. The CTI between R1 and R2 is smallest at the 
highest fault current where both relays operate — which is the maximum 
fault current at Bus 2 (1363.9 A), where R2 operates at its fastest. 
Grading at any lower current gives an artificially wide margin that 
disappears at high fault levels.

- Critical grading point: I_k,max at Bus 2 = 1363.9 A
- R2 operating time at 1363.9 A: 0.173 s
- Required R1 time: 0.173 + 0.300 = 0.473 s
- M_R1 = 1363.9 / 300 = 4.546
- TMS = 0.473 × (4.546² − 1) / 80 = 0.1163 → rounded to **TMS = 0.125**
- Verified: t_R1 = 0.509 s, CTI = 0.336 s at 1363.9 A ✓
- CTI at Bus 3 fault (867.2 A): 0.928 s — wider as expected ✓

### CTI = 0.3 s Justification

NRS 034 permits a coordination time interval of 0.3 s for numeric 
(digital) protection relays. This is justified because numeric relays 
have timing errors below 1%, no mechanical disc overshoot, and breaker 
opening times well characterised at 60–80 ms. The traditional 0.4 s CTI 
applies to electromechanical relays where disc overshoot adds 100 ms of 
uncertainty. All relays in this scheme are assumed to be numeric 
(e.g. ABB REF615, Siemens 7SJ, or SEL-351 equivalent) consistent 
with Eskom's numeric relay replacement programme.

---

## 4. Basis for Instantaneous Element Settings (I>>)

The instantaneous element provides fast clearance for close-in faults 
without waiting for the IDMT curve. It must be set to avoid operating 
for faults in the next downstream zone — if it overreaches, it would 
trip before the downstream relay, violating selectivity.

Setting formula: **I>> = 1.25 × I_k,max at next downstream bus**

The 25% security margin covers CT measurement error (class 5P20: ≤5% 
composite error) plus relay measurement tolerance (numeric relay: ≤1%) 
plus a safety margin for DC offset effects on CT output.

### R3 — Disabled
Bus 3 is the terminal bus. There is no downstream zone to protect against 
and no relay to coordinate with. The instantaneous element is disabled. 
R3 relies entirely on its EI IDMT curve.

### R2 — I>> = 1100 A primary
- Proximity test: I_k,max(Bus 3) / I_k,max(Bus 2) = 867.2 / 1363.9 = 0.636
- 0.636 < 0.75 — sufficient discrimination exists, element applicable ✓
- I>> = 1.25 × 867.2 = 1084 A → rounded to **1100 A (5.5 A secondary)**
- Far end check: 867.2 A < 1100 A → R2 I>> does NOT trip for Bus 3 fault ✓
- Near end check: 1363.9 A > 1100 A → R2 I>> trips for Line 2→3 fault ✓

### R1 — I>> = 1700 A primary
- Proximity test: 1363.9 / 8412.7 = 0.162 < 0.75 ✓
- I>> = 1.25 × 1363.9 = 1704.9 A → rounded to **1700 A (2.83 A secondary)**
- Far end check: 1363.9 A < 1700 A → R1 I>> does NOT trip for Bus 2 fault ✓
- Near end check: 8412.7 A > 1700 A → R1 I>> trips for Line 1→2 fault ✓

---

## 5. Key Assumptions and Sensitivity

| Assumption | Value used | Sensitivity |
|---|---|---|
| Source max infeed | 250 MVA | Used for I>> settings — if grid strengthens, I>> must be reviewed |
| Source min infeed | 120 MVA | Used for pickup settings — all relays confirmed to pick up ✓ |
| CTI | 0.3 s | Assumes numeric relays throughout — tightest defensible margin |
| EI curve | IEC 60255-151 | Mine load inrush assumed 6–8× FLC; if load mix changes, curve choice should be revisited |
| Transformer earthing | Dyn11 (delta HV, star earthed LV) | Standard Eskom distribution — verify on nameplate before commissioning |
| Line temperature correction | 160°C end temperature | IEC 60865, ACSR conductor — affects minimum fault case resistance |

**The setting with least margin is R1's TMS.** At maximum infeed and Bus 2 
fault (1363.9 A), the CTI between R1 and R2 is 0.336 s — only 36 ms above 
the 0.3 s minimum. Any future load growth reducing the fault current at 
Bus 2 would widen this margin. Any future network reinforcement increasing 
fault current at Bus 2 could narrow it further — a settings review is 
triggered if S_sc at Bus 2 increases by more than 20%.

---

## 6. Known Limitation — Grid Code Backup Time

The Eskom Grid Code requires protection to clear 11 kV faults within 1.0 s 
to preserve transient stability at the 132 kV level. Under minimum infeed 
conditions (S_sc = 120 MVA), R1's backup operating time for a Bus 3 fault 
is 2.97 s — a Grid Code violation.

This is a fundamental limitation of pure IDMT grading on a long rural 
feeder. R1's pickup is 300 A, and at the minimum Bus 3 fault current of 
627 A, the multiplier M = 2.09 — close to pickup where the EI curve is 
extremely steep. Reducing TMS_R1 to meet the 1.0 s requirement would 
collapse the CTI between R1 and R2 at high fault currents.

The correct solution is a communication-assisted scheme — specifically a 
Permissive Overreach Transfer Trip (POTT) scheme, where R1 can trip in 
under 80 ms for any fault on the feeder once R2 or R3 asserts a start 
signal over a pilot channel. This is standard practice on Eskom 
transmission-connected feeders but is outside the scope of this 
overcurrent coordination study. The limitation is accepted, documented, 
and noted for the next design stage.

---

## 7. Complete Settings Table

| Relay | Location | CT ratio | CT class | I_s (A) | PSM | Curve | TMS | I>> (A) |
|---|---|---|---|---|---|---|---|---|
| R1 | Bus 1 — Substation | 600/1 A | 5P20 | 300 | 0.50 | IEC EI | 0.125 | 1700 |
| R2 | Bus 2 — Mid-Feeder | 200/1 A | 5P20 | 100 | 0.50 | IEC EI | 0.400 | 1100 |
| R3 | Bus 3 — Mine Incomer | 100/1 A | 5P20 | 100 | 1.00 | IEC EI | 0.100 | Disabled |

*All settings are primary values. Secondary values: divide by CT ratio.*  
*CTI = 0.3 s throughout (numeric relay standard, NRS 034).*  
*Verified under both maximum (250 MVA) and minimum (120 MVA) source infeed.*
