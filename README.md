# Time-Series Analysis of Wheat Futures

A quantitative time-series modeling project analyzing 25 years of daily wheat futures prices, focusing on variance stabilization, seasonality decomposition, and ARIMA-family forecasting.

---

## Immediate Next Steps
- [ ] Email professor with finalized group member details.
- [ ] Pull historical wheat futures price series via Bloomberg Terminal (check for contract roll rules and seasonality).
- [ ] Run Box-Cox transformation to evaluate and stabilize variance across cycles.

---

## 📋 Project Roadmap

### 1. Data Pipeline & Exploratory Analysis
- **Data Collection:** Pull 25 years of continuous daily wheat futures price history (Bloomberg).
- **EDA:** Plot multi-decade price trajectories against key seasonal events, harvest windows, and historical supply shocks.
- **Decomposition:** Separate the series into trend, seasonal, and cyclical components using classical/STL decomposition methods.

### 2. Statistical Testing & Diagnostics
- **Stationarity:** Augmented Dickey-Fuller (ADF) and KPSS tests to determine required orders of integration ($d$).
- **Autocorrelation & White Noise:** Ljung-Box test to check residual dependence and serial correlation.
- **Parameter Identification:** Plot and interpret Autocorrelation Functions (ACF) and Partial Autocorrelation Functions (PACF) to identify candidate $p$ and $q$ lags.

### 3. Time-Series Modeling & Validation
- **Model Estimation:** Fit ARMA / ARIMA / SARIMA configurations across parameter grids.
- **Model Selection:** Evaluate candidates against AIC, BIC, and out-of-sample log-likelihood.
- **Residual Diagnostics:** Verify normality, homoscedasticity, and lack of serial correlation in residuals.

### 4. Forecasting & Evaluation
- Generate rolling out-of-sample point and interval forecasts.
- Benchmark predictions against naive / random walk baselines using RMSE, MAE, and MAPE.

---

## Deliverables
- **Data File:** Cleaned, adjusted continuous daily wheat futures time series (`.csv` / `.parquet`).
- **Final Report:** Complete analysis write-up covering:
  - Methodology and diagnostic justifications.
  - Model comparison and forecast performance.
  - Practical limitations (e.g., rollover artifacts, structural breaks, weather shocks, macro regime shifts).

---

## Tech Stack & Methods
- **Language:** Python
- **Core Techniques:** Box-Cox Transformation, Decomposition, ADF/KPSS Testing, Ljung-Box Test, ARIMA, GARCH
- **Data Source:** Bloomberg Terminal / CME / YFinance
