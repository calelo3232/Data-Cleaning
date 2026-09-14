# Congressional Trading Analysis

An analysis of [U.S. Senators' stock trading disclosures](https://www.kaggle.com/datasets/heresjohnnyv/congress-investments): do senators cluster into distinguishable trading "styles," and can sale/purchase volume be forecast from historical trends? All analysis is in [`data_cleaning.ipynb`](data_cleaning.ipynb).

## What's inside

**Exploratory Data Analysis** &mdash; distribution of asset types, most-traded assets, and most-active traders in the raw disclosure data.

**Clustering Senators by Trading Behavior** &mdash; each senator's sequence of (asset, transaction date) sales is fed into a Gaussian Hidden Markov Model, and each senator is assigned to whichever hidden state their sequence visits most. The resulting clusters are then broken down by political party to see whether trading behavior tracks party affiliation (it doesn't, cleanly &mdash; see the notebook's cluster output).

**Time Series Forecasting** &mdash; sale and purchase volume since 2014 are seasonally decomposed, then an auto-ARIMA search picks a model order which is fit and used to forecast future sale volume.

## Running it

```bash
pip install -r requirements.txt
```

You'll also need the source dataset: download `SenatorCleaned.csv` from the [Kaggle dataset above](https://www.kaggle.com/datasets/heresjohnnyv/congress-investments) and place it in the same directory as the notebook (it isn't checked into this repo).

```bash
jupyter notebook data_cleaning.ipynb
```

Or open directly in [Google Colab](https://colab.research.google.com/github/calelo3232/Data-Cleaning/blob/main/data_cleaning.ipynb) and upload `SenatorCleaned.csv` when prompted.

## Notes on the approach

- Senator-to-party mapping is a hardcoded lookup built from the senators present in this dataset (`senators_party` in the notebook); `load_and_clean_data()` warns if it encounters a name that isn't in the mapping rather than silently dropping it.
- The seasonal decomposition and ARIMA cells expect enough daily-resolution history to detect a yearly cycle (roughly two years of data); they won't run meaningfully on a small or sparse subset of the data.
