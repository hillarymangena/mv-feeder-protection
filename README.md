# MV Feeder Protection Study — Pandapower

ABOUT
This project presents a complete overcurrent relay coordination study for an Eskom-style 11 kV radial feeder serving a mine in the Mpumalanga coalfields. The study was motivated by a realistic operational scenario: a selectivity failure at the substation relay that de-energised a mining load for 47 minutes, triggering a financial penalty under the supply agreement. Starting from a clean network model built in pandapower, the work covers load flow analysis, symmetrical and asymmetrical fault studies under both maximum (250 MVA) and minimum (120 MVA) Eskom grid infeed conditions, CT selection, IEC 60255 Extremely Inverse IDMT relay grading for three series relays, instantaneous element sizing, and time-current curve generation. Settings are verified for selectivity under four distinct fault scenarios and under worst-case minimum infeed contingency. A formal settings rationale document is included. The study identifies a Grid Code backup time violation inherent to pure IDMT grading on long rural feeders,and points to a Permissive Overreach Transfer Trip (POTT) scheme as the engineering resolution — a finding that reflects real-world constraints on utility distribution networks.
All calculations are reproducible at zero cost using Python, pandapower, and JupyterLab. The primary tooling limitation is that pandapower does not natively support dynamic simulation or full COMTRADE oscillography output — capabilities that tools like DIgSILENT PowerFactory or ETAP would provide in a professional setting, enabling transient stability verification and relay testing integration alongside the steady-state studies performed here.


## Setup
```bash
python3 -m venv protection_env
source protection_env/bin/activate
pip install -r requirements.txt
jupyter lab
```
## Transformer Nameplate Parameters — Justification

| Parameter | Value | Source |
|---|---|---|
| Rating | 40 MVA | DBR |
| Voltage ratio | 132/11 kV | DBR |
| vk_percent | 10% | DBR |
| vkr_percent | 0.5% | Derived: X/R = 20 (DBR), R = vk/20 = 10/20 |
| pfe_kw | 30 kW | Typical standard for 40 MVA Eskom distribution transformer |
| i0_percent | 0.05% | Typical standard for 40 MVA ONAN-cooled transformer |
