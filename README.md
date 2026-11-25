
# 📉 Customer Churn Prediction

### Artificial Neural Networks, Early Stopping and Machine Learning Model Comparison

This project develops machine learning models to predict customer churn using customer demographics, financial information, and account activity.

The primary focus is on **Artificial Neural Network (ANN) architecture selection, hyperparameter tuning, and early stopping**, followed by a comparison with Random Forest, XGBoost, and Logistic Regression.

The objective is to identify customers who are likely to leave the bank and evaluate how different classification models perform on this task.

---

## 📊 About the Dataset

The project uses the **Churn Modelling** dataset.

| Dataset Information | Details |
|---|---|
| Total observations | 10,000 |
| Total columns | 14 |
| Target variable | Exited |
| Non-churn customers | 7,963 (79.63%) |
| Churn customers | 2,037 (20.37%) |
| Missing values | 0 |
| Duplicate rows | 0 |

### Important Features

| Category | Variables |
|---|---|
| Demographic | Age, Gender, Geography |
| Financial | Credit Score, Balance, Estimated Salary |
| Account Information | Tenure, Number of Products |
| Customer Activity | Has Credit Card, Is Active Member |
| Target | Exited |

The target variable is:

- `1` = Customer exited (Churn)
- `0` = Customer retained (Non-churn)

Identifier and name columns such as `RowNumber`, `CustomerId`, and `Surname` were excluded from model training.

---

## 🔍 Data Preprocessing

The following preprocessing steps were performed:

- Inspected missing values, duplicate records, and target distribution.
- Separated numerical and categorical variables.
- Applied One-Hot Encoding to `Geography` and `Gender`.
- Standardised numerical variables using StandardScaler for ANN and Logistic Regression.
- Used class weights to address the imbalance between churn and non-churn customers.
- Ensured that preprocessing was fitted using training data only to prevent data leakage.

### Data Splitting

The dataset was divided into three parts:

| Dataset | Observations | Percentage |
|---|---:|---:|
| Training | 6,400 | 64% |
| Validation | 1,600 | 16% |
| Testing | 2,000 | 20% |

Stratified sampling was used to preserve the churn proportion across all three sets.

The test set remained untouched during hyperparameter tuning, early stopping, and threshold selection.

---

## 🧠 Artificial Neural Network (ANN)

The main focus of this project is the development and evaluation of an Artificial Neural Network for binary classification.

### Network Architecture

The ANN consists of:

- Input layer containing the processed customer features.
- One or more hidden layers with ReLU activation.
- Output layer with a single neuron and Sigmoid activation.

The network uses:

- **Activation Function:** ReLU
- **Output Activation:** Sigmoid
- **Optimizer:** Adam
- **Loss Function:** Binary Cross-Entropy
- **Weight Initialization:** He Initialization
- **Batch Size:** 32

### Mathematical Formulation

**1. ReLU Activation**

$$
f(x)=\max(0,x)
$$

**2. Sigmoid Activation**

$$
\sigma(z)=\frac{1}{1+e^{-z}}
$$

The sigmoid function produces a probability between 0 and 1.

**3. Binary Cross-Entropy Loss**

$$
L=-\frac{1}{N}\sum_{i=1}^{N}
\left[
y_i\log(p_i)+(1-y_i)\log(1-p_i)
\right]
$$

Where:

- $y_i$ = Actual class
- $p_i$ = Predicted churn probability
- $N$ = Number of observations

---

## ⚙️ ANN Architecture Tuning

Different ANN configurations were evaluated using **5-fold Stratified Cross-Validation**.

The search considered:

- Number of hidden layers: 1, 2, and 3
- Hidden units: 16, 32, and 64
- Activation functions: ReLU and Leaky ReLU
- Learning rates: 0.0001, 0.001, and 0.01

ROC-AUC was used as the primary architecture-selection metric.

Several configurations achieved comparable performance. Therefore, the simplest competitive architecture was selected rather than automatically choosing the most complex model.

### Selected Architecture

| Parameter | Selected Value |
|---|---|
| Hidden Layers | 1 |
| Hidden Units | 16 |
| Activation | ReLU |
| Learning Rate | 0.001 |
| Optimizer | Adam |
| Mean CV ROC-AUC | 0.847 |

The selected architecture was then frozen for the early-stopping experiment.

---

## ⏱️ Early Stopping and Overfitting Analysis

Two ANN models were trained to investigate the effect of early stopping.

| Model | Training Configuration |
|---|---|
| ANN without Early Stopping | 150 epochs |
| ANN with Early Stopping | Maximum 150 epochs, patience = 10 |

