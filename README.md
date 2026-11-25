
# 🌍 Life Expectancy Prediction

### Understanding the factors that influence life expectancy across countries

This project uses **Multiple Linear Regression** to study the relationship between life expectancy and various health, demographic, economic, and social indicators.

The objective is to develop a statistical model that estimates life expectancy and identifies important factors associated with differences in longevity across countries.

---

## 📊 About the Dataset

The project uses the **Life Expectancy Data** dataset, containing country-level information on life expectancy and related indicators.

- **Observations:** 2,938
- **Countries:** 193
- **Time period:** 2000–2015
- **Total columns:** 22

### Key Features

| Category | Variables |
|---|---|
| Health | Adult Mortality, BMI, HIV/AIDS |
| Child Health | Infant Deaths, Under-five Deaths |
| Immunization | Hepatitis B, Polio, Diphtheria |
| Economy | GDP, Population, Health Expenditure |
| Education | Schooling, Income Composition |
| Target Variable | Life Expectancy |

---

## 🧠 Statistical Methodology

The analysis follows these steps:

1. Data cleaning and duplicate removal
2. Missing-value analysis and treatment
3. Exploratory Data Analysis (EDA)
4. Correlation analysis and predictor selection
5. Train-test split
6. Transformation of highly skewed variables
7. Multiple Linear Regression
8. Regression diagnostics
9. Comparison of regularized regression models

### Multiple Linear Regression

The relationship between life expectancy and the selected predictors is represented by:

$$
Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + \cdots + \beta_pX_p + \varepsilon
$$

Where:

- $Y$ = Life expectancy
- $X_1, X_2, \ldots, X_p$ = Predictor variables
- $\beta_0$ = Intercept
- $\beta_i$ = Regression coefficients
- $\varepsilon$ = Random error

The regression coefficients describe the expected change in life expectancy associated with a one-unit change in a predictor, keeping other predictors constant.

### Model Refinement

To improve the regression model:

- Highly correlated predictors were examined to reduce redundancy.
- Missing numerical values were treated using median imputation.
- Skewed predictors were transformed using logarithmic transformation.
- A Yeo–Johnson transformation was applied to the response variable to improve residual normality.
- Regression assumptions and influential observations were evaluated.

The refined OLS model was compared with **Ridge, Lasso, and Elastic Net Regression**.

---

## 🔬 Regression Diagnostics

Several diagnostic tests were used to check the assumptions and reliability of the regression model.

| Test | Purpose |
|---|---|
| VIF | Checks multicollinearity among predictors |
| Breusch–Pagan Test | Checks constant variance of residuals |
| Jarque–Bera Test | Checks normality of residuals |
| Durbin–Watson Statistic | Checks residual autocorrelation |
| Studentized Residuals, Leverage & Cook's Distance | Identifies outliers and influential observations |

### 1. Multicollinearity — VIF

$$
VIF_j = \frac{1}{1-R_j^2}
$$

A high VIF indicates possible multicollinearity. The maximum VIF in the final model was approximately **3.95**.

### 2. Heteroscedasticity — Breusch–Pagan Test

Checks whether the residual variance is constant.

$$
H_0: \sigma_i^2 = \sigma^2
$$

$$
H_1: \sigma_i^2 \neq \sigma^2
$$

The final model showed no significant evidence of heteroscedasticity.

### 3. Normality — Jarque–Bera Test

Checks whether residuals are approximately normally distributed.

$$
JB = \frac{n}{6}
\left[
S^2 + \frac{(K-3)^2}{4}
\right]
$$

The Yeo–Johnson transformation improved residual normality in the final model.

### 4. Autocorrelation — Durbin–Watson Statistic

Checks for first-order autocorrelation in residuals.

$$
DW =
\frac{\sum_{i=2}^{n}(e_i-e_{i-1})^2}
{\sum_{i=1}^{n}e_i^2}
$$

The final model's statistic was approximately **1.84**, close to 2.

### 5. Outliers and Influence

Studentized residuals, leverage, and Cook's distance were used to identify unusual or influential observations.

A potential outlier was examined and retained because it did not show strong influence.

Residuals vs. Fitted and Q–Q plots were also used for visual assessment of model assumptions.

---

## 📈 Model Performance

The models were evaluated on unseen test data using R², MAE, and RMSE.

| Model | R² | MAE | RMSE |
|---|---:|---:|---:|
| Ridge Regression | **0.723** | **3.698** | **4.857** |
| Elastic Net | 0.722 | 3.710 | 4.861 |
| Lasso Regression | 0.716 | 3.771 | 4.915 |
| Refined OLS | 0.716 | 3.772 | 4.915 |

### Evaluation Metrics

**1. R² (Coefficient of Determination)**

$$
R^2 = 1 - \frac{\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}
{\sum_{i=1}^{n}(y_i-\bar{y})^2}
$$

**2. Adjusted R²**

$$
R^2_{\text{adj}} =
1 - (1-R^2)\frac{n-1}{n-p-1}
$$

**3. MAE (Mean Absolute Error)**

$$
MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i-\hat{y}_i|
$$

**4. RMSE (Root Mean Squared Error)**

$$
RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}
$$

Where:

- $y_i$ = Actual value
- $\hat{y}_i$ = Predicted value
- $\bar{y}$ = Mean of actual values
- $n$ = Number of observations
- $p$ = Number of predictors

### Best Performing Model

Ridge Regression achieved the best overall test performance, with an R² of approximately **0.723** and an average absolute prediction error of approximately **3.70 years**.

---

## 🏁 Conclusion

This project demonstrates how statistical regression techniques can be used to understand and predict life expectancy using country-level indicators.

Through data cleaning, exploratory analysis, feature selection, transformation, and regression diagnostics, the model was refined and evaluated using multiple regression approaches.

Ridge Regression provided the strongest test-set performance, explaining approximately **72.3% of the variation** in life expectancy in the evaluation data.

The project highlights the importance of statistical modeling, assumption checking, and model evaluation in developing reliable predictive models.

---

## 👩‍💻 Author

**Annanya Aggrawal**
