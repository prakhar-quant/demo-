
# 🏙️ City Lifestyle Segmentation

### Discovering City Profiles Using Unsupervised Machine Learning

This project applies **unsupervised machine learning and clustering techniques** to group cities based on socioeconomic, connectivity, environmental, and quality-of-life indicators.

The objective is to identify meaningful city-lifestyle profiles, determine an appropriate number of clusters, and understand the characteristics that distinguish each group.

---

## 📊 About the Dataset

The project uses the `city_lifestyle_dataset.csv` dataset.

| Dataset Information | Details |
|---|---|
| Total observations | 300 cities |
| Total columns | 10 |
| Numerical variables | 8 |
| Categorical variables | 2 |
| Missing values | 0 |
| Clustering target | No predefined target |

**Note:** The dataset is synthetic. Therefore, the identified clusters represent patterns within this dataset rather than verified characteristics of real-world cities.

### Key Variables

| Category | Variables |
|---|---|
| Demographic | Population Density |
| Economic | Average Income, Average Rent |
| Connectivity | Internet Penetration, Public Transport Score |
| Environmental | Air Quality Index, Green Space Ratio |
| Quality of Life | Happiness Score |
| Descriptive | City Name, Country |

`city_name` and `country` were excluded from clustering. Country was used only after clustering for geographical interpretation.

---

## 🔍 Data Preprocessing and EDA

The following steps were performed:

- Examined data types, summary statistics, and missing values.
- Analysed feature distributions and skewness.
- Investigated correlations among numerical variables.
- Selected eight numerical variables for clustering.
- Standardised all clustering variables using Z-score standardisation.

### Standardisation

Since the variables have different measurement scales, standardisation was necessary before applying distance-based clustering algorithms.

$$
Z = \frac{X-\bar{X}}{s}
$$

Where:

- $X$ = Original observation
- $\bar{X}$ = Feature mean
- $s$ = Feature standard deviation

After standardisation, each feature has approximately zero mean and unit standard deviation.

---

## 📉 Principal Component Analysis (PCA)

PCA was applied to reduce dimensionality, understand the major sources of variation, and visualise the clustering structure.

The first three principal components were retained based on the scree plot.

### Explained Variance

| Component | Explained Variance | Cumulative Variance |
|---|---:|---:|
| PC1 | 53.81% | 53.81% |
| PC2 | 25.83% | 79.64% |
| PC3 | 7.33% | 86.97% |

The first two components explain approximately **79.64%** of the total variation, providing a useful two-dimensional representation of the data.

### Interpretation of Components

- **PC1:** Primarily represents economic development, connectivity, and quality of life.
- **PC2:** Represents urban density and environmental characteristics.
- **PC3:** Primarily reflects green-space and environmental characteristics.

PCA was used for dimensionality reduction and interpretation, while clustering was performed on the standardised original variables.

---

## 🎯 Choosing the Number of Clusters

K-Means models with cluster counts from 2 to 8 were evaluated using three internal validation metrics.

| Metric | Preferred Value |
|---|---|
| Silhouette Score | Higher |
| Davies–Bouldin Index | Lower |
| Calinski–Harabasz Score | Higher |

### K-Means Results

| K | Silhouette | Davies–Bouldin | Calinski–Harabasz |
|---|---:|---:|---:|
| 2 | 0.3555 | 1.1119 | 208.86 |
| **3** | **0.3562** | **1.1027** | 195.32 |
| 4 | 0.2710 | 1.2198 | 167.30 |
| 5 | 0.2297 | 1.3582 | 144.31 |
| 6 | 0.2029 | 1.4595 | 129.46 |
| 7 | 0.1974 | 1.5445 | 119.10 |
| 8 | 0.1951 | 1.4722 | 111.93 |

Based on the validation metrics and cluster interpretability, **K = 3** was selected for the final K-Means segmentation.

---

## 🤖 Clustering Methods

Two clustering approaches were implemented and compared.

### 1. K-Means Clustering

