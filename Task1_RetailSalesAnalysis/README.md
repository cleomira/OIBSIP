# Task 1: Retail Sales Analysis

## Project Overview
This project analyzes customer shopping transaction data to uncover sales trends, customer demographics, and product performance patterns, using Python, pandas, matplotlib, and seaborn in Jupyter Notebook.

## Dataset
- **Source:** [Customer Shopping Dataset – Retail Sales Data](https://www.kaggle.com/datasets/mehmettahiraslan/customer-shopping-dataset) (Kaggle)
- **Size:** 99,457 transactions
- **Time range:** January 2021 – March 2023 (final period excluded from trend analysis due to incomplete data)
- **Columns:** invoice number, customer ID, gender, age, product category, quantity, price, payment method, invoice date, shopping mall

## Objectives
- Analyze monthly and quarterly sales trends
- Examine customer demographics (age distribution, gender breakdown)
- Identify top-selling product categories and revenue by category
- Explore correlations between numerical variables
- Uncover a non-obvious insight beyond the standard analysis
- Provide actionable business recommendations

## Key Findings
- Quarterly sales remained stable (~$27.9M–$29.3M) over two years, with no clear growth trend.
- February consistently shows the lowest monthly sales in both 2021 and 2022 — driven by fewer transactions, not lower spend per transaction.
- Clothing leads in both transaction volume and total revenue, but Technology generates disproportionately high revenue (3rd highest) despite having one of the lowest transaction counts, due to a much higher average order value.
- Customer age shows no meaningful correlation with spending behavior.
- Purchase amount is driven primarily by item price rather than quantity purchased.

## Business Recommendations
1. Run February promotions focused on driving transaction volume/footfall rather than discounting.
2. Prioritize Technology as a high-margin, revenue-per-sale category rather than a volume category.
3. Avoid age-based customer segmentation, since age has no measurable relationship with purchase behavior.
4. Investigate the flat, two-year sales trend to identify growth opportunities.

## Tools Used
- Python
- pandas
- matplotlib
- seaborn
- Jupyter Notebook

## Files in This Folder
- `retail_sales_analysis.ipynb` — full analysis notebook with code, charts, and written observations
- `customer_shopping_data.csv` — dataset used
- `screenshots/` — exported chart images referenced in this README

## How to Run
1. Clone this repository
2. Install dependencies: `pip install pandas matplotlib seaborn`
3. Open `retail_sales_analysis.ipynb` in Jupyter Notebook
4. Run all cells (Kernel → Restart Kernel and Run All)
