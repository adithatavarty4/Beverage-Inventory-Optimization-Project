# Beverage Distribution Inventory Optimization — Project Scope & Rubric

## 1. Project Framing

**Scenario:** You are a supply chain analyst for a regional beverage distributor/bottler. Your job is to recommend an inventory ordering policy for a set of products (SKUs) across multiple distribution regions, using historical sales data. Leadership wants two things: (1) a demand forecast they can trust, and (2) a policy that avoids both stockouts and excess holding cost — with proof that your recommendation is better than what they're doing today.

**Why this project:** it combines core data science technique (time series forecasting, simulation, model evaluation) with a genuine supply chain deliverable (inventory policy), making it relevant to both Data Science/Analytics and Supply Chain internship tracks.

**Dataset:** Kaggle "Store Item Demand Forecasting Dataset" (synthetic, 50 stores × 50 items, daily sales 2019–2023), narrowed to a working subset of 5 items ("SKUs") × 3 stores ("regions") = 15 SKU-region series, ~27K rows.

---

## 2. Learning Objectives

By the end of this project you should be able to:
- Explain the difference between moving average, exponential smoothing, and ARIMA forecasting, and when each is appropriate
- Compute and justify an EOQ, safety stock level, and reorder point from a demand forecast
- Design and run a Monte Carlo simulation to stress-test a policy under uncertainty
- Evaluate a model/policy against a baseline using an appropriate error metric, and explain why the comparison is fair
- Communicate a technical result to a non-technical audience in a one-page memo

---

## 3. Phases, Deliverables, and Rubric

### Phase 1: Data Exploration & Understanding
**Deliverable:** A short exploratory notebook/script with plots and written observations.

| Criteria | Weak | Strong |
|---|---|---|
| Visualization | One plot, no interpretation | Multiple SKUs plotted; trend/seasonality/noise explicitly identified in writing |
| Data understanding | Doesn't check for missing values/duplicates | Explicitly checks and reports data quality |
| Scope decision | Uses all 2,500 combinations or picks arbitrarily | Clear, stated rationale for the SKU/region subset chosen |

### Phase 2: Demand Forecasting
**Deliverable:** A forecast for each SKU-region series, comparing at least two methods.

| Criteria | Weak | Strong |
|---|---|---|
| Method progression | Jumps straight to ARIMA with no comparison | Builds moving average → exponential smoothing → ARIMA, compares them |
| Train/test discipline | Evaluates on the same data used to fit the model | Proper train/test split (e.g., last 90 days held out) |
| Error metric | No quantified accuracy | Reports MAPE or RMSE per method, per SKU |
| Interpretation | "The forecast looks okay" | Explains *why* one method outperforms another for a given SKU (e.g., "item_5 has strong weekly seasonality, so exponential smoothing with a seasonal component outperforms plain ARIMA") |

### Phase 3: Inventory Policy (EOQ / Safety Stock / Reorder Point)
**Deliverable:** A calculated ordering policy per SKU-region, built on top of the Phase 2 forecast.

| Criteria | Weak | Strong |
|---|---|---|
| Formula correctness | EOQ/safety stock formulas implemented incorrectly or copied without understanding | Correctly implemented, with each input (holding cost, order cost, lead time, service level) explicitly stated and justified |
| Assumptions | Unstated or arbitrary | Clearly documented (e.g., "assumed holding cost = 20% of unit price annually, lead time = 5 days, target service level = 98%, based on typical industry benchmarks") |
| Output | A single number with no context | A policy per SKU-region: order quantity, safety stock, reorder point, with the target service level stated |

### Phase 4: Risk Stress-Test (Monte Carlo)
**Deliverable:** A simulation testing the Phase 3 policy against demand and lead-time uncertainty.

| Criteria | Weak | Strong |
|---|---|---|
| Simulation design | Deterministic, no randomness | Randomizes demand (using forecast residual variance) and lead time across many simulated trials (e.g., 1,000+ runs) |
| Output metric | No clear result | Reports simulated stockout rate / service level achieved vs. target |
| Interpretation | No discussion of failure modes | Identifies which SKUs are most at risk and why (e.g., high variance, thin safety stock) |

### Phase 5: Baseline Comparison (the most important phase for resume value)
**Deliverable:** A head-to-head comparison of your policy against a naive baseline.

| Criteria | Weak | Strong |
|---|---|---|
| Baseline definition | No baseline, or an unfairly weak strawman | A reasonable naive policy clearly defined (e.g., "reorder a fixed amount when inventory drops below the trailing 30-day average sales") |
| Comparison metric | Vague ("better") | One clean quantified statement: e.g., "achieves the 98% target service level using 15% less average inventory than the baseline" or "reduces simulated stockout incidents by X% at equal holding cost" |
| Fairness | Baseline is a strawman designed to lose | Baseline reflects a genuinely plausible real-world default, not something rigged to fail |

### Phase 6: Dashboard & Communication
**Deliverable:** A Power BI/Tableau dashboard (or clean Python visualization) plus a 1-page memo.

| Criteria | Weak | Strong |
|---|---|---|
| Dashboard | Raw charts with no narrative | Clear layout: demand trend, policy summary, baseline comparison, risk metrics — tells a story at a glance |
| Memo | Jupyter notebook dump | Structured: Problem → Approach → Result (with the one quantified number) → Recommendation, written for a non-technical reader |
| Repo hygiene | Uncommented code, no README | Clean folder structure, README explaining the project, requirements listed |

---

## 4. Definition of "Done" (Minimum Viable Version)

You can honestly call this project complete and resume-worthy once you have:
1. A forecast for at least 3-5 SKUs, evaluated with a real train/test split and an error metric
2. A calculated inventory policy (EOQ, safety stock, reorder point) for those SKUs, with assumptions stated
3. At least one Monte Carlo stress-test showing how the policy performs under uncertainty
4. **One clean, defensible, quantified sentence comparing your policy to a naive baseline** — this is the single most important deliverable in the entire project
5. A README and a short memo explaining the whole thing in plain language

## 5. Stretch Goals (Optional, only after the above is solid)

- Add a facility-location/network optimization component (PuLP-based)
- Incorporate the `price`/`promo` columns into the forecast (promotion-uplift modeling)
- Extend from 5 SKUs to the full 50-item catalog with automated per-SKU policy generation
- Build a simple interactive dashboard (Streamlit or a Power BI report) instead of static charts

## 6. Resume Bullet Target (write this once Phase 5 is done)

Aim for something in this shape:
> "Built a demand forecasting and inventory optimization pipeline for a simulated beverage distributor (Python, ARIMA, Monte Carlo simulation); achieved a 98% target service level using X% less average inventory than a naive reorder policy across 5 SKUs."

The exact numbers get filled in once you have real results — don't write the bullet before you have Phase 5 done.