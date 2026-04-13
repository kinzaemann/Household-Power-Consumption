# ⚡ Energy Consumption Time Series Forecasting

### Predicting Short-Term Household Energy Usage with ARIMA, Prophet & XGBoost

*A production-grade time series analysis pipeline comparing statistical, decomposition-based, and machine learning approaches to energy demand forecasting — featuring interactive visualizations and rigorous model evaluation.*
---

## 📋 Table of Contents

- [Overview](#-overview)
- [Dataset](#-dataset)
- [Project Pipeline](#-project-pipeline)
- [Key Visualizations](#-key-visualizations)
- [Models & Results](#-models--results)
- [Feature Engineering](#-feature-engineering)
- [Key Insights](#-key-insights)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [License](#-license)

---

## 🎯 Overview

Energy demand forecasting is critical for grid management, cost optimization, and sustainability planning. This project builds and compares **three distinct forecasting paradigms** on real-world household-level electricity consumption data:

| Approach | Model | Philosophy |
|----------|-------|-----------|
| **Statistical** | ARIMA (Auto-ARIMA) | Captures linear temporal dependencies via autoregression and moving averages |
| **Decomposition** | Facebook Prophet | Additive/multiplicative decomposition with automatic seasonality detection |
| **Machine Learning** | XGBoost | Gradient-boosted trees over engineered temporal features |

All models are evaluated on the **same 30-day holdout window** using MAE, RMSE, and MAPE for a fair, apples-to-apples comparison.

---

## 📊 Dataset

**Source:** [UCI Individual Household Electric Power Consumption](https://www.kaggle.com/datasets/imtkaggleteam/household-power-consumption)

| Property | Detail |
|----------|--------|
| **Origin** | Single household in Sceaux (Paris region), France |
| **Period** | December 2006 – November 2010 |
| **Granularity** | 1-minute sampling intervals |
| **Size** | ~2,075,259 measurements |
| **Features** | 7 electrical quantities + date/time |

### Feature Descriptions

| Column | Description | Unit |
|--------|-------------|------|
| `Global_active_power` | Household minute-averaged active power | kW |
| `Global_reactive_power` | Household minute-averaged reactive power | kW |
| `Voltage` | Minute-averaged voltage | V |
| `Global_intensity` | Minute-averaged current intensity | A |
| `Sub_metering_1` | Kitchen (dishwasher, oven, microwave) | Wh |
| `Sub_metering_2` | Laundry room (washing machine, dryer, light) | Wh |
| `Sub_metering_3` | Water heater & air conditioning | Wh |

---

## 🔄 Project Pipeline
┌──────────────────────────────────────────────────────────────────────┐
│ DATA INGESTION │
│ Raw CSV (2M+ rows, 1-min intervals) → Parse datetime → Clean NaN │
└──────────────────────┬───────────────────────────────────────────────┘
▼
┌──────────────────────────────────────────────────────────────────────┐
│ DATA PREPROCESSING │
│ Forward-fill imputation → Resample to hourly & daily averages │
└──────────────────────┬───────────────────────────────────────────────┘
▼
┌──────────────────────────────────────────────────────────────────────┐
│ EXPLORATORY DATA ANALYSIS │
│ Trend analysis · Seasonal patterns · Heatmaps · Correlations │
│ Year-over-year comparison · Distribution analysis │
└──────────────────────┬───────────────────────────────────────────────┘
▼
┌──────────────────────────────────────────────────────────────────────┐
│ STATISTICAL ANALYSIS │
│ Augmented Dickey-Fuller test · Seasonal decomposition │
└──────────────────────┬───────────────────────────────────────────────┘
▼┌──────────────────────────────────────────────────────────────────────┐
│ FEATURE ENGINEERING │
│ Calendar · Cyclical encoding · Lag features · Rolling statistics │
└──────────────────────┬───────────────────────────────────────────────┘
▼
┌──────────────────────────────────────────────────────────────────────┐
│ MODEL TRAINING & FORECASTING │
│ ARIMA (auto_arima) │ Prophet (multiplicative) │ XGBoost (boosted) │
└──────────────────────┬───────────────────────────────────────────────┘
▼
┌──────────────────────────────────────────────────────────────────────┐
│ EVALUATION & COMPARISON │
│ MAE · RMSE · MAPE · Residual analysis · Error-by-horizon plots │
└──────────────────────────────────────────────────────────────────────┘

---

## 📈 Key Visualizations

The notebook produces **12+ interactive Plotly charts**, including:

- **Full 4-year time series** with 30-day & 90-day rolling averages
- **Monthly consumption box plots** revealing seasonal patterns
- **Day × Hour heatmap** showing intraday usage rhythms
- **Year-over-year comparison** of consumption patterns
- **Correlation matrix** across all electrical features
- **Seasonal decomposition** (trend, seasonal, residual)
- **Train/test split visualization**
- **Actual vs. forecast overlay** for each model
- **Feature importance** chart (XGBoost top 15 features)
- **Model comparison bar charts** (MAE, RMSE, MAPE)
- **Residual diagnostics** (time plot, distribution, box plots, cumulative error)
- **Error-by-horizon** analysis (does accuracy degrade over the forecast window?)

> All visualizations are **interactive** — zoom, pan, hover for details, toggle traces on/off.

---

## 🏆 Models & Results

### Performance on 30-Day Test Window

| Model | MAE (kW) | RMSE (kW) | MAPE (%) | Rank |
|-------|----------|-----------|----------|------|
| **ARIMA** | _auto_ | _auto_ | _auto_ | — |
| **Prophet** | _auto_ | _auto_ | _auto_ | — |
| **XGBoost** | _auto_ | _auto_ | _auto_ | — |

> *Actual values are computed at runtime — run the notebook to see final numbers.*

### Model Strengths

| Model | Best For |
|-------|----------|
| **ARIMA** | Statistical rigor, formal confidence intervals, regulatory reporting |
| **Prophet** | Multi-seasonality capture, stakeholder communication, interpretable components |
| **XGBoost** | Highest accuracy potential, non-linear pattern capture, feature-driven insights |

---

## 🔧 Feature Engineering

**26 engineered features** across 5 categories:

| Category | Features | Count |
|----------|----------|-------|
| **Calendar** | day_of_week, month, quarter, day_of_month, week_of_year, is_weekend, day_of_year | 7 |
| **Cyclical** | sin/cos encoding of day_of_week, month, day_of_year | 6 |
| **Lag** | t-1, t-2, t-3, t-7, t-14, t-30 | 6 |
| **Rolling** | 7/14/30-day rolling mean & standard deviation | 6 |
| **Expanding** | Expanding mean | 1 |

All lag and rolling features are **shifted by 1 day** to prevent data leakage.

---

## 🔍 Key Insights

1. **Strong Seasonality** — Winter months (Nov–Feb) show **40–60% higher** consumption than summer, driven by heating demand
2. **Weekly Patterns** — Weekends exhibit later morning peaks and more sustained midday usage
3. **Intraday Peaks** — Evening peak at 18:00–21:00 (cooking, lighting); secondary peak at 07:00–09:00
4. **Year-over-Year Stability** — Consumption patterns are remarkably consistent across all observed years
5. **Feature Importance** — Lag-1 and rolling averages dominate XGBoost's predictions, confirming strong autocorrelation
6. **Non-stationarity** — ADF test confirms the series is non-stationary at the 5% level, validating the need for differencing (ARIMA) or detrending

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| **Language** | Python 3.10+ |
| **Data Processing** | Pandas, NumPy |
| **Visualization** | Plotly (interactive), Plotly Subplots |
| **Statistical Modeling** | Statsmodels, pmdarima (Auto-ARIMA) |
| **ML Forecasting** | Facebook Prophet, XGBoost |
| **Evaluation** | scikit-learn (MAE, RMSE, MAPE) |
| **Statistical Tests** | SciPy, Statsmodels (ADF test) |

---

## 🚀 Getting Started

### Option 1: Run on Kaggle (Recommended)

1. Open the notebook on [Kaggle]
2. Add the [Household Power Consumption dataset](https://www.kaggle.com/datasets/imtkaggleteam/household-power-consumption)
3. Enable **Internet** in Session Options (for `pmdarima` install)
4. Run all cells sequentially

### Option 2: Run Locally

```bash
# Clone the repository
git clone https://github.com/kinzaemann/energy-consumption-forecasting.git
cd energy-consumption-forecasting

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

