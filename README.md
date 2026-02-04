# Fixed Income Portfolio Monitoring Dashboard (MVP)

## Project Overview
This project is an automated portfolio monitoring tool designed for Fixed Income ETF portfolios. It calculates key risk metrics such as **Duration**, **DV01**, and **Active Weights** against a benchmark, simulating the daily workflow of a portfolio manager.

## Key Features
- **Automated Data Pipeline:** Fetches historical data using Yahoo Finance API (`yfinance`).
- **Risk Analysis:** Calculates Portfolio Duration and DV01 (Dollar Value of a 01) to monitor interest rate sensitivity.
- **Compliance Engine:** Automated alerts for constraints (e.g., Cash < 3%, Single Issuer > 35%).
- **Reporting:** Generates visualization for Active Exposure and Risk breakdown.

## Tech Stack
- **Python 3.x**
- **Pandas / NumPy:** Data manipulation and financial calculations.
- **Yfinance:** Market data extraction.
- **Matplotlib:** Data visualization.

## How to Run
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
