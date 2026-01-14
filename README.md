# Brain Cell Classification using t-SNE

## Overview
This project explores **unsupervised and semi-supervised learning techniques** to identify and classify brain cell types from high-dimensional gene expression data. The analysis was completed as part of **MITx MicroMasters in Statistics and Data Science**.

Key goals of the project include:
- Identifying major brain cell types
- Discovering fine-grained cellular sub-types
- Evaluating feature importance using supervised learning
- Analyzing the effect of dimensionality reduction and model hyperparameters

---

## Project Objectives
- Visualize high-dimensional gene expression data
- Identify **three major brain cell types**
- Discover **multiple sub-types** within each main cell type
- Perform feature selection using logistic regression
- Study the impact of **t-SNE hyperparameters** on interpretability

---

## 1. Data Visualization & Dimensionality Reduction

### PCA Analysis
- Initial PCA on raw data showed no clear clustering
- Logarithmic transformation applied to stabilize extreme gene expression values
- PCA on transformed data revealed **three clearly separated clusters**

### Key Insight
Although PCA is not a clustering method, the emergence of three groups strongly suggests the presence of **three main brain cell types**, consistent with biological expectations.

---

## 2. Clustering Brain Cell Types

### Main Cell Types
- **K-Means clustering (k = 3)** applied to PCA-transformed data
- PCA plots labeled with cluster assignments showed clear separation
- Confirms existence of **three distinct brain cell categories**

### Sub-Type Discovery
- **t-SNE** applied to PCA-reduced data for non-linear visualization
- Optimal parameters selected using **KL-divergence**
  - Components: 2
  - Perplexity: ~40–50
- t-SNE visualization revealed **11 distinct sub-clusters**
- K-Means clustering with **k = 11** used to label sub-types

### Key Insight
Each major brain cell type contains multiple distinct sub-types, supporting the hypothesis of **hierarchical cellular organization**.

---

## 3. Unsupervised Feature Selection via Supervised Learning

### Logistic Regression Setup
- Cluster labels treated as ground truth
- Logistic regression trained on **original high-dimensional data**
- Logarithmic transformation applied
- Regularization tested:
  - L1 (Lasso)
  - L2 (Ridge)

### Model Selection
| Regularization | Cross-Validation Accuracy |
|---------------|--------------------------|
| L1 | ~0.86 |
| **L2** | **~0.89** |

**L2 regularization** was selected due to better stability and accuracy.

---

## 4. Gene Feature Importance Analysis

### Feature Selection
- Selected **top 100 genes** based on absolute logistic regression coefficients
- Evaluated classification performance using:
  - Top 100 coefficient-based features
  - Top 100 variance-based features
  - 100 randomly selected features

### Results
| Feature Selection Method | Accuracy |
|------------------------|----------|
| Top 100 coefficients | **~0.93** |
| Top 100 variance | ~0.92 |
| Random features | ~0.36 |

### Key Insight
Coefficient-based feature selection performs comparably to variance-based selection and **far outperforms random selection**, validating its effectiveness.

---

## 5. Hyperparameter Influence Analysis

### t-SNE Principal Components
- t-SNE run on PCA-reduced data with varying components:
  - 10, 50, 100, 250, 500 PCs
- Observed that increasing PCs:
  - Increased KL-divergence
  - Reduced cluster clarity

### t-SNE Perplexity
- Extremely low or high perplexity produced poor visualizations
- Optimal range observed: **10–50**
- Best balance between compact clusters and interpretability

### t-SNE Learning Rate
- Optimal range observed: **10–1000**
- Very low learning rates caused dispersion
- Very high learning rates caused point collapse

### Logistic Regression Regularization
- L2 retained informative features
- L1 zeroed out most coefficients
- L2 produced better downstream classification performance

---

## Tools & Libraries
- Python  
- NumPy  
- Pandas  
- Matplotlib  
- scikit-learn  
- statsmodels  

---

## Key Takeaways
- Log transformation is critical for gene expression data
- PCA + t-SNE provides powerful hierarchical visualization
- Brain cell types exhibit clear sub-structure
- Feature selection via supervised learning enhances interpretability
- Hyperparameter tuning is essential for meaningful t-SNE results

---

# Effectiveness of t-SNE Algorithm as a Clustering Method

This written report explores the effectiveness of t-SNE and k-means algorithms as clustering methods. Here, we analyze data sets of single-cell RNA sequences; the rows represent each cell and the columns represent the normalized transcript compatibility count of an equivalence class of short RNA sequences, rescaled to units of counts per million. The transcript compatibility count at location (i, j), where 'i' is the i-th row and 'j' is the j-th column, represents the level of expression of the j-th gene in the i-th cell. The data was compiled by the Allen Institute, and the data contains cells from the neocortex of mice, a region in the brain which governs higher-level functions such as perception and cognition.