Early stopping monitors validation loss and stops training when validation performance does not improve for 10 consecutive epochs.

The weights from the best validation-loss epoch are restored.

### Results

| Parameter | ANN without Early Stopping | ANN with Early Stopping |
|---|---:|---:|
| Maximum Epochs | 150 | 150 |
| Epochs Completed | 150 | 103 |
| Best Epoch | 79 | 93 |
| Minimum Validation Loss | 0.4536 | 0.4642 |

The early-stopping model stopped after 103 epochs and restored the weights from epoch 93.

Training and validation learning curves were examined to understand the effect of training duration on model performance.

---

## 🤖 Comparison with Other Models

To evaluate the ANN against alternative classification approaches, the following models were implemented:

| Model | Approach |
|---|---|
| Logistic Regression | Linear classification baseline |
| Random Forest | Ensemble of decision trees |
| XGBoost | Gradient boosting |
| ANN | Neural network classification |

### Hyperparameter Tuning

- Logistic Regression was used as the baseline model with balanced class weights.
- Random Forest was tuned using RandomizedSearchCV.
- XGBoost was tuned using RandomizedSearchCV with `scale_pos_weight` to address class imbalance.
- The ANN architecture was selected using cross-validation.

The final ANN with early stopping was compared with the tuned tree-based models.

---

## 📈 Final Model Performance

All models were evaluated on the untouched test dataset of 2,000 customers.

The classification threshold for each model was selected using the validation set by maximising the Class 1 F1-score.

| Model | Accuracy | Precision | Recall | F1 Score |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.7140 | 0.3878 | 0.7002 | 0.4991 |
| ANN + Early Stopping | **0.8540** | **0.6593** | 0.5848 | 0.6198 |
| Random Forest | 0.8255 | 0.5549 | 0.7199 | 0.6267 |
| XGBoost | 0.8015 | 0.5081 | **0.7715** | **0.6127** |

### Interpretation

- ANN achieved the highest test accuracy and precision.
- XGBoost achieved the highest recall, detecting a larger proportion of actual churners.
- Random Forest achieved the highest F1-score.
- Logistic Regression provided a baseline for comparison.

The results demonstrate that the best model depends on whether the priority is overall classification accuracy, identifying churners, or balancing precision and recall.

---

## 📐 Evaluation Metrics

**1. Accuracy**

Measures the proportion of correctly classified customers.

$$
Accuracy=\frac{TP+TN}{TP+TN+FP+FN}
$$

**2. Precision**

Measures the proportion of predicted churners who actually churned.

$$
Precision=\frac{TP}{TP+FP}
$$

**3. Recall**

Measures the proportion of actual churners correctly identified.

$$
Recall=\frac{TP}{TP+FN}
$$

**4. F1 Score**

The harmonic mean of precision and recall.

$$
F1=2\times\frac{Precision\times Recall}
{Precision+Recall}
$$

**5. ROC-AUC**

Measures the ability of a model to distinguish churners from non-churners across different classification thresholds.

**6. PR-AUC**

Summarises the precision-recall trade-off and is particularly useful when the positive class is less frequent.

---

## 🎯 Decision Threshold Selection

A default threshold of 0.50 is not always optimal, particularly when the target variable is imbalanced.

In this project, the threshold was selected using the validation set by maximising the F1-score for Class 1 (churn).

| Model | Selected Threshold |
|---|---:|
| Logistic Regression | 0.05 |
| ANN + Early Stopping | 0.70 |
| Random Forest | 0.05 |
| XGBoost | 0.05 |

The selected thresholds were fixed before evaluating the final models on the test set.

---

## 🔬 Model Interpretability — SHAP

SHAP (SHapley Additive exPlanations) was used to understand how customer characteristics influence model predictions.

SHAP analysis was performed for:

- XGBoost
- ANN with Early Stopping

The analysis included:

- Global feature importance
- SHAP summary plots
- Direction and magnitude of individual feature contributions

This provides additional insight into the factors influencing the predicted probability of customer churn.

---

## 🏁 Conclusion

This project demonstrates the application of artificial neural networks and traditional machine learning algorithms to customer churn prediction.

The ANN architecture was selected through cross-validation, followed by an early-stopping experiment to investigate training performance and overfitting.

Four classification approaches were compared using an untouched test dataset.

ANN achieved the highest test accuracy of **85.40%**, while XGBoost achieved the highest recall of **77.15%** and Random Forest achieved the highest F1-score of **0.6267**.

Overall, the project highlights the importance of architecture selection, early stopping, class imbalance handling, threshold optimisation, and model interpretability in developing customer churn prediction models.

---

## 👩‍💻 Author

**Ananya Aggrawal**
