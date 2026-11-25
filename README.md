
# 🌊 Tsunami Prediction from Seismic Measurements

### Machine Learning-Based Classification of Tsunami-Associated Earthquakes

This project applies machine learning classification techniques to investigate whether an earthquake is associated with a tsunami using seismic, geographical, and impact-related measurements.

The objective is to compare different classification algorithms and identify a model that can detect tsunami-associated earthquakes effectively.

---

## 📊 About the Dataset

The project uses the `earthquake_data_tsunami.csv` dataset.

| Dataset Information | Details |
|---|---|
| Total observations | 782 |
| Total columns | 13 |
| Original time period | 2001–2022 |
| Modelling period | 2013–2022 |
| Modelling observations | 418 |
| Target variable | `tsunami` |

### Data Quality

All 364 observations before 2013 were labelled as non-tsunami events, including major historical tsunami events.

Therefore, these observations were excluded from modelling.

The final dataset contains:

- 418 observations
- 304 tsunami-associated events
- 114 non-tsunami events

### Important Features

| Category | Variables |
|---|---|
| Physical | Magnitude, Depth |
| Impact | CDI, MMI, Significance Score |
| Geographic | Latitude, Longitude |
| Seismic Network | Dmin, Gap |
| Target | Tsunami |

---

## 🔍 Data Preprocessing

The following preprocessing steps were performed:

- Excluded observations before 2013 due to unreliable tsunami labels.
- Removed `nst` because it could reveal the historical data collection period.
- Treated zero values in `dmin` as missing.
- Applied median imputation and a missing-value indicator.
- Applied feature scaling where required.
- Excluded calendar variables from the final predictors.

---

## 🧠 Exploratory Data Analysis

Exploratory analysis was performed to understand feature distributions and relationships between tsunami and non-tsunami earthquakes.

The **Mann–Whitney U Test** was used to compare feature distributions between the two groups without assuming normality.

Correlation analysis was also performed to examine relationships among predictors and their association with the target variable.

---

## 🤖 Classification Models

Four machine learning algorithms were implemented and compared.

| Model | Description |
|---|---|
| Logistic Regression | Linear classification |
| K-Nearest Neighbors | Distance-based classification |
| Random Forest | Tree-based ensemble |
| XGBoost | Gradient boosting |

### Model Development

1. Stratified train-test split (80:20)
2. Preprocessing using pipelines
3. Hyperparameter tuning using GridSearchCV
4. Repeated stratified 5-fold cross-validation
5. Evaluation using classification metrics
6. Decision-threshold optimisation

---

## 📈 Model Performance

### Cross-Validation Results

| Model | ROC-AUC | Recall | F1 Score |
|---|---:|---:|---:|
| Logistic Regression | 0.633 | 0.620 | 0.699 |
| KNN | 0.684 | 0.869 | 0.823 |
| Random Forest | 0.851 | **0.957** | **0.903** |
| XGBoost | **0.864** | 0.885 | 0.887 |

### Test Set Results

| Model | Accuracy | Precision | Recall | F1 Score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.690 | 0.872 | 0.672 | 0.759 | 0.750 |
| KNN | 0.798 | 0.833 | 0.902 | 0.866 | 0.792 |
| Random Forest | 0.881 | 0.905 | **0.934** | 0.919 | 0.867 |
| XGBoost | **0.893** | **0.948** | 0.902 | **0.924** | **0.889** |

XGBoost achieved the highest test ROC-AUC, accuracy, precision, and F1 score. Random Forest achieved higher recall at the default threshold.

---

## 📐 Evaluation Metrics

### Precision

Measures the proportion of predicted tsunami events that were actually tsunami-associated.

$$
Precision = \frac{TP}{TP+FP}
$$

### Recall

Measures the proportion of actual tsunami-associated events correctly identified.

$$
Recall = \frac{TP}{TP+FN}
$$

Recall was prioritised because missing a tsunami-associated event is particularly important.

### F1 Score

The harmonic mean of precision and recall.

$$
F1 = 2 \times \frac{Precision \times Recall}
{Precision+Recall}
$$

### ROC-AUC

Measures the model's ability to distinguish between the two classes across different classification thresholds.

---

## 🎯 Decision Threshold Optimisation

The default threshold of 0.50 was adjusted to improve recall.

For XGBoost, a threshold of **0.42** was selected using cross-validated training predictions.

| Metric | Threshold 0.50 | Threshold 0.42 |
|---|---:|---:|
| Accuracy | 0.893 | 0.893 |
| Precision | 0.948 | 0.933 |
| Recall | 0.902 | **0.918** |
| F1 Score | 0.924 | **0.926** |
| False Negatives | 6 | **5** |

The adjusted threshold detected one additional tsunami-associated event, with one additional false alarm.

---

## 🔬 Model Interpretation

### Feature Importance

Permutation importance was used to identify the features contributing most to model performance.

The important features included:

- Longitude
- Depth
- Latitude
- Magnitude

Longitude showed the strongest individual permutation importance in the test evaluation.

### SHAP Analysis

SHAP (SHapley Additive exPlanations) was used to examine how individual features influenced model predictions.

---

## 🏁 Conclusion

This project demonstrates the application of machine learning classification techniques to identify earthquakes associated with tsunamis using seismic and geographical measurements.

After addressing data-quality issues and removing potential information leakage, four classification algorithms were compared.

XGBoost achieved the strongest overall test performance, with an ROC-AUC of **0.889** and an F1 score of **0.924**.

Threshold optimisation improved recall from 0.902 to 0.918, reducing missed tsunami-associated events from six to five.

The project highlights the importance of data preprocessing, model comparison, and recall-focused evaluation in tsunami-related classification tasks.

---

## 👩‍💻 Author

**Ananya Aggrawal**

