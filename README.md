# Fixed Income Portfolio Monitoring Dashboard (MVP)

## Project Overview
This project is an automated portfolio monitoring tool designed for Fixed Income ETF portfolios. It calculates key risk metrics such as **Duration**, **DV01**, and **Active Weights** against a benchmark, simulating the daily workflow of a portfolio manager.

## Key Features
- **Automated Data Pipeline:** Fetches historical data using Yahoo Finance API (`yfinance`).
- **Risk Analysis:** Calculates Portfolio Duration and DV01 (Dollar Value of a 01) to monitor interest rate sensitivity.
- **Compliance Engine:** Automated alerts for constraints (e.g., Cash < 3%, Single Issuer > 35%).
- **Reporting:** Generates visualization for Active Exposure and Risk breakdown.

## Tech Stack
- **Python 3.11
- **Pandas / NumPy:** Data manipulation and financial calculations.
- **Yfinance:** Market data extraction.
- **Matplotlib:** Data visualization.

## How to Run
1. Install dependencies:
   ```bash
   pip install -r requirements.txt

# Fixed Income Portfolio Monitoring & Rebalancing (APM-style)

This project is a **Fixed Income portfolio monitoring** and **rebalance decision** prototype that mimics an APM’s daily workflow:
- monitor holdings vs. benchmark
- compute interest-rate risk proxies (Duration, DV01)
- generate guideline alerts (breach/warn)
- produce a rebalance order blotter
- validate post-trade risk / compliance outcomes (before vs. after)

> **Note:** This repo uses **bond ETFs as exposure proxies** (rates/credit/TIPS/cash) and a **monthly-updated duration table** for demonstration. The architecture is designed to be replaceable with production data sources (factsheets / risk systems).

---

## Key Outputs
### Monitoring
- **Holdings / Weights / Active Weight** vs benchmark  
- **Sector / Rating exposure** snapshots  
- **Portfolio Duration** and **DV01** proxies  
- **Guideline alerts** (breach/warn) and weekly trends

### Rebalancing
- **Benchmark rebalance** (target = benchmark weights)  
- **Constrained rebalance** (adds concentration limit: `AGG <= 35%`)  
- **Order blotter** (BUY/SELL shares + notional)  
- **Post-trade validation** (active deviation, duration alignment, breach resolution)

---

## Method Overview (Workflow)
1. **Load market prices** (daily close)  
2. Build **portfolio market value** and **weights** from fixed shares  
3. Load **duration (monthly)** → forward-fill to daily → compute:
   - Portfolio Duration = sum(weight × duration)
   - DV01 proxy = market_value × duration × 1bp
4. Define **benchmark weights** (proxy index) and compute:
   - Active Weight = portfolio_weight − benchmark_weight
5. Apply **guidelines** to generate alerts:
   - Single-name weight limit
   - Minimum cash proxy (BIL)
   - Duration band
   - Active weight deviation threshold
6. Generate **rebalance orders** and simulate **post-trade** weights/risk  
7. Compare **PRE vs POST vs POST (Constrained)** outcomes and export charts

---

## Results Summary (PRE vs POST vs POST-Constrained)
### PRE (Initial)
- Multiple **Active Weight Deviation WARNs** across several holdings (AGG/BIL/HYG/TLT)
- Duration active was meaningfully off benchmark

### POST (Target = Benchmark)
- Active weights were largely neutralized and duration alignment improved significantly  
- However, a **Single-name concentration breach** appeared because the benchmark itself is concentrated in **AGG (~45%)**, exceeding the guideline cap (35%)

### POST (Constrained Rebalance: AGG capped at 35%)
- Concentration breach was resolved by enforcing **AGG <= 35%**
- A **structural active deviation** remained for AGG (~ -10% active), which is expected:
  - It reflects the trade-off between **tracking benchmark** and **meeting compliance limits**
  - This is tracked as a WARN rather than a breach

**Key takeaway:** The constrained rebalance demonstrates realistic APM decision-making: **compliance-first**, while still remaining as close as possible to the benchmark structure.

---

## Figures (Exported)
After running the notebook, charts and tables are exported to `outputs/`:

- Active Weight Comparison  
  - `outputs/YYYYMMDD_active_weight_comparison.png`
- Breach/Warn Counts by State  
  - `outputs/YYYYMMDD_breach_warn_counts_by_state.png`
- Sector Exposure Comparison  
  - `outputs/YYYYMMDD_sector_exposure_comparison.png`
- Rating Exposure Comparison  
  - `outputs/YYYYMMDD_rating_exposure_comparison.png`
- DV01 by Holding (Top)  
  - `outputs/YYYYMMDD_dv01_by_holding_top.png`
- DV01 Share by Sector  
  - `outputs/YYYYMMDD_dv01_share_by_sector.png`

Tables exported as CSV:
- `outputs/YYYYMMDD_orders_benchmark.csv`
- `outputs/YYYYMMDD_orders_constrained.csv`
- `outputs/YYYYMMDD_breach_summary.csv`
- `outputs/YYYYMMDD_abs_active_table.csv`

---

## Assumptions & Limitations
- ETFs are used as **exposure proxies** (rates/credit/TIPS/cash)
- Duration is a **monthly-updated input** (demo table), forward-filled to daily
- DV01 is a simplified proxy (MV × Duration × 1bp), not a full curve-based risk model
- Transaction costs, bid-ask, and liquidity constraints are not modeled (can be added)

---

## Next Improvements (Production-like Extensions)
- Replace duration inputs with ETF factsheet ingestion (scheduled monthly update)
- Add transaction cost model and minimum lot / liquidity constraints
- Add tracking error proxy and risk-budget constraints
- Export a daily HTML/PDF pack for PM/Risk stakeholders
