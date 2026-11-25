
# 🌊 Tsunami Prediction from Seismic Measurements

### Machine Learning-Based Classification of Tsunami-Associated Earthquakes

This project uses machine learning classification techniques to investigate whether an earthquake is associated with a tsunami using seismic, geographical, and impact-related measurements.

The main objective is to compare different classification algorithms and identify a model that can detect as many tsunami-associated earthquakes as possible while controlling false alarms.

**Primary focus:** Recall is prioritised because missing a tsunami event can have much more serious consequences than generating a false alarm.

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

### Important Data Quality Consideration

A data-quality audit revealed that all 364 observations before 2013 were incorrectly labelled as non-tsunami events, including major historical disasters such as the 2004 Sumatra and 2011 Japan earthquakes.

Therefore, these observations were excluded from modelling.

The final modelling dataset contains:

- **418 earthquake observations**
- **304 tsunami-associated events (72.7%)**
- **114 non-tsunami events (27.3%)**

### Important Features

| Category | Variables |
|---|---|
| Physical | Magnitude, Depth |
| Impact | CDI, MMI, Significance Score |
| Geographic | Latitude, Longitude |
| Seismic Network | Dmin, Gap |
| Target | Tsunami |

The target variable is:

- `1` = Earthquake associated with a tsunami
- `0` = Earthquake not associated with a tsunami

---

## 🔍 Data Cleaning and Preprocessing

Several data-quality issues were investigated before model development.

- Observations before 2013 were excluded because their tsunami labels were unreliable.
- The `nst` variable was removed because its recording pattern revealed the historical data collection period, creating information leakage.
- Zero values in `dmin` were treated as missing observations.
- Missing values were handled using median imputation.
- A missing-value indicator was added for `dmin` within the preprocessing pipeline.
- Feature scaling was applied where required, particularly for Logistic Regression and KNN.

Calendar variables (`Year` and `Month`) were excluded from the final predictor set.

---

## 🧠 Exploratory Data Analysis

Exploratory analysis was performed to understand feature distributions, relationships, and differences between tsunami and non-tsunami events.

### Statistical Analysis

The **Mann–Whitney U Test** was used to compare feature distributions between the two outcome groups without assuming normality.

Magnitude and geographical coordinates showed statistically significant differences between the groups, although the differences in magnitude were relatively small.

### Correlation Analysis

A correlation matrix was examined to understand relationships among predictors and their associations with the target variable.

The analysis also highlighted the relationship between significance score, magnitude, and earthquake intensity measurements.

---

## 🤖 Classification Models

Four machine learning algorithms were implemented and compared.

| Model | Description |
|---|---|
| Logistic Regression | Linear classification baseline |
| K-Nearest Neighbors (KNN) | Distance-based classification |
| Random Forest | Ensemble of decision trees |
| XGBoost | Gradient boosting-based classification |

### Model Development

The modelling procedure included:

1. Stratified train-test splitting (80:20).
2. Preprocessing using pipelines to prevent data leakage.
3. Hyperparameter tuning using GridSearchCV.
4. Repeated stratified 5-fold cross-validation (3 repetitions).
5. Comparison using multiple classification metrics.
6. Decision-threshold optimisation with recall as a priority.

The final dataset was divided into:

- **Training:** 334 observations
- **Testing:** 84 observations

---

## 📈 Model Performance

Models were compared using repeated cross-validation on the training data.

| Model | ROC-AUC | Recall | F1 Score | Average Precision |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.633 | 0.620 | 0.699 | 0.800 |
| KNN | 0.684 | 0.869 | 0.823 | 0.830 |
| Random Forest | 0.851 | **0.957** | **0.903** | 0.921 |
| XGBoost | **0.864** | 0.885 | 0.887 | **0.922** |

### Test Set Performance

| Model | Accuracy | Precision | Recall | F1 Score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.690 | 0.872 | 0.672 | 0.759 | 0.750 |
| KNN | 0.798 | 0.833 | 0.902 | 0.866 | 0.792 |
| Random Forest | 0.881 | 0.905 | **0.934** | 0.919 | 0.867 |
| XGBoost | **0.893** | **0.948** | 0.902 | **0.924** | **0.889** |

**XGBoost** achieved the highest test ROC-AUC, accuracy, precision, and F1 score. Random Forest achieved higher recall at the default classification threshold.

---

## 📐 Evaluation Metrics

Since tsunami detection prioritises identifying positive events, several classification metrics were considered.

### 1. Precision

Measures how many predicted tsunami events were actually associated with a tsunami.

$$
Precision = \frac{TP}{TP+FP}
$$

### 2. Recall (Sensitivity)

Measures the proportion of actual tsunami-associated events correctly identified.

$$
Recall = \frac{TP}{TP+FN}
$$

Recall was given particular importance because false negatives represent missed tsunami-associated events.

### 3. F1 Score

The harmonic mean of precision and recall.

$$
F1 = 2 \times \frac{Precision \times Recall}
{Precision+Recall}
$$

### 4. ROC-AUC

Measures the model's ability to distinguish between the two classes across different classification thresholds.

A higher ROC-AUC indicates better ranking and discrimination performance.

---

## 🎯 Decision Threshold Optimisation

The default classification threshold of 0.50 was not treated as fixed.

For XGBoost, the decision threshold was adjusted using cross-validated training predictions to achieve a recall-first operating point.

| Metric | Threshold 0.50 | Threshold 0.42 |
|---|---:|---:|
| Accuracy | 0.893 | 0.893 |
| Precision | 0.948 | 0.933 |
| Recall | 0.902 | **0.918** |
| F1 Score | 0.924 | **0.926** |
| False Negatives | 6 | **5** |
| False Positives | 3 | 4 |

At a threshold of **0.42**, XGBoost detected one additional tsunami-associated event, at the cost of one additional false alarm.

The threshold was selected using training data rather than the held-out test data.

---

## 🔬 Model Interpretation

### Feature Importance

Permutation importance was used to examine the contribution of individual features to model performance.

The most important features included:

- Longitude
- Depth
- Latitude
- Magnitude

Longitude showed the strongest individual permutation importance in the test evaluation.

### SHAP Analysis

SHAP (SHapley Additive exPlanations) was used to examine how individual features influenced model predictions and to improve model interpretability.

---

## ⚠️ Generalisation and Limitations

An important part of this project was investigating whether the model could generalise beyond the geographical regions represented in its training data.

Random cross-validation produced an ROC-AUC of approximately **0.854** for the full feature set.

However, spatially blocked cross-validation reduced the ROC-AUC to approximately **0.684**.

This indicates that geographical information contributes substantially to model performance and that random validation may overestimate its ability to predict events in unfamiliar regions.

Consequently, the model should not be interpreted as a reliable real-time tsunami early-warning system. Its predictions require further validation across independent geographical regions and operational datasets.

---

## 🏁 Conclusion

This project demonstrates the application of machine learning classification techniques to identify earthquakes associated with tsunamis using seismic and geographical measurements.

A major part of the analysis involved identifying unreliable historical labels and removing features that introduced information leakage.

Among the evaluated algorithms, XGBoost achieved the strongest overall test-set performance, with an ROC-AUC of **0.889** and an F1 score of **0.924**.

Decision-threshold optimisation further improved recall from 0.902 to 0.918, reducing missed tsunami-associated events from six to five on the held-out test set.

However, spatially blocked validation revealed limitations in generalisation to unfamiliar geographical regions. This highlights the importance of data quality, appropriate validation strategies, and careful interpretation when developing models for high-impact applications.

---

## 👩‍💻 Author

**Ananya Aggrawal**
