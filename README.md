
# ⚖️ Undertrial Prison Population Forecasting

### ARIMA-Based Time Series Analysis of India's Undertrial Prison Population

This project analyses the historical trend of India's undertrial prison population and applies **ARIMA (AutoRegressive Integrated Moving Average)** to model the series and forecast future population levels.

The objective is to understand the historical movement of the undertrial population and explore how time-series forecasting can support data-driven planning.

---

## 📊 About the Dataset

The project uses the `undertrials.csv` dataset.

| Dataset Information | Details |
|---|---|
| Variable | Undertrial Prison Population |
| Time Period | 1995–2023 |
| Observations | 29 |
| Frequency | Annual |
| Target Variable | Number of Undertrials |

Each observation represents the reported undertrial prison population for a particular year.

---

## 📈 Historical Time Series

The historical series shows an overall increasing trend, with fluctuations across years.

![Undertrial Prison Population from 1995 to 2023](images/historical_timeseries.png)

The plot includes every year from 1995 to 2023. Year labels are displayed vertically to prevent overlap.

---

## 🧠 Time Series Methodology

The analysis follows these steps:

1. Load and prepare the annual time series
2. Visualize the historical series
3. Test stationarity using the Augmented Dickey–Fuller (ADF) test
4. Apply first-order differencing
5. Examine ACF and PACF
6. Fit ARIMA models
7. Compare candidate models using AIC
8. Perform residual diagnostics
9. Forecast the next five years

### Forecasting Workflow

```mermaid
flowchart TD
    A["Annual Data<br/>1995–2023"]
    B["Time Series Plot"]
    C["ADF Stationarity Test"]
    D["First Differencing"]
    E["ACF and PACF"]
    F["Fit ARIMA Models"]
    G["Compare Models using AIC"]
    H["Select ARIMA(1,1,1)"]
    I["Five-Year Forecast"]

    A --> B --> C --> D --> E --> F --> G --> H --> I

    style H fill:#14532d,color:#ffffff,stroke:#22c55e
    style I fill:#14532d,color:#ffffff,stroke:#22c55e
```

---

## 🔬 Stationarity and Differencing

The **Augmented Dickey–Fuller (ADF) test** was used to check whether the series was stationary.

- **H₀:** The series contains a unit root.
- **H₁:** The series is stationary.

The original series was non-stationary.

First-order differencing was applied:

$$
\Delta Y_t = Y_t - Y_{t-1}
$$

After differencing, the series became stationary.

![First Difference of Undertrial Prison Population](images/first_difference.png)

---

## 📉 ACF and PACF

The Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) were examined to understand the dependence structure of the differenced series and guide ARIMA model selection.

![ACF of First-Differenced Series](images/acf.png)

![PACF of First-Differenced Series](images/pacf.png)

---

## 📐 ARIMA Model

ARIMA combines autoregressive, differencing, and moving-average components.

The general mathematical representation is:

$$
\phi(B)(1-B)^dY_t = c + \theta(B)\varepsilon_t
$$

Where:

- $p$ = Autoregressive order
- $d$ = Differencing order
- $q$ = Moving-average order
- $B$ = Backshift operator
- $\varepsilon_t$ = Random error

The selected model was:

**ARIMA(1,1,1)**

The model includes one autoregressive term, first-order differencing, and one moving-average term, with a linear trend.

---

## 🔍 Model Selection

Candidate models were compared using the Akaike Information Criterion (AIC).

$$
AIC = -2\ln(L) + 2k
$$

Where:

- $L$ = Maximum likelihood
- $k$ = Number of estimated parameters

A lower AIC indicates a better balance between model fit and complexity.

| Model | AIC |
|---|---:|
| ARIMA(1,1,0) | 635.789 |
| ARIMA(0,1,1) | 636.168 |
| **ARIMA(1,1,1)** | **634.410** |

ARIMA(1,1,1) had the lowest AIC among the compared models and was selected for forecasting.

---

## 🧪 Residual Diagnostics

Residual diagnostics were used to check whether the fitted model adequately captured the time-series structure.

| Test | Purpose |
|---|---|
| Ljung–Box Test | Checks residual autocorrelation |
| Jarque–Bera Test | Checks residual normality |
| Breusch–Pagan Test | Checks constant error variance |

The Ljung–Box test did not indicate significant residual autocorrelation at the tested lag.

The Jarque–Bera test indicated a departure from normality, while the Breusch–Pagan test did not provide significant evidence of heteroscedasticity at the 5% level.

---

## 📊 Actual vs Estimated Population

The fitted values from ARIMA(1,1,1) were compared with the observed population.

![Actual vs Estimated Undertrial Prison Population](images/actual_vs_estimated.png)

The fitted series captures the broad historical movement of the observed data, although some year-to-year differences remain.

All available years are included, with year labels displayed vertically.

---

## 🔮 Five-Year Forecast

The selected ARIMA(1,1,1) model was used to forecast the next five annual periods.

| Year | Forecast Population |
|---|---:|
| 2024 | 390,494 |
| 2025 | 400,361 |
| 2026 | 405,660 |
| 2027 | 413,207 |
| 2028 | 419,647 |

![ARIMA Five-Year Forecast](images/forecast.png)

The forecast indicates a gradual increase in the undertrial prison population over the forecast horizon. The shaded region represents the 95% forecast confidence interval.

---

## 🏁 Conclusion

This project demonstrates the application of time-series analysis and ARIMA modeling to India's undertrial prison population.

The original series was non-stationary and became stationary after first-order differencing. After examining autocorrelation patterns and comparing candidate models using AIC, ARIMA(1,1,1) was selected as the preferred specification.

The model captures the broad historical movement of the series and provides a five-year forecast extending to 2028.

Overall, the analysis illustrates how statistical time-series techniques can be applied to study and forecast important social and administrative trends.

---

## 👩‍💻 Author

**Ananya Aggrawal**
