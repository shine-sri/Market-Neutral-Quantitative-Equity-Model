# **Market-Neutral Quantitative Equity Model**

A **systematic, market-neutral long–short equity trading strategy** built on Indian large-cap stocks, combining **classical quantitative signals** with **machine learning**, and validated using **out-of-sample testing and Monte Carlo stress analysis**.

---

## **Project Overview**

This project implements an **end-to-end quantitative trading system** with a focus on:

- Multi-alpha signal generation  
- Market-neutral portfolio construction  
- Volatility-aware risk management  
- Robust backtesting without look-ahead bias  

The objective is **risk-adjusted consistency**, not directional market bets.

---

## **Strategy Summary**

The model blends **four independent alpha sources**, each capturing a different market inefficiency:

- **Mean Reversion** – exploiting short-term price overreaction  
- **Momentum** – capturing medium-term trend persistence  
- **Statistical Arbitrage** – trading relative mispricing between cointegrated stocks  
- **Machine Learning** – improving signal timing using nonlinear regression  

These signals are combined into a **single composite ranking score** and traded in a **long–short, market-neutral** framework.

---

## **Asset Universe**

- **Market:** NSE (India)  
- **Reference Index:** NIFTY 50  

### **Stocks Used (Fixed Universe)**

RELIANCE.NS
TCS.NS
INFY.NS
HDFCBANK.NS
ICICIBANK.NS
SBIN.NS
KOTAKBANK.NS
AXISBANK.NS
LT.NS
ITC.NS
HINDUNILVR.NS
BHARTIARTL.NS
ASIANPAINT.NS
HCLTECH.NS
MARUTI.NS
TITAN.NS
SUNPHARMA.NS


**Why these stocks?**
- High liquidity  
- Sector diversification  
- Continuous trading history since 2009  

The universe is **static** to avoid survivorship bias.

---

## **Data Details**

- **Source:** Yahoo Finance  
- **Access Method:** `yfinance`  
- **Frequency:** Daily  
- **Price Type:** Adjusted Close  

> **Note:** Although OHLCV data is available, **all signals are derived exclusively from adjusted close prices and price-based transformations**. No fundamentals, news, or alternative data are used.

---

## **Time Periods**

- **Full Data Range:** 2009 – 2025  
- **Training Period:** 2009 – 2020  
- **Out-of-Sample Testing:** 2021 – 2025  

Strict temporal separation is maintained to prevent data leakage.

---

## **Alpha Signal Construction**

### **1. Mean Reversion**

- 20-day rolling mean and standard deviation  
- Z-score based deviation from recent equilibrium  
- Contrarian positioning against extreme moves  

---

### **2. Momentum**

- 6-month (126 trading days) price momentum  
- Only positive momentum retained  
- Avoids short-side trend exposure  

---

### **3. Statistical Arbitrage**

- Engle–Granger cointegration test  
- P-value threshold: **0.05**  
- Spread z-score used for relative value trading  
- Long undervalued stock, short overvalued stock  

---

### **4. Machine Learning Alpha**

- **Model:** XGBoost Regressor  
- **Features:**
  - Mean reversion signal  
  - Momentum signal  
  - Volatility (ATR)  
- **Target:** 21-day forward return  
- **Scaling:** StandardScaler  

The ML model enhances signal quality by learning nonlinear relationships between price-based features and future returns.

---

## **Alpha Blending**

Signals are combined using **fixed, interpretable weights**:

Composite Alpha =
0.30 × Mean Reversion
0.25 × Momentum
0.25 × Statistical Arbitrage
0.20 × Machine Learning


Stocks are ranked cross-sectionally on each rebalance date.

---

## **Portfolio Construction**

- **Structure:** Long–Short Equity  
- **Positions:**
  - Top 8 ranked stocks → Long  
  - Bottom 8 ranked stocks → Short  
- **Exposure:** Approximately market-neutral  

Returns are primarily driven by **relative stock selection**, not market direction.

---

## **Risk Management**

### **Volatility-Parity Position Sizing**

- Volatility measured using **14-day ATR**  
- Position size inversely proportional to volatility  
- Ensures equal risk contribution per trade  

---

### **Stops**

- **Initial Stop:** ± 1.3 × ATR  
- **Trailing Stop:** Adjusted dynamically as price moves favorably  

This limits downside risk while allowing profitable trades to run.

---

## **Rebalancing**

- **Frequency:** Every 21 trading days  
- All positions are closed and rebuilt at each rebalance  

This prevents signal decay and excessive turnover.

---

## **Backtesting Methodology**

The backtest is designed to avoid common pitfalls:

- No look-ahead bias  
- Strict out-of-sample evaluation  
- Daily mark-to-market accounting  
- Realistic portfolio constraints  

---

## **Performance Metrics**

The following metrics are evaluated:

- **CAGR (Annualized Return)**  
- **Sharpe Ratio (Risk-Adjusted Return)**  
- **Maximum Drawdown**  
- **Equity Curve Behavior**  

---

## **Monte Carlo Stress Testing**

To assess robustness against return sequencing risk:

- Bootstrap resampling of daily returns  
- **1000 simulated equity paths**  
- Reporting worst-case and median CAGR outcomes  

---

## **Final Out-of-Sample Results (2021–2025)**

Final Capital: ₹1,755,306
CAGR: 12.63%
Sharpe Ratio: 0.70
Max Drawdown: -23%

Monte Carlo:
Worst 5% CAGR: -3.0%
Median CAGR: 12.1%


---

## **Assumptions & Limitations**

### **Assumptions**
- No transaction costs  
- No slippage  
- Immediate execution at close price  

### **Limitations**
- Fixed alpha weights  
- No regime classifier  
- Daily data only  
- Transaction costs not modeled  

---

## **Future Enhancements**

- Dynamic alpha weighting  
- Regime classification (trend / range / stress)  
- Transaction cost and slippage modeling  
- Intraday extensions  
- Options-implied volatility features  

---

## **Author**

**Shine Srivastava**

---

## **Disclaimer**

This project is for **educational and research purposes only**.  
It does not constitute financial advice or a deployable trading system.

