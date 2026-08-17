# Unveiling the Android App Market (Google Play Store Analysis)

## Project Overview
This project performs a comprehensive analysis of the Google Play Store ecosystem — cleaning two messy real-world datasets, exploring app category saturation, analyzing ratings and pricing trends, and conducting independent sentiment analysis on user reviews — to surface data-driven recommendations for a developer planning to launch a new app.

## Datasets
- **Apps dataset:** [googleplaystore.csv](https://raw.githubusercontent.com/malborroni/Foundations_of_Computer-Science/master/datasets/googleplaystore.csv) — 10,841 apps, 13 columns (category, rating, size, installs, price, content rating, etc.)
- **Reviews dataset:** [googleplaystore_user_reviews.csv](https://raw.githubusercontent.com/Ciroye/sentiment-analysis-google-play-reviews/master/googleplaystore_user_reviews.csv) — 64,295 user reviews, 5 columns (including pre-existing sentiment labels used only as a validation check, not a shortcut)

## Data Cleaning
- Removed 1 known corrupted row with shifted column values (Category showed "1.9", an impossible rating value confirmed the shift)
- `Installs` converted from strings like "10,000+" to integers; `Price` converted from "$4.99" strings to floats
- `Size` parsed from mixed "19M"/"14k"/"Varies with device" formats into numeric bytes, with "Varies with device" left as null rather than guessed
- Removed 483 fully duplicate rows and 698 repeat-scrape duplicates (same app, different scrape — kept the version with the highest review count)
- `Rating` nulls (13.6%) imputed using category-level medians
- Reviews dataset: dropped 26,868 rows with no review text and removed 7,735 duplicate reviews, leaving 29,692 usable reviews

**Apps dataset: 10,840 → 9,659 rows. Reviews dataset: 64,295 → 29,692 rows.**

## Category Analysis
FAMILY (1,878 apps) and GAME (945 apps) are by far the most saturated categories, together representing ~30% of the entire catalog. BEAUTY (53), COMICS (56), and PARENTING (60) are the least saturated.

## Ratings Analysis
Ratings are left-skewed (mean 4.19, median 4.3), with most apps clustering between 4.0–4.8. EVENTS (4.45) and BOOKS_AND_REFERENCE (4.38) have the highest average ratings; **DATING (4.01) has the lowest** despite moderate app count — a quality gap rather than a demand gap.

## Size vs. Installs
No meaningful correlation was found between app size and install count (r = 0.13) — smaller app size does not, on its own, predict higher installs.

## Pricing Analysis
92.2% of apps are free. Among paid apps, pricing is right-skewed (median $2.99, mean pulled up to $14.06 by a cluster of $400 novelty apps). Realistic competitive pricing sits between $0.99–$4.99. FAMILY generates the highest estimated revenue among paid apps, followed by LIFESTYLE and GAME — notably, FAMILY is both the most saturated *and* highest-revenue category.

## Sentiment Analysis
TextBlob was used to independently classify review sentiment (Positive/Neutral/Negative based on polarity thresholds), achieving 93.0% agreement with the dataset's pre-existing sentiment labels. Overall sentiment skews positive (60.2%), consistent with the high average star ratings. By category, COMICS (84.4% positive) and AUTO_AND_VEHICLES (78.8%) scored highest; **GAME had the highest negative sentiment share (29.4%)** despite a respectable average star rating — a notable mismatch between star ratings and written review sentiment, most likely reflecting user frustration with ads, monetization, or bugs that doesn't always show up in the star score.

## Interactive Visualization
A Plotly scatter plot (Rating vs. Installs, colored by Category, sized by Review count) was used to explore these relationships interactively, with hover details and category isolation via the legend.

## Conclusion: 3 Data-Driven Insights for a New App Developer
1. **Target DATING for a quality-driven entry** — proven demand (170 existing apps) but the lowest average rating of any category signals real user dissatisfaction with existing options, a more evidence-backed opportunity than an empty, unproven category.
2. **Price between $0.99–$4.99 if going paid, and don't rely on smaller app size to drive installs** — size showed virtually no correlation with installs (r = 0.13); installs are more likely driven by quality, category, and marketing.
3. **Watch review sentiment, not just star ratings, especially in GAME** — GAME's respectable star average masks the highest negative review-sentiment share of any category, suggesting star ratings alone can understate real user frustration.

## Tools Used
- Python
- pandas, numpy
- matplotlib, seaborn
- TextBlob (sentiment analysis)
- Plotly (interactive visualization)
- Jupyter Notebook

## Files in This Folder
- `play_store_analysis.ipynb` — full analysis notebook with code, charts, and written observations
- `README.md` — this file
- `screenshots/` — exported chart images referenced in this README

*(Note: both datasets are loaded directly from their GitHub source URLs within the notebook rather than stored locally in this folder, due to file size.)*

## How to Run
1. Clone this repository
2. Install dependencies: `pip install pandas numpy matplotlib seaborn textblob plotly`
3. Open `play_store_analysis.ipynb` in Jupyter Notebook
4. Run all cells (Kernel → Restart Kernel and Run All) — the notebook loads both datasets directly from their source URLs, so an internet connection is required
