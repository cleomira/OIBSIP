## Predicting House Prices with Linear Regression

## Project Overview
This project builds and evaluates a Linear Regression model to predict house prices, covering the full end-to-end workflow from EDA and feature encoding through model training, evaluation, residual analysis, and coefficient interpretation — with a bonus comparison against Ridge and Lasso regularized models.

## Dataset
- **Source:** [Housing Prices Dataset](https://www.kaggle.com/datasets/yasserh/housing-prices-dataset) (Kaggle)
- **Size:** 545 houses, 13 columns, no missing values
- **Features:** `area`, `bedrooms`, `bathrooms`, `stories`, `parking` (numeric); `mainroad`, `guestroom`, `basement`, `hotwaterheating`, `airconditioning`, `prefarea` (binary yes/no); `furnishingstatus` (furnished / semi-furnished / unfurnished)
- **Target:** `price`

## EDA
House prices range from $1.75M to $13.3M with a right-skewed distribution (skewness = 1.21), clustering mostly between $3M–$6M with a long tail of higher-priced outliers.

## Feature Selection
Based on domain reasoning prior to modeling, `area`, `bathrooms`, and `stories` were hypothesized as strong predictors (size/quality proxies), along with `airconditioning` and `prefarea` (amenity/location-quality proxies). `bedrooms` and `hotwaterheating` were flagged as likely weaker predictors due to expected overlap with other size-related features. These hypotheses were tested against the correlation heatmap and later confirmed by the model's coefficients.

## Data Preparation
- Six binary yes/no columns were mapped directly to 1/0
- `furnishingstatus` (3 categories) was One-Hot Encoded with `drop_first=True`, using "furnished" as the baseline category to avoid the dummy variable trap
- No missing values required handling

## Correlation Findings
`area` (r=0.54) and `bathrooms` (r=0.52) showed the strongest correlation with price, followed by `airconditioning` (r=0.45) and `stories` (r=0.42). `hotwaterheating` (r=0.09) was confirmed as a weak predictor. No dangerous multicollinearity was found between independent predictors.

## Model & Evaluation
An 80/20 train/test split was used. Linear Regression achieved:

| Metric | Value |
|---|---|
| MSE | 1,754,318,687,330.66 |
| RMSE | $1,324,506.96 (~28% of average house price) |
| R² | 0.653 |

**Actual vs. Predicted / Residual Analysis:** The model performs reasonably well for houses under ~$6M, but systematically underpredicts the most expensive homes (>$8M), and residual variance increases at higher price levels — indicating heteroscedasticity and a possible non-linear relationship the linear model can't fully capture.

## Coefficient Analysis
Holding other features constant:
- **Largest positive drivers:** `bathrooms` (+$1,094,445 per bathroom), `airconditioning` (+$791,427), `hotwaterheating` (+$684,650), `prefarea` (+$629,891)
- **Largest negative driver:** `furnishingstatus_unfurnished` (−$413,645 vs. furnished baseline)
- `area`, despite having the highest simple correlation with price, showed a small per-unit coefficient (+$236/sq ft) once other correlated features (bathrooms, stories) were accounted for — much of its apparent influence is captured indirectly through those features.

## Bonus: Ridge & Lasso Comparison

| Model | RMSE | R² |
|---|---|---|
| Linear Regression | $1,324,506.96 | 0.6529 |
| Ridge | $1,325,320.44 | 0.6524 |
| Lasso | $1,325,501.26 | 0.6523 |

Regularization made virtually no difference, indicating the base model wasn't overfitting or suffering from meaningful multicollinearity. This points to the model's limitation being missing predictive features (exact location, lot size, house age/condition) and non-linear price effects rather than coefficient instability.

## Conclusion
The model explains ~65% of price variance with a typical error of ~28% of average price. Bathrooms, air conditioning, and furnishing status matter more per-unit than raw square footage. The model's main weakness is underpredicting high-end homes, suggesting future improvement should focus on adding location/condition/lot-size data and testing non-linear models (e.g. Random Forest, Gradient Boosting) rather than further linear regularization.

## Tools Used
- Python
- pandas, numpy
- scikit-learn (LinearRegression, Ridge, Lasso, train_test_split, metrics)
- matplotlib, seaborn
- Jupyter Notebook

## Files in This Folder
- `house_price_regression.ipynb` — full analysis notebook with code, charts, and written observations
- `Housing.csv` — dataset used
- `README.md` — this file
- `screenshots/` — exported chart images referenced in this README

## How to Run
1. Clone this repository
2. Install dependencies: `pip install pandas numpy scikit-learn matplotlib seaborn`
3. Open `house_price_regression.ipynb` in Jupyter Notebook
4. Run all cells (Kernel → Restart Kernel and Run All)
