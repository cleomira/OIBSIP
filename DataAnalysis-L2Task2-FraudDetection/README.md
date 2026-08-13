# Fraud Detection

## Project Overview
This project builds a machine learning pipeline to detect fraudulent credit card transactions from a severely imbalanced dataset, covering EDA, class imbalance handling with SMOTE, model training and comparison, threshold-sensitive evaluation, feature importance analysis, and a discussion of production scalability.

## Dataset
- **Source:** [Credit Card Fraud Detection](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) (Kaggle, mlg-ulb)
- **Size:** 284,807 transactions, 31 columns
- **Features:** `V1`–`V28` (anonymized PCA-transformed components), `Time` (seconds since first transaction), `Amount` (transaction amount), `Class` (target: 1 = fraud, 0 = legitimate)

## Class Imbalance
Only 492 of 284,807 transactions (0.17%) are labeled fraudulent — a severe imbalance of roughly 1 fraud case per 578 legitimate transactions. A model predicting "not fraud" for every transaction would achieve 99.83% accuracy while catching zero fraud, which is the central motivation for this project's focus on precision, recall, F1, and AUC-ROC rather than accuracy.

## EDA
- **Transaction amount:** Fraudulent transactions have a higher mean amount ($122.21) than legitimate ones ($88.29), but a *lower* median ($9.25 vs. $22.00) — indicating fraud amounts are polarized between small "card-testing" transactions and occasional larger charges, rather than being uniformly high-value.
- **Time-of-day:** Fraud is disproportionately concentrated at certain hours — most notably hour 2 (2 AM), where fraud represents ~11.6% of all fraud transactions vs. only ~1.1% of non-fraud transactions at that same hour (>10x overrepresentation), and a smaller spike at hour 11. This is consistent with fraud clustering during low-oversight periods, though the dataset spans only ~2 days, so this pattern should be treated as suggestive rather than conclusively generalizable.

## Class Imbalance Handling
The data was split 80/20 (train/test) using **stratified sampling** to preserve the real-world 0.17% fraud ratio in both sets. **SMOTE** (Synthetic Minority Oversampling Technique) was then applied to the training set only, generating synthetic fraud examples to balance the training data to 50/50 — the test set was left untouched to ensure honest, real-world evaluation.

## Models Trained
1. **Logistic Regression** — linear baseline
2. **Random Forest** — non-linear ensemble model

## Results

| Model | Precision (Fraud) | Recall (Fraud) | F1 (Fraud) | AUC-ROC | False Positives |
|---|---|---|---|---|---|
| Logistic Regression | 0.08 | 0.91 | 0.15 | 0.9725 | 1,025 |
| **Random Forest** | **0.83** | 0.84 | **0.83** | 0.9583 | **17** |

Random Forest is the clear practical choice despite Logistic Regression's higher AUC-ROC — AUC-ROC measures ranking quality across all thresholds, but at the actual default decision threshold (0.5), Logistic Regression produced 1,025 false positives (operationally unusable) versus Random Forest's 17, for only a modest recall trade-off (84% vs. 91%).

## Recall vs. Precision: Which Matters More
Recall is generally prioritized in fraud detection, since a false negative (missed fraud) represents direct, undetected financial loss, while a false positive (flagged legitimate transaction) costs only a minor customer inconvenience. However, precision can't be ignored entirely — as shown by Logistic Regression's results, extremely low precision generates too many false alarms for a review team to realistically act on. In practice, the classification threshold should be tuned based on the real operational cost of manual review versus the cost of undetected fraud, rather than relying on a model's default threshold.

## Feature Importance
`V14`, `V10`, `V4`, `V12`, and `V17` were consistently top-ranked predictors across both models, providing convergent evidence these anonymized components capture genuine fraud signal. Notably, the engineered `Hour` feature and `Amount` ranked low in both models despite the clear time-of-day pattern found in EDA — suggesting the PCA-derived features already encode whatever signal correlates with transaction timing, making `Hour` largely redundant once those features are available. Since V1–V28 are PCA-anonymized for confidentiality, business-level interpretation (i.e., *why* these components matter) isn't possible — only their relative statistical importance can be reported.

## Scalability (1M Transactions/Hour)
Random Forest inference is fast enough for this volume (~278 transactions/second) on modest, parallelizable infrastructure. Production considerations beyond this notebook would include: periodic model retraining as fraud patterns evolve, a real-time feature computation pipeline (vs. this notebook's precomputed features), switching from SMOTE to `class_weight='balanced'` at scale (SMOTE doesn't scale well to millions of rows), continuous threshold monitoring against real review capacity, and infrastructure redundancy with model drift monitoring.

## Tools Used
- Python
- pandas, numpy
- scikit-learn (LogisticRegression, RandomForestClassifier, train_test_split, metrics)
- imbalanced-learn (SMOTE)
- matplotlib, seaborn
- Jupyter Notebook

## Files in This Folder
- `fraud_detection.ipynb` — full analysis notebook with code, charts, and written observations
- `creditcard.csv` — dataset used
- `README.md` — this file
- `screenshots/` — exported chart images referenced in this README

## How to Run
1. Clone this repository
2. Install dependencies: `pip install pandas numpy scikit-learn imbalanced-learn matplotlib seaborn`
3. Open `fraud_detection.ipynb` in Jupyter Notebook
4. Run all cells (Kernel → Restart Kernel and Run All)
