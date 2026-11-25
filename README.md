
# 🌍 Life Expectancy Prediction

### Understanding the factors that influence life expectancy across countries

This project uses **Multiple Linear Regression** to study the relationship between life expectancy and various health, demographic, economic, and social indicators.

The objective is to develop a statistical model that can estimate life expectancy and identify important factors associated with differences in life expectancy across countries.

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

The dataset provides a broad view of the factors associated with population health and longevity.

---

## 🧠 Statistical Methodology

The analysis follows a systematic statistical approach:

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

\[
Y=\beta_0+\beta_1X_1+\beta_2X_2+\cdots+\beta_pX_p+\varepsilon
\]

Where:

- \(Y\) = Life expectancy
- \(X_1,X_2,\ldots,X_p\) = Predictor variables
- \(\beta_0\) = Intercept
- \(\beta_i\) = Regression coefficients
- \(\varepsilon\) = Random error

The regression coefficients describe the expected change in the response associated with a one-unit change in a predictor, keeping other predictors constant.

### Model Refinement

To improve the regression model:

- Highly correlated predictors were examined to reduce redundancy.
- Missing numerical values were treated using median imputation.
- Skewed predictors were transformed using logarithmic transformation.
- A Yeo–Johnson transformation was applied to the response variable to improve residual normality.
- Regression assumptions and influential observations were evaluated.

The refined OLS model was compared with **Ridge, Lasso, and Elastic Net Regression**.

---

## 📈 Model Performance

The models were evaluated on unseen test data using R², Mean Absolute Error (MAE), and Root Mean Squared Error (RMSE).

| Model | R² | MAE | RMSE |
|---|---:|---:|---:|
| Ridge Regression | **0.723** | **3.698** | **4.857** |
| Elastic Net | 0.722 | 3.710 | 4.861 |
| Lasso Regression | 0.716 | 3.771 | 4.915 |
| Refined OLS | 0.716 | 3.772 | 4.915 |

### Evaluation Metrics

**R² (Coefficient of Determination)**

\[
R^2=1-\frac{\sum(y_i-\hat y_i)^2}{\sum(y_i-\bar y)^2}
\]

Measures how much variation in life expectancy is explained by the model.

**MAE (Mean Absolute Error)**

\[
MAE=\frac{1}{n}\sum_{i=1}^{n}|y_i-\hat y_i|
\]

Measures the average absolute prediction error.

**RMSE (Root Mean Squared Error)**

\[
RMSE=\sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat y_i)^2}
\]

Penalizes larger prediction errors more heavily.

### Best Performing Model

Ridge Regression achieved the best overall test performance, with an R² of approximately **0.723** and an average absolute prediction error of approximately **3.70 years**.

---

## 🔍 Key Findings

- Life expectancy is associated with a combination of health, economic, and social indicators.
- Adult mortality and HIV/AIDS are important health-related variables considered in the analysis.
- Education and income-related indicators help capture differences in socioeconomic development.
- Regularization techniques provide performance comparable to the refined OLS model.
- Ridge Regression achieved a small improvement in predictive performance over the other models.

---

## 🏁 Conclusion

This project demonstrates how statistical regression techniques can be used to understand and predict life expectancy using country-level indicators.

Through data cleaning, exploratory analysis, feature selection, transformation, and regression diagnostics, the model was refined and evaluated using multiple regression approaches.

Ridge Regression provided the strongest test-set performance, explaining approximately **72.3% of the variation** in life expectancy in the evaluation data.

The analysis highlights the importance of combining health, education, and economic indicators when studying population longevity.

---

## 👩‍💻 Author

**Annanya Aggrawal**

