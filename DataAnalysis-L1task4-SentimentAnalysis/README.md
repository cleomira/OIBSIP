# Task 4: Sentiment Analysis

## Project Overview
This project builds and compares machine learning models that classify the sentiment of tweets (Positive, Negative, or Neutral), using a full NLP pipeline from raw text cleaning through TF-IDF feature extraction, model training, evaluation, and error analysis.

## Dataset
- **Source:** [Twitter Entity Sentiment Analysis](https://www.kaggle.com/datasets/jp797498e/twitter-entity-sentiment-analysis) (Kaggle)
- **Size:** 74,682 tweets (raw), reduced to 59,669 after filtering and cleaning
- **Columns:** tweet ID, entity (brand/product/game the tweet references), sentiment label, tweet text

## Data Preparation
- The dataset originally contained 4 classes (Positive, Negative, Neutral, Irrelevant). The **"Irrelevant"** class (12,990 rows) was dropped, since it flags off-topic tweets rather than representing a sentiment — merging it into Neutral would have contaminated that class with text unrelated to actual neutral sentiment.
- Rows with missing text were dropped.
- 1,452 tweets (2.4%) became empty strings after text cleaning (e.g. tweets consisting only of URLs or stopwords) and were removed, since they contain no usable features.
- **Final class distribution:** Negative 22,358 · Positive 20,655 · Neutral 18,108 (reasonably balanced, no severe class imbalance).

## Text Preprocessing Pipeline
- Lowercasing
- URL and @mention removal
- Unicode normalization (to catch "smart" punctuation like curly quotes that standard punctuation-stripping misses)
- Punctuation and number removal
- Tokenization
- English stopword removal
- Lemmatization (chosen over stemming to preserve readable, real words for error analysis)

## Feature Extraction
TF-IDF (Term Frequency–Inverse Document Frequency) was used to convert cleaned text into numerical features, limited to the 5,000 most informative terms (`max_features=5000`, `min_df=5`, `max_df=0.8`), down-weighting overly common words while emphasizing terms that help discriminate between sentiment classes.

## Models Trained
1. **Multinomial Naive Bayes** — fast probabilistic baseline for text classification
2. **Logistic Regression** — linear model well-suited to high-dimensional sparse TF-IDF data

Data was split 80/20 (train/test) using stratified sampling to preserve class proportions.

## Results

| Model | Accuracy | Negative F1 | Neutral F1 | Positive F1 |
|---|---|---|---|---|
| Naive Bayes | 72.3% | 0.76 | 0.64 | 0.74 |
| **Logistic Regression** | **76.4%** | **0.80** | **0.70** | **0.78** |

**Logistic Regression outperformed Naive Bayes on every metric.** The largest gap was on the Neutral class, where Logistic Regression's ability to weigh combinations of words (rather than treating each word independently) better captured the subtler language typical of neutral/factual tweets.

## Key Insight
WordCloud and error analysis both revealed that **Neutral tweets are defined by an absence of sentiment language** (dominated by brand/product names and factual statements) rather than a distinct "neutral tone" of their own — making this class structurally harder to separate from mildly-worded Positive or Negative tweets using word-frequency-based features. This explains why both models struggled most with Neutral classification.

## Error Analysis
Manual review of 5 misclassified examples identified three recurring failure patterns:
1. Genuine sentiment diluted by longer factual/descriptive content
2. Single ambiguous words carrying the entire sentiment signal (e.g. "miss")
3. Labels depending on external context/background knowledge not present in the tweet text, or plausible label noise

## Conclusion
Logistic Regression is the recommended model for this task. A real-world application would be a brand/product sentiment monitoring dashboard — e.g. a gaming company tracking public reaction to releases or platform announcements, flagging spikes in negative sentiment for rapid response. Given the model's specific weakness on Neutral classification, low-confidence or Neutral predictions should be treated cautiously or routed to human review rather than fully automated.

## Tools Used
- Python
- pandas, numpy
- NLTK (tokenization, stopwords, lemmatization)
- scikit-learn (TF-IDF, Naive Bayes, Logistic Regression, evaluation metrics)
- matplotlib, seaborn
- WordCloud
- Jupyter Notebook

## Files in This Folder
- `sentiment_analysis.ipynb` — full analysis notebook with code, visualizations, and written observations
- `twitter_training.csv` — dataset used
- `README.md` — this file
- `screenshots/` — exported chart images referenced in this README

## How to Run
1. Clone this repository
2. Install dependencies: `pip install pandas numpy nltk scikit-learn matplotlib seaborn wordcloud`
3. Open `sentiment_analysis.ipynb` in Jupyter Notebook
4. Run all cells (Kernel → Restart Kernel and Run All) — the notebook will download required NLTK resources (punkt, stopwords, wordnet) automatically on first run
