# Rolling-strategy-with-TX-2020-2024
# TX Futures Rolling Backtest  
Taiwan Index Futures Rolling Strategy Backtesting

##  Project Overview
This project implements a **rolling backtest framework for Taiwan Index Futures (TX)**, developed as **Assignment 1 for the EDGE Internship**.

The objective is to simulate a futures trading strategy that **rolls positions N days before contract expiration**, compute daily PnL based on closing prices, and compare the resulting performance against a benchmark strategy that uses **continuous futures prices without explicit rolling**.

The analysis covers the period **2014–2024** and focuses on accurately modeling:
- Front-month and next-month contract selection  
- Rolling logic based on time-to-maturity  
- Rolling costs with market direction considerations  

---

## Data Description
The following datasets are used:
- **TX Futures Monthly Trading Data** (from TEJ Pro)  
- **TX Futures Daily Transaction Data** (from Taiwan Futures Exchange)

Each trading day contains multiple contracts with different expiration months. Contracts are ranked by trading volume to identify:
- **Front contract (Cn)**: highest trading volume  
- **Next contract (Cf)**: second-highest trading volume  

---

## Rolling Strategy Logic
For a given rolling parameter **N (days before expiration)**:

1. Identify the front and next contracts based on trading volume
2. Sort contracts by expiration and remaining days
3. If remaining days > N, use the front contract’s daily return
4. If remaining days = N (or the first day below N), execute a roll:
   - Close the front contract
   - Open the next contract on the same trading day
5. Repeat this process iteratively over the full sample period
6. Accumulate daily returns to construct the PnL curve

This approach avoids static contract classification and allows contracts to **dynamically switch roles over time**, which better reflects real futures market behavior.

---

## Rolling Cost Modeling
Rolling costs are defined as:

\[
C_{roll} = |P_{close}^{near} - P_{open}^{far}|
\]

To determine the **sign of rolling cost**, the strategy evaluates both long and short PnL outcomes and assigns a market direction variable (`best_dir`):

- If `best_dir = long`, rolling cost is added
- If `best_dir = short`, rolling cost is subtracted

This design allows rolling costs to be **positive or negative**, depending on market conditions, rather than assuming a constant penalty.

---

## Performance Evaluation
The following performance metrics are computed:
- Annualized Return
- Annualized Volatility
- Sharpe Ratio
- Maximum Drawdown
- Maximum Recovery Days

PnL curves are plotted to compare:
- Rolling strategies with different N values
- Benchmark strategy using continuous prices

Empirical results show that **explicit rolling strategies outperform simple continuous-price-based PnL**, indicating that rolling behavior and cost treatment significantly affect futures backtest outcomes.

---

## Implementation
- Language: **Python**
- Environment: **Google Colab**
- Core libraries: `pandas`, `numpy`, `matplotlib`

The full implementation notebook is available here:  
[https://colab.research.google.com/drive/1AezhMI4NQkLAIVd47-7h08xZO7Po0oga](https://colab.research.google.com/drive/1yNmDBYGc0q9rfr5ZkLzKphvBT8UWBkKD)

---

## Repository Structure
