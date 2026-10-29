# Beverage Inventory Optimization

A demand forecasting and inventory optimization project for a simulated regional beverage distributor. The goal is to recommend an ordering policy that avoids both stockouts and excess holding cost, and to show it beats a naive baseline.

> **Status: in progress.** Phases 1-3 are done for one SKU-region (item_1 / store_1). The other 14 series, the Monte Carlo stress-test, the baseline comparison and the dashboard are still to do.

## Data

Synthetic daily retail sales (Kaggle "Store Item Demand Forecasting"-style data, 2019-2023): 50 stores x 50 items. Columns: `date, store_id, item_id, sales, price, promo, weekday, month`.

The analysis uses a working subset of **5 items ("SKUs") x 3 stores ("regions") = 15 series**.

`retail_sales.csv` (~195 MB) is not in the repo because it exceeds GitHub's 100 MB file limit. Place it in the project root before running the notebook.

## Approach

| Phase | What | Status |
|---|---|---|
| 1. Exploration | Trend, seasonality, data quality, promo effect | Done |
| 2. Forecasting | SARIMAX with promo as an exogenous variable, weekly aggregation, held-out test plus walk-forward backtest | Done for item_1 / store_1 |
| 3. Inventory policy | EOQ, safety stock, reorder point at a 98% service level | Done for item_1 / store_1 |
| 4. Monte Carlo stress-test | Random demand and lead time across 1,000+ runs | To do |
| 5. Baseline comparison | Policy vs. a naive reorder rule | To do |
| 6. Dashboard and memo | Visuals plus a 1-page memo | To do |

## Findings so far

- Sales show clear seasonality (rising January to May, dipping August to November).
- Promo days sell about 56% more than non-promo days (about 72.9 vs 46.8 units per day), so promo is included as an exogenous variable.
- A weekly SARIMAX fit was chosen over a daily one with a 365-day seasonal period, which is too heavy to fit reliably.
- The held-out test gave a MAPE of about 2.3%. That is low enough to be suspicious, so a walk-forward backtest was added to get a more robust estimate of forecast error.
- Forecast error (sigma, per week): **7.07** from the single held-out test, **11.27** from the pooled backtest. The conservative value is used as the primary one.

### item_1 / store_1 policy (98% service level)

| Input | Value |
|---|---|
| Unit price | $21.30 |
| Holding cost | $4.26 / unit / year (20% of price) |
| Order cost | $75 / order |
| Lead time | 2 weeks |
| Average demand | 313.3 units/week (about 16,291/year) |
| **EOQ** | **about 757 units per order** |

Safety stock = Z x sigma x sqrt(lead time), with Z = 2.054. Reorder point = average weekly demand x lead time + safety stock. Both are reported as a range, using the optimistic and conservative sigma. Phase 4 will test which is adequate.

## Repo contents

- `explore.ipynb`: the analysis notebook (exploration, forecasting, policy)
- `scope.md`: project scope, phases and rubric
- `what_i_learned.md`: notes on the concepts (EOQ, safety stock, ARIMA/SARIMAX, etc.)

## Requirements

Python 3 with `pandas`, `numpy`, `matplotlib`, `scipy` and `statsmodels`, plus Jupyter to run the notebook.

## Next steps

1. Wrap the fit, backtest, sigma and policy pipeline into a function and run it for at least 3-5 SKU-regions.
2. Monte Carlo stress-test of the policy under demand and lead-time uncertainty.
3. Compare against a naive baseline and report one quantified result.
4. Dashboard and 1-page memo.
