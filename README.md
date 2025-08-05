# Imbalance‑Aware Execution Engine

A research‑grade prototype of the Lehalle‑Neuman **signal‑aware optimal execution** model.  The goal is to evolve this repo from an educational sandbox (paper‑trading only!) into a deployable micro‑service that can slice institutional‑sized orders while reacting to real‑time order‑book imbalance.

---

## 1  Project goals

| Phase  | Objective                                                                                                                         |
| ------ | --------------------------------------------------------------------------------------------------------------------------------- |
| **P0** | Re‑implement Cartea–Jaimungal (CJ) instantaneous‑impact strategy driven by live imbalance signal; back‑test on historic LOB data. |
| **P1** | Extend to Gatheral–Schied–Slynko (GSS) transient‑impact kernel with closed‑form OU solution; paper‑trade on exchange simulator.   |
| **P2** | Add adaptive limit/market order placement, queue‑position modelling, auto‑recalibration of parameters.                            |
| **P3** | Production‑hardening: risk controls, logging, Kubernetes deployment, CI/CD, live capital.                                         |

*(We are currently at ****P0****.)*

---

## 2  Directory layout (proposed)

```text
.
├── data/                 # raw & processed LOB datasets (git‑ignored)
├── notebooks/            # exploratory analyses & calibration experiments
├── src/
│   ├── signals/          # streaming imbalance calculation
│   ├── execution/        # CJ & GSS rate calculators, slicing logic
│   ├── order_router/     # limit/market decision rules + exchange adapters
│   ├── backtest/         # event‑driven simulator, metrics
│   └── config/           # YAML/JSON default parameters
├── tests/                # pytest unit & integration tests
├── scripts/              # helper CLIs (download‑data, backtest‑run, etc.)
├── requirements.txt      # pinned runtime deps
└── README.md             # this file
```

---

## 3  Quick‑start (local dev)

```bash
# 1  Create Python env (≥3.9 recommended)
python -m venv .venv
source .venv/bin/activate
pip install --upgrade pip

# 2  Install deps (edit requirements.txt as we go)
pip install -r requirements.txt

# 3  Run unit tests
pytest -q
```

> **Note**: large LOB parquet/CSV files live in `data/` and are excluded in `.gitignore`.  We’ll add a helper script to fetch sample datasets automatically.

---

## 4  First milestone checklist (P0)

4  First milestone checklist (P0)

Module

File(s) / task

Done?

signals

imbalance.py – streaming bid/ask imbalance;ou_calibration.py – rolling γ, σ MLE

☐

execution

cj_rate.py – analytic  for CJ model;slicer.py – discretise  into child orders

☐

backtest

Event‑driven simulator that replays historic quotes & trades;metrics: implementation shortfall, VWAP slippage, fill ratio

☐

order_router

Simple LO/MO heuristic + mock exchange adapter

☐

tests & CI

pytest unit tests ≥85 % coverage;GitHub Action running tests + flake8

☐

When every box is ✔︎ we’ll tag v0.1.

When each box is green we cut the **v0.1** tag.


