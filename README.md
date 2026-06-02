# MV Feeder Protection Study — Pandapower

Relay coordination study on a typical Eskom 11 kV radial feeder (e.g. Mpumalanga coalfields context).
Built with pandapower, Python, and JupyterLab.

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
