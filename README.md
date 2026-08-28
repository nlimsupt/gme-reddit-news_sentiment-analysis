# GameStop Reddit and Institutional News Sentiment Analysis

## Project Overview

The GameStop short squeeze of early 2021 highlighted the growing role of online investor communities in financial markets. During this period, discussions on Reddit—particularly r/wallstreetbets—occurred alongside extensive institutional news coverage and unusually large movements in GameStop (GME) stock.

This project compares **Reddit investor discussions and institutional financial news as two distinct information sources** during the GameStop episode and examines whether sentiment derived from either source was associated with short-term GME market movements.

The analysis combines natural language processing, statistical inference, predictive modeling, time-series analysis, and event-study methods to examine both differences in the information environment and potential relationships between sentiment and stock returns.

---

## Research Questions

### Primary Research Question

**Does institutional-news sentiment provide stronger explanatory and predictive power for short-term GME returns than Reddit sentiment during the GameStop short-squeeze period?**

### Supporting Questions

1. How did sentiment differ between Reddit discussions and institutional news coverage?
2. What major topics characterized each information source?
3. Was daily sentiment associated with same-day GME returns?
4. Did lagged sentiment help explain or predict next-day GME returns?
5. Did Reddit or institutional-news sentiment demonstrate a lead-lag relationship with GME returns?
6. Did GME experience abnormal market-adjusted returns around major events during the short-squeeze period?

---

## Data Sources

The project integrates three types of data:

### Reddit Data

GameStop-related posts were collected from **r/wallstreetbets** using the Arctic Shift API.

- Raw observations: 2,439 posts
- Final processed observations: 2,361 posts
- Period: December 1, 2020 – March 30, 2021
- Key fields include post text, timestamps, and Reddit scores

Post titles and available self-text were cleaned and combined for subsequent sentiment and topic analyses.

### Institutional News Data

GME-related institutional news records were collected using the GDELT DOC 2.0 API.

- Raw observations: 2,069 articles
- Final processed observations: 1,334 articles
- Period: January 15, 2021 – March 31, 2021
- 366 unique publishing domains

Article headlines were used as the primary text source for sentiment analysis and topic modeling.

### Market Data

Daily market data were collected for:

- GameStop (`GME`)
- SPDR S&P 500 ETF Trust (`SPY`)
- AMC Entertainment (`AMC`)
- BlackBerry (`BB`)

The processed price, return, and volume datasets each contain 125 trading-day observations covering October 1, 2020 through March 31, 2021.

GME is the primary security analyzed, while SPY serves as the broader-market benchmark in the return and event-study analyses.

---

## Analytical Methodology

The project uses multiple analytical methods to examine different aspects of the research questions.

### Sentiment Analysis

VADER was used to measure sentiment in Reddit posts and institutional-news headlines. Reddit sentiment was additionally evaluated using a WallStreetBets-specific augmented lexicon to account for community-specific financial terminology.

Sentiment differences between Reddit and institutional news were evaluated using:

- Mann-Whitney U test
- Welch's t-test
- Cohen's d

### Topic Modeling

Latent Dirichlet Allocation (LDA) was applied separately to Reddit posts and institutional-news headlines to identify major themes within each information source.

### Sentiment and Return Relationships

Daily sentiment measures were integrated with GME market data to evaluate same-day and lagged relationships using:

- Pearson correlation
- Spearman correlation
- Chi-square test
- Ordinary Least Squares (OLS) regression

### Predictive Modeling

Logistic Regression and Random Forest models were used to evaluate whether lagged sentiment and market variables could predict next-day GME price direction.

Three feature configurations were compared:

- Reddit-based features
- Institutional-news-based features
- Combined Reddit, news, and market features

Models were evaluated using **5-fold TimeSeriesSplit cross-validation** to preserve temporal ordering.

### Lead-Lag Analysis

Granger causality tests were used to examine whether Reddit or institutional-news sentiment provided statistically significant information about future GME returns at one- and two-day lags.

### Event Study

An event study examined abnormal GME returns around six major events during the GameStop episode. Expected returns were estimated relative to SPY using a pre-event market model, and cumulative abnormal returns (CARs) were evaluated over a nine-trading-day event window.

---

## Key Findings

### Reddit and Institutional News Reflected Different Information Environments

Reddit sentiment was substantially more positive than institutional-news sentiment. Mean VADER compound sentiment was **0.3054 for Reddit** compared with **0.0107 for institutional news**.

The difference was statistically significant under both the Mann-Whitney U test and Welch's t-test, while Cohen's d of **0.5641** indicated a medium effect size.

The WallStreetBets-specific lexicon also affected Reddit sentiment measurement: compound sentiment scores changed for **74.5%** of posts and sentiment classifications changed for **18.6%** of posts relative to standard VADER.

Topic modeling further showed that Reddit emphasized retail trading behavior, short squeezes, options, trading restrictions, and GameStop-specific company developments, while institutional news emphasized short sellers, hedge funds, broader meme-stock coverage, and the wider GameStop market narrative.

