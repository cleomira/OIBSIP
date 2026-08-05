# Task 2: Customer Segmentation Analysis

## Project Overview
This project applies K-Means clustering to segment an e-commerce company's customer base into distinct groups based on purchasing behaviour (Recency, Frequency, Monetary), enabling targeted marketing strategies for each segment.

## Dataset
- **Source:** [Customer Personality Analysis](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis) (Kaggle)
- **Size:** 2,240 customers (2,216 after removing rows with missing income)
- **Columns:** demographic data (birth year, education, marital status, income, household composition), purchase history (spend by product category, purchase channel counts), campaign response history, and customer enrollment date

## Data Cleaning
- Removed 24 rows with missing `Income`
- Removed an income outlier ($666,666)
- Removed implausible birth years (as early as 1893)
- Removed invalid marital status categories (`'Absurd'`, `'YOLO'`), folded `'Alone'` into `'Single'`
- Dropped two constant, non-informative columns (`Z_CostContact`, `Z_Revenue`)

## Objectives
- Engineer RFM (Recency, Frequency, Monetary) features from raw purchase data
- Standardise features and determine the optimal number of clusters using the Elbow Method
- Apply K-Means clustering to segment customers
- Visualise and profile each cluster
- Recommend a targeted marketing action for each segment

## Methodology
1. Calculated `Frequency` (total purchases across all channels) and `Monetary` (total spend across all product categories) per customer; `Recency` was already provided in the dataset.
2. Standardised Recency, Frequency, and Monetary using `StandardScaler`.
3. Used the Elbow Method to determine the optimal number of clusters — **K = 4** was selected, based on the inertia curve flattening noticeably after this point.
4. Fit a K-Means model and visualised clusters across three feature-pair combinations (Recency–Monetary, Frequency–Monetary, Recency–Frequency).
5. Profiled each cluster by mean Recency, Frequency, Monetary, Average Purchase Value, Age, and Income.

## Key Findings
Four evenly sized customer segments (23–27% of the base each) were identified:

| Cluster | Size | Profile | Recency | Frequency | Monetary | Income |
|---|---|---|---|---|---|---|
| 0 | 598 (26.6%) | New/Casual Low-Spenders | 24.51 | 8.64 | $128.28 | $36,661 |
| 1 | 570 (25.4%) | Disengaged Low-Value | 74.77 | 8.98 | $135.79 | $37,354 |
| 2 | 534 (23.8%) | At-Risk High-Value | 73.32 | 21.51 | $1,151.33 | $68,615 |
| 3 | 506 (22.6%) | Loyal Champions | 23.51 | 21.92 | $1,128.69 | $68,845 |

Income, not age, appears to be the primary driver of purchase value, high- and low-spending cluster pairs differ sharply in income (~$68.6K–$68.8K vs. ~$36.7K–$37.4K) but only modestly in age (42.7–47.8 across all clusters).

## Marketing Recommendations
1. **Loyal Champions (Cluster 3):** Reward and retain via VIP perks, early product access, and referral incentives, avoid heavy discounting since they already convert well.
2. **At-Risk High-Value (Cluster 2):** Highest-priority segment; launch an urgent, personalised win-back campaign, as this group represents the largest revenue risk in the customer base.
3. **Disengaged Low-Value (Cluster 1):** Low-cost, automated re-engagement only (e.g. a "we miss you" email); not worth heavy investment given low historical value.
4. **New/Casual Low-Spenders (Cluster 0):** Nurture and grow through onboarding sequences and small first-purchase incentives, this segment has the most upside since they're already engaged but not yet high-value.

## Tools Used
- Python
- pandas
- scikit-learn (StandardScaler, KMeans)
- matplotlib
- seaborn
- Jupyter Notebook

## Files in This Folder
- `customer_segmentation_analysis.ipynb` — full analysis notebook with code, charts, and written observations
- `customer_segmentation.csv` — dataset used
- `README.md` — this file
- `screenshots/` — exported chart images referenced in this README

## How to Run
1. Clone this repository
2. Install dependencies: `pip install pandas scikit-learn matplotlib seaborn`
3. Open `customer_segmentation_analysis.ipynb` in Jupyter Notebook
4. Run all cells (Kernel → Restart Kernel and Run All)
