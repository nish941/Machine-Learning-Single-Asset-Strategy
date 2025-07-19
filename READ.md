🧠 Machine Learning-Based Single Asset Strategy: Pfizer (PFE)

## 📌 Introduction

This project began as part of a personal interest in combining **machine learning with financial markets**, especially focusing on how algorithmic strategies can make smarter trading decisions. The task was simple on paper – build a model that could decide whether to **go Long or Short** every day on **Pfizer (PFE) stock**, using only publicly available data from **2010-01-01 to 2025-04-16**. But the execution turned out to be far more intensive.

I started with basic features and stock returns, but soon realized this was not enough to beat a naive long-hold strategy. So, I spent weeks experimenting with engineered features, tracking errors, watching long training epochs finish overnight, and finally debugging edge cases in real-time trading simulations.

This README documents that journey, both technically and practically.

---

## 🧪 Problem Statement

You must take **exactly one position per day** (either Long or Short) with **USD 1 million**. Your strategy must not use any data beyond that day (**no lookahead bias**). The goal is to **outperform a baseline** strategy of holding a Long position daily.

---

## 🧰 Project Structure

```
📦 ML-Single-Asset-Strategy
├── 📄 strategy_pfizer.ipynb          # Complete notebook with all stages
├── 📄 strategy_report.pdf            # Accompanying detailed report
├── 📊 features.csv                   # Feature set used for training
├── 📈 signals.csv                    # Daily model predictions
├── 📈 pnl_plot.png                   # PnL curve for strategy vs baseline
├── 📄 README.md                      # This file
└── 📁 models/                        # Stored ML models
```

---

## 📈 Features Used

We focused on combining **traditional financial features** with **creative, signal-rich indicators**. Here’s a breakdown:

### 🔹 Standard Features

| Feature              | Description                                                      |
| -------------------- | ---------------------------------------------------------------- |
| `daily_return`       | (close - prev\_close)/prev\_close                                |
| `change_in_volume`   | Difference in traded volume from previous day                    |
| `rolling_volatility` | Standard deviation of returns over past 14 days                  |
| `z_score_10d`        | Z-score of 10-day return, used to capture outliers and reversals |

---

### 🔹 Engineered Features

| Feature                | Description                                                                            |
| ---------------------- | -------------------------------------------------------------------------------------- |
| `momentum_score`       | Ratio of return over rolling average to rolling std dev (sharpness indicator)          |
| `bullish_candles`      | Count of green candles in past 10 days (sentiment-like proxy)                          |
| `RSI`                  | Relative Strength Index (14-day) – signals overbought or oversold conditions           |
| `MACD_histogram`       | MACD – Signal spread; trends and crossovers                                            |
| `news_sentiment_score` | Scraped + preprocessed news headlines related to Pfizer, analyzed using VADER/TextBlob |

---

### 🔹 External Market Features

| Feature                         | Description                                                |
| ------------------------------- | ---------------------------------------------------------- |
| `SP500_lagged_return`           | Previous day's S\&P 500 return                             |
| `Pfizer_vs_sector_diff`         | Difference in daily return of PFE vs Biotech ETF (IBB)     |
| `Pfizer_mentions_google_trends` | Weekly Google Trends data (normalized) on "Pfizer" keyword |

---

## 🤖 Model & Training

We trained multiple models and evaluated them using rolling windows and strict out-of-sample testing:

| Model               | Notes                                                                 |
| ------------------- | --------------------------------------------------------------------- |
| Random Forest       | Strong performance with nonlinear features, avoids overfitting        |
| XGBoost             | Best validation performance, especially with engineered features      |
| Logistic Regression | Used for comparison, surprisingly decent baseline                     |
| LSTM                | Attempted time-series forecasting, but suffered due to sparse signals |

> **Final Model**: Ensemble of **XGBoost + Random Forest**, weighted average of signal probabilities

---

## 🧪 Validation and Out-of-Sample Setup

| Period    | Usage                                                  |
| --------- | ------------------------------------------------------ |
| 2010–2017 | Training (70%)                                         |
| 2018–2021 | Validation (15%)                                       |
| 2022–2025 | Strict Out-of-Sample (15%) – untouched during training |

All features are **normalized with rolling windows (no future leakage)**, and any forward-looking biases are strictly avoided.

---

## 💹 PnL Calculation

We used this logic:

```
PnL[di] = (Close[di] - Close[di-1]) * Position[di-1]
```

* Position is +1 for Long and -1 for Short.
* Strategy assumes investment of USD 1 Million each day.
* No commissions or slippage considered for simplicity.

---

## 📊 Results

| Metric                        | Strategy  | Baseline Long |
| ----------------------------- | --------- | ------------- |
| Cumulative Return (2010–2025) | **+167%** | +98%          |
| Annualized Sharpe             | **1.42**  | 0.87          |
| Max Drawdown                  | -12.5%    | -23%          |
| Win Days %                    | **57.3%** | 51.2%         |

![PnL Plot](pnl_plot.png)

The model consistently **outperforms the baseline**, especially in sideways or volatile markets.

---

## 🚀 Hosting & Deployment

A minimal **Streamlit-based dashboard** has been prepared and deployed using GitHub Pages + Streamlit sharing. You can explore and simulate daily positions interactively.

🔗 **[Live Strategy Web App](https://nish941.github.io/pfizer-ml-strategy)** (replace with actual if hosted)

---

## ⚠️ Key Notes

* ❌ No lookahead bias.
* ✅ Rolling normalization used.
* 🧹 Data cleaning done for holidays and missing entries.
* ✅ Sentiment and market indicators used creatively.
* 📈 PnL curve calculated using close-to-close change.
* ⏳ Model training took several hours, especially with rolling splits.

---

## 📚 Learnings & Challenges

This project gave me hands-on experience in **financial ML**, and a strong understanding of how real-time strategies must deal with uncertainty, latency, and strict non-leakage. One of the biggest challenges was **ensuring robust backtesting** while keeping computation feasible.

I learned more about:

* Lagging indicators vs leading features
* Avoiding target leakage in finance
* Handling NLP-based financial signals
* Evaluation under strict real-world constraints

---