### Daily Sentiment Was Not Reliably Associated With GME Returns

Neither Reddit nor institutional-news sentiment showed a statistically significant same-day relationship with GME returns.

Lagged OLS models produced similar results. None of the sentiment variables significantly explained next-day GME returns, with model R-squared values ranging from **0.0452 to 0.1054**.

### Sentiment Had Limited Out-of-Sample Predictive Value

Across the time-series cross-validation experiments, mean classification accuracy ranged from **0.400 to 0.600**, while mean ROC-AUC ranged from **0.436 to 0.613**.

The strongest average result was produced by the news-only Random Forest model, with mean accuracy of **0.600** and mean ROC-AUC of **0.613**, but performance varied substantially across folds.

Combined Reddit and institutional-news features did not consistently improve predictive performance.

Granger causality tests also found no statistically significant evidence that either sentiment source predicted future GME returns at one- or two-day lags.

### Major Events Were Associated With Large Abnormal Returns

Despite the weak daily sentiment-return relationships, GME experienced unusually large market-adjusted movements around several major events.

Cumulative abnormal returns were statistically significant for **five of the six selected events**. The largest positive CAR occurred around the January 22 major price spike (**CAR = 2.2665**), while the February 1 post-restriction selloff produced a large negative CAR (**CAR = -1.1036**).

These findings indicate that major events coincided with extraordinary GME price movements, but the event study does not establish that Reddit or institutional-news sentiment caused those movements.

---

## Repository Structure

```text
├── data/
│   ├── raw/
│   └── processed/
│
└── notebooks/
    ├── 01_reddit_data_collection.ipynb
    ├── 02_news_data_collection.ipynb
    ├── 03_stock_data_collection.ipynb
    ├── 04_data_preprocessing.ipynb
    └── 05_data_analysis.ipynb
```

| Notebook | Purpose |
| --- | --- |
| `01_reddit_data_collection.ipynb` | Collects GameStop-related r/wallstreetbets posts using the Arctic Shift API. |
| `02_news_data_collection.ipynb` | Collects institutional news coverage using the GDELT DOC 2.0 API. |
| `03_stock_data_collection.ipynb` | Collects GME, SPY, AMC, and BB market data. |
| `04_data_preprocessing.ipynb` | Cleans, validates, deduplicates, and transforms Reddit, news, and market datasets. |
| **`05_data_analysis.ipynb`** | **Main analysis notebook containing the analytical results, discussion, and conclusions.** |

---

## How to Review This Project

For readers primarily interested in the analytical results, **`05_data_analysis.ipynb` is the main notebook**. It contains the exploratory analysis, sentiment analysis, topic modeling, statistical tests, predictive modeling, Granger causality analysis, event study, discussion, and final conclusions.

Notebooks `01`–`04` document the preceding data-collection and preprocessing pipeline.

---

## Tools and Technologies

- **Python**
- **pandas / NumPy** — data preparation and analysis
- **Matplotlib** — data visualization
- **NLTK VADER** — sentiment analysis
- **scikit-learn** — topic modeling and predictive modeling
- **statsmodels** — OLS regression and Granger causality analysis
- **SciPy** — statistical hypothesis testing
- **Arctic Shift API** — Reddit data collection
- **GDELT DOC 2.0 API** — institutional news collection
- **yfinance** — financial market data collection
- **Jupyter Notebook** — analytical workflow and documentation

---

## Limitations

Several limitations should be considered when interpreting the findings.

- The Reddit and institutional-news datasets cover different periods, reducing the overlapping sample available for direct comparison and combined modeling.
- The number of daily observations available for predictive modeling was relatively small, particularly for institutional news and combined models.
- VADER is a lexicon-based sentiment method and may not capture all context, sarcasm, or domain-specific meanings in financial discussions.
- LDA topics require subjective interpretation and may vary with preprocessing choices and model configuration.
- Daily aggregation may obscure intraday relationships between sentiment and market movements during the highly volatile GameStop episode.
- The predictive models used a limited feature set and were intended to test whether sentiment provided a stable signal rather than to construct a production trading model.
- Event windows overlap for several major events, and the selected dates were based on the known GameStop narrative. The event-study results therefore identify abnormal returns around these events but should not be interpreted as causal evidence.

---

## Conclusion

The analysis does not support the initial expectation that institutional-news sentiment provides stronger explanatory or predictive power for short-term GME returns than Reddit sentiment.

Reddit and institutional news differed meaningfully in both sentiment and thematic emphasis, indicating that they represented distinct information environments during the GameStop episode. However, these differences did not translate into statistically reliable same-day relationships, significant lagged relationships, stable out-of-sample predictive performance, or Granger-causal relationships with daily GME returns.

At the same time, the event study identified unusually large abnormal returns around several major events. Overall, the findings suggest that the GameStop episode cannot be explained by a simple relationship in which more positive Reddit or institutional-news sentiment consistently leads to higher short-term returns. Daily sentiment alone was insufficient to explain or reliably predict GME's extreme price movements.
