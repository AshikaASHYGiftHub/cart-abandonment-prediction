# Predictive Analysis of Online Shopping Cart Abandonment
### MSc Computing (Data Analytics) - Dublin City University | 2026

## Overview
This study applies the CRISP-DM methodology to predict whether 
a user session results in a purchase or abandonment using 
real-world session-level behavioural data from the Google 
Analytics 4 (GA4) public e-commerce dataset.

Raw event-level data comprising 300,000 interactions was 
aggregated into 29,178 session-level observations through 
systematic feature engineering. The severe class imbalance 
(0.89% purchase rate) was addressed using SMOTE applied 
exclusively to the training set.

---

## Research Questions
1. Can session outcomes (purchase or abandonment) be reliably 
   predicted using in-session behavioural signals?
2. Which session-level behaviours are most strongly associated 
   with purchase completion?

---

## Dataset
**Source:** Google BigQuery GA4 Obfuscated E-Commerce Public Dataset

| Detail | Value |
|---|---|
| Raw records | 300,000 event-level interactions |
| Sessions | 29,178 session-level observations |
| Purchase rate | 0.89% (259 purchase sessions) |
| Time period | January 2021 |
| Features engineered | 15 session-level features |

Access the raw dataset via:
[Google BigQuery GA4 Public Dataset](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=ga4_obfuscated_sample_ecommerce&t=events_20210131&page=table)

---

## Methodology — CRISP-DM Pipeline

```
Raw GA4 Event Data (300,000 records)
↓
Session-Level Aggregation (29,178 sessions)
↓
Missing Value Analysis & Feature Dropping
↓
IQR-Based Outlier Handling
↓
VIF Multicollinearity Analysis (4 features removed)
↓
Categorical Encoding (Geographic regions + Device type)
↓
Stratified Train/Test Split (80/20)
      ↓
SMOTE Applied to Training Set Only
↓
Model Training & Evaluation
↓
KMeans Clustering (K=3)
```
---

## Key Technical Decisions

- **IQR over Z-score** — Raw engagement time had skewness 
  of 394.58, violating normality assumption
- **VIF over correlation heatmaps** — VIF detects joint 
  multicollinearity; 4 features removed (VIF > 10)
- **SMOTE after split** — Prevents data leakage into test set
- **PCA rejected** — Required 5 components for 95% variance; 
  original features retained for interpretability
- **AIC/BIC/R² used** — Accuracy uninformative at 0.89% 
  minority class

---

## Results

### Model Comparison

| Metric | Logistic Regression | Random Forest |
|---|---|---|
| AIC | 425.15 | 349.91 |
| BIC | 525.23 | 449.99 |
| R² | 0.5826 | 0.6541 |
| ROC-AUC | 0.9982 | 0.9982 |
| Precision | 0.70 | 0.71 |
| Recall | 1.00 | 0.98 |
| F1 Score | 0.83 | 0.82 |

**Random Forest selected** — lower AIC, lower BIC, higher R²

### Top 5 Feature Importances (Random Forest)

| Feature | Importance |
|---|---|
| Payment information count | 40.1% |
| Total engagement time | 21.5% |
| Session duration | 15.9% |
| Product view count | 11.3% |
| Cart addition count | 7.0% |

### KMeans Clustering (K=3)

| Cluster | Sessions | Purchase Rate |
|---|---|---|
| Cluster 0 — Casual Browsers | 21,401 | 0.00% |
| Cluster 1 — High Intent Buyers | 295 | 57.29% |
| Cluster 2 — Engaged Non-Buyers | 7,481 | 1.20% |

---

## Key Findings
- Session outcomes are reliably predictable from behavioural 
  signals with ROC-AUC of 0.9982
- Payment information count (40.1%) is the strongest predictor 
  — users reaching payment stage are the highest value targets
- Largest funnel drop-off (80.5%) occurs between product views 
  and cart additions
- High-intent segment (Cluster 1) achieves 57.29% purchase rate 
  across just 295 sessions — not visible in aggregate statistics
- Geographic and device features contribute less than 4% 
  combined importance

---

## Visualisation Outputs

All charts are in the `Outputs/` folder:

| File | Description |
|---|---|
| `vif_chart.png` | VIF scores for all candidate features |
| `funnel_chart.png` | Purchase funnel drop-off analysis |
| `feature_importance.png` | Random Forest top 20 feature importances |
| `roc_curve.png` | ROC curve for both models |
| `elbow_plot.png` | KMeans elbow method for K selection |
| `residual_diagnostics.png` | Residual diagnostic plots |
| `boxplots_features.png` | Feature distribution boxplots |
| `eda_engagement_distribution.png` | EDA engagement time distribution |
| `pca_scree_plot.png` | PCA scree plot (PCA rejected) |

---

## Repository Structure

```
cart-abandonment-prediction/
├── Dataset/
│   ├── README.md              # Data source and access info
│   └── session_data_cleaned.csv  # Processed session-level data
├── Outputs/                   # All visualisation charts
│   ├── vif_chart.png
│   ├── funnel_chart.png
│   ├── feature_importance.png
│   ├── roc_curve.png
│   ├── elbow_plot.png
│   ├── residual_diagnostics.png
│   ├── boxplots_features.png
│   ├── eda_engagement_distribution.png
│   └── pca_scree_plot.png
├── Report/
│   └── Report_Group44.pdf
├── Data_Mining_Project.ipynb
└── README.md
```
---

## Tools & Technologies
- Python
- Scikit-learn
- Google BigQuery (GA4 Public Dataset)
- SMOTE (imbalanced-learn)
- Pandas
- Matplotlib
- Seaborn
- Jupyter Notebook

---

## Future Work
- Analyse data spanning multiple months for concept drift
- Incorporate transaction value for value-weighted prediction
- Implement real-time API scoring for live session intervention
- Apply recursive feature elimination to formally exclude 
  geographic features

---

## Ethical Considerations
The GA4 dataset is publicly available, uses pseudonymous 
identifiers, and contains no personally identifiable 
information. Analysis is conducted at aggregated session level. 
This study is intended solely for academic purposes.

---

## Authors
**Ashika Rajkumar** — MSc Computing (Data Analytics), Dublin City University

**Indumanaswini Bhukya** — MSc Computing (Data Analytics), Dublin City University

**Henry Unachukwu** — MSc Computing (Data Analytics), Dublin City University

**Mrinal Vattiprolu** — MSc Computing (Data Analytics), Dublin City University

## Contributions
| Task | Ashika | Indumanaswini | Henry | Mrinal |
|---|---|---|---|---|
| Data cleaning & preprocessing | Yes | | | |
| Feature engineering | Yes | | | |
| Result interpretation | Yes | | | |
| Report writing | Yes | Yes | Yes | Yes |
| EDA & visualisation | | Yes | | |
| Model development | | | Yes | |
| Literature review | | | | Yes |
