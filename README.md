
# ⚖️ Undertrial Prison Population Forecasting

### ARIMA-Based Time Series Analysis of India's Undertrial Prison Population

This project analyses the historical trend of India's undertrial prison population and applies **ARIMA (AutoRegressive Integrated Moving Average)** to model its movement and generate future forecasts.

The objective is to understand the long-term pattern in the undertrial prison population and explore how time-series forecasting can support data-driven planning and policy discussions.

---

## 📊 About the Dataset

The dataset contains annual observations of India's undertrial prison population.

| Dataset Information | Details |
|---|---|
| Variable | Undertrial Prison Population |
| Time Period | 1995–2023 |
| Total Observations | 29 |
| Frequency | Annual |
| Target Variable | Number of Undertrials |

The dataset records the yearly number of individuals held in prisons while awaiting the completion of their trials.

---

## 📈 Exploratory Data Analysis

The historical series shows an overall increasing trend in the undertrial prison population, with fluctuations across years.

The population increased substantially over the observed period, particularly in recent years.

---

## 🧠 Time Series Methodology

The analysis follows these steps:

1. Data loading and time-index preparation
2. Visualization of the historical time series
3. Stationarity testing using the Augmented Dickey–Fuller (ADF) test
4. First-order differencing
5. ACF and PACF analysis
6. ARIMA model fitting
7. Model comparison using AIC
8. Forecast generation

### 1. Stationarity — Augmented Dickey–Fuller Test

The ADF test is used to determine whether the time series is stationary.

**Hypotheses:**

$$
H_0: \text{The series has a unit root}
$$

$$
H_1: \text{The series is stationary}
$$

The original series was non-stationary. First-order differencing was therefore applied.

The differencing operation is:

$$
\Delta Y_t = Y_t - Y_{t-1}
$$

After differencing, the ADF test indicated stationarity.

### 2. ACF and PACF

The Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) were examined to understand the dependence structure of the differenced series and guide ARIMA model selection.

### 3. ARIMA Model

The ARIMA model combines autoregressive, differencing, and moving-average components.

Its general mathematical representation is:

$$
\phi(B)(1-B)^dY_t = c + \theta(B)\varepsilon_t
$$

Where:

- $p$ = Order of the autoregressive component
- $d$ = Degree of differencing
- $q$ = Order of the moving-average component
- $B$ = Backshift operator
- $\varepsilon_t$ = Random error
- $\phi(B)$ = AR polynomial
- $\theta(B)$ = MA polynomial

The selected model was:

**ARIMA(1,1,1)**

It includes one autoregressive term, first-order differencing, and one moving-average term.

---

## 🔍 Model Selection

Three ARIMA specifications were compared using the Akaike Information Criterion (AIC).

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

ARIMA(1,1,1) achieved the lowest AIC among the three models and was selected for forecasting.

---

## 📉 Model Fit

The fitted values from the selected ARIMA model were compared with the observed undertrial prison population.

The model captures the broad historical movement of the series, including its major fluctuations and overall upward trend.

---

## 🔮 Forecasting

The selected ARIMA(1,1,1) model was used to generate forecasts for the next five annual periods.

The forecasting procedure also calculates confidence intervals to represent uncertainty around the predicted values.

The forecast extends the historical trend into future periods and provides an analytical perspective on the possible movement of the undertrial prison population.

---

## 🏁 Conclusion

This project demonstrates the application of time-series analysis and ARIMA modeling to study and forecast India's undertrial prison population.

The original series was found to be non-stationary and was made stationary through first-order differencing. After examining autocorrelation patterns and comparing candidate models using AIC, ARIMA(1,1,1) was selected as the preferred specification.

The fitted values capture the broad historical movement of the series, while the five-period forecast provides an outlook for future population levels.

Overall, the analysis demonstrates how statistical time-series techniques can support the study of important social and administrative trends.

---

## 👩‍💻 Author

**Ananya Aggrawal**