K-Means partitions observations into K clusters by minimising the within-cluster sum of squared distances.

$$
J = \sum_{k=1}^{K}\sum_{i \in C_k}
\left\|x_i-\mu_k\right\|^2
$$

Where:

- $K$ = Number of clusters
- $C_k$ = Set of observations in cluster $k$
- $x_i$ = Observation
- $\mu_k$ = Cluster centroid

The final model used **K-Means with K = 3**.

### 2. Hierarchical Clustering

Agglomerative hierarchical clustering was implemented using **Ward's linkage**.

Ward's method merges clusters while minimising the increase in within-cluster variance.

The best Ward solution was K = 2, based on its internal validation metrics.

### Model Comparison

| Method | Selected K | Silhouette | Davies–Bouldin | Calinski–Harabasz |
|---|---:|---:|---:|---:|
| **K-Means** | **3** | **0.3562** | **1.1027** | **195.32** |
| Ward Hierarchical | 2 | 0.3152 | 1.1037 | 176.00 |

K-Means with three clusters performed better across all three reported validation metrics and was retained as the final segmentation model.

---

## 📌 Final Cluster Profiles

The final K-Means model identified three distinct city-lifestyle groups.

| Feature | Cluster 0 | Cluster 1 | Cluster 2 |
|---|---:|---:|---:|
| Population Density | 2,991 | 2,367 | 8,204 |
| Average Income | 1,434 | 3,804 | 2,532 |
| Internet Penetration | 55.30 | 86.26 | 73.00 |
| Average Rent | 495 | 1,362 | 889 |
| Air Quality Index | 78.52 | 53.76 | 97.39 |
| Public Transport Score | 40.99 | 60.76 | 63.09 |
| Happiness Score | 4.97 | 8.15 | 5.63 |
| Green Space Ratio | 36.03 | 37.08 | 25.46 |

### Cluster Interpretation

- **Cluster 0 – Lower-Income Cities:** Lower income, internet penetration, rent, public transport scores, and happiness scores.
- **Cluster 1 – Higher-Income Cities:** Higher income, internet penetration, rent, public transport scores, and happiness scores, with relatively lower population density.
- **Cluster 2 – High-Density Cities:** Highest population density, higher air quality index values, lower green-space ratio, and lower happiness than Cluster 1.

These profiles describe the average characteristics of the three groups within the dataset.

---

## 🔬 Cluster Stability and Geographic Association

### Cluster Stability

The stability of the K-Means solution was evaluated using the Adjusted Rand Index (ARI) across 20 random initialisations.

$$
ARI = 0.9862
$$

The high average pairwise ARI indicates that the three-cluster solution was highly consistent across the tested random initialisations.

### Geographic Association

Country was excluded from clustering and used only for post-clustering interpretation.

A Chi-square test of independence was conducted to examine the association between country and cluster membership.

Cramér's V was also calculated to measure the strength of association.

$$
V = \sqrt{\frac{\chi^2}{n\min(r-1,c-1)}}
$$

The analysis produced:

- **Chi-square statistic:** 400.413
- **Cramér's V:** 0.817
- **p-value:** 7.689 × 10⁻⁸⁰

The results indicate a strong association between geographical category and cluster membership within this synthetic dataset.

---

## 🏁 Conclusion

This project demonstrates how unsupervised learning techniques can uncover meaningful patterns in multidimensional city-lifestyle data.

After standardisation and PCA, K-Means and Ward hierarchical clustering were evaluated using multiple internal validation metrics.

K-Means with three clusters provided the strongest overall segmentation among the evaluated solutions. The resulting clusters represented lower-income cities, higher-income cities, and high-density cities with distinct environmental and lifestyle characteristics.

The stability analysis further supported the consistency of the selected K-Means solution.

Overall, the project demonstrates the application of **PCA, K-Means, Hierarchical Clustering, and statistical validation** for identifying and interpreting city-lifestyle patterns.

---

## 👩‍💻 Author

**Ananya Aggrawal**
