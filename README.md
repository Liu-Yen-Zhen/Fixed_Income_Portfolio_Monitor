# Fixed Income Portfolio Monitoring Dashboard (One-click)

A lightweight, end-to-end **fixed income portfolio monitoring** prototype that reproduces a typical buy-side monitoring workflow:
**positions → risk metrics → rule-based alerts → trend tracking → rebalance proposal → post-trade validation**.

This project is designed to resemble the daily/weekly monitoring pack used by PM/Investment Ops:
- **Active weight (vs benchmark proxy)** monitoring and top deviations
- **Duration / Duration active** tracking
- **DV01** exposure and concentration
- **Rule-based alerts** (WARN / BREACH) and weekly trends
- **Rebalance proposal** to reduce active deviations under constraints

---

## What this repo contains

- `Fixed_Income_Portfolio_Monitor_one_click.ipynb`  
  One-click notebook that:
  1) generates demo reference data (security master, duration table, benchmark weights)  
  2) downloads market prices via Yahoo Finance  
  3) computes portfolio weights, duration, DV01, benchmark proxy, and active weights  
  4) triggers alerts and exports **CSV + PNG/PDF charts** to `outputs/`  
  5) optionally runs a rebalance and compares **PRE vs POST vs POST (Constrained)**

- `outputs/`  
  Auto-export folder for charts and tables.

---

## Key concepts / terminology

- **Benchmark proxy**: A practical benchmark representation using a weighted basket (e.g., core rates + IG credit + TIPS + cash proxy) when the official benchmark data is not directly available in the data source.
- **Active weight**: `portfolio_weight - benchmark_weight`. Top absolute deviations are primary monitoring drivers.
- **Duration**: interest-rate sensitivity measure.  
- **Duration active**: `portfolio_duration - benchmark_duration`.
- **DV01**: approximate PV change for a 1bp move in yield (rate risk contribution proxy).
- **WARN vs BREACH**: two-tier severity for monitoring escalation.

---

## Monitoring outputs (examples)

Generated artifacts include:
- One-page monitoring summary (active weight / duration vs benchmark / DV01 / summary tables)
- Weekly alert trends (total alerts, alert-days, top rules)
- CSV exports for positions, exposures, alerts, and (optional) rebalance orders

All charts/tables are exported to `outputs/` with timestamped filenames.

---

## How to run (one-click)

### Option A — Jupyter Notebook
1. Open `Fixed_Income_Portfolio_Monitor_one_click.ipynb`
2. Run all cells (Kernel → Restart & Run All)
3. Check exported outputs in `outputs/`

### Option B — Local environment
```bash
pip install -r requirements.txt
jupyter notebook
