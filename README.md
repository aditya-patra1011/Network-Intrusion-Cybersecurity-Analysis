# Network Intrusion Detection — Data Analysis Project

A machine learning pipeline for detecting network intrusions using statistical analysis and classification models on network traffic data.

---

## Overview

This project builds a binary classifier to distinguish malicious network traffic (attacks) from normal connections. It walks through the full data science workflow — from raw data inspection to model evaluation — using the `network_intrusion.csv` dataset.

---

## Project Structure

```
├── Data_Analysis_Network.ipynb   # Main analysis notebook
├── network_intrusion.csv         # Input dataset (required)
├── eda_plots.png                 # EDA visualizations
├── correlation_heatmap.png       # Feature correlation matrix
├── target_correlation.png        # Per-feature correlation with target
├── cm_logistic.png               # Confusion matrix — Logistic Regression
├── cm_decisiontree.png           # Confusion matrix — Decision Tree
├── cm_randomforest.png           # Confusion matrix — Random Forest
├── roc_curve.png                 # ROC curves for all models
├── model_comparison.png          # Accuracy vs ROC-AUC bar chart
└── feature_importance.png        # Random Forest feature importances
```

---

## Notebook Phases

### Phase 1 — Load & Understand
Loads the dataset and prints shape, column names, data types, missing values, and target/attack-type distributions.

### Phase 2 — Data Preprocessing
- Label-encodes categorical columns (`protocol_type`, `flag`)
- Drops `attack_type` to prevent data leakage
- Splits data into train/test sets (80/20, stratified)
- Applies `StandardScaler` to all features

### Phase 3 — Exploratory Data Analysis (EDA)
Generates a 6-panel visualisation covering:
- Attack type distribution
- Protocol type vs traffic type
- Connection duration by traffic type
- Source bytes by traffic type
- Failed login attempts vs traffic type
- SYN error rate by attack type

### Phase 4 — Statistical Analysis
Runs hypothesis tests to identify features that significantly differ between normal and attack traffic:
- **T-tests** on `duration`, `src_bytes`, `serror_rate`, `num_failed_logins`
- **Chi-square tests** on `protocol_type` and `logged_in`

### Phase 5 — Correlation Analysis
Produces a full feature correlation heatmap and a ranked bar chart of each feature's correlation with the target label.

### Phase 6 — Model Building & Evaluation
Trains and evaluates three classifiers:

| Model | Metric |
|---|---|
| Logistic Regression | Accuracy, ROC-AUC, Confusion Matrix |
| Decision Tree (max_depth=10) | Accuracy, ROC-AUC, Confusion Matrix |
| Random Forest (100 estimators) | Accuracy, ROC-AUC, Confusion Matrix |

Also performs **5-fold cross-validation** via `sklearn.pipeline.Pipeline` and plots a combined ROC curve and model comparison chart.

### Phase 7 — Feature Importance
Extracts Random Forest feature importances, highlights features above the median, and prints the top 5 most predictive features.

---

## Requirements

Install dependencies with:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy
```

| Library | Purpose |
|---|---|
| `pandas`, `numpy` | Data manipulation |
| `matplotlib`, `seaborn` | Visualisation |
| `scikit-learn` | ML models, preprocessing, evaluation |
| `scipy` | Statistical tests (T-test, Chi-square, ANOVA) |

---

## Usage

1. Place `network_intrusion.csv` in the same directory as the notebook.
2. Launch Jupyter:
   ```bash
   jupyter notebook Data_Analysis_Network.ipynb
   ```
3. Run all cells top-to-bottom (`Kernel → Restart & Run All`).
4. All plots are saved as `.png` files in the working directory.

---

## Dataset

The notebook expects a CSV file named `network_intrusion.csv` with at least the following columns:

| Column | Description |
|---|---|
| `target` | Binary label — `0` = Normal, `1` = Attack |
| `attack_type` | Multi-class attack category (dropped before training) |
| `protocol_type` | Network protocol (categorical) |
| `flag` | Connection flag (categorical) |
| `duration` | Connection duration in seconds |
| `src_bytes` | Bytes sent from source |
| `serror_rate` | SYN error rate |
| `num_failed_logins` | Number of failed login attempts |
| `logged_in` | Whether the user was logged in (binary) |

---

## Key Design Decisions

- **Data leakage prevention** — `attack_type` is dropped before model training since it directly encodes the target.
- **Pipeline-based cross-validation** — scaling is applied inside the pipeline to prevent leakage from the test fold.
- **Stratified splitting** — preserves class balance across train/test sets.

---

## Output Files

All charts are saved as 150 DPI PNG files and can be used directly in reports or presentations.
