# Supply Chain Optimisation & Analytics Dashboard

## Problem

Late deliveries damage customer satisfaction and profitability. The goal was
to identify which orders, regions, and shipping modes drive late-delivery
risk, segment customers by value and risk, and turn the findings into a
dashboard usable by operations/logistics teams.

## Approach

- **Dataset**: The DataCo Smart Supply Chain dataset provides a rich, real‑world view of customer orders, shipping behaviour, product categories, delivery performance, and financial metrics. This makes it suitable for analysing operational bottlenecks and developing predictive models to support supply chain decision‑making. [View Dataset](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis/data?select=DataCoSupplyChainDataset.csv)
- **Data preparation**: cleaned the DataCo dataset (180k+ orders), handled
  hidden nulls and duplicates, converted dates, and engineered three features:
  shipping delay risk, profit margin, and customer lifetime value (CLV).
- **Predictive modelling**: built a Logistic Regression pipeline (with
  `ColumnTransformer`/`OneHotEncoder`/`StandardScaler` preprocessing) to
  predict late-delivery risk from shipment and order features.
- **Customer segmentation**: applied K-Means clustering (k=3) on sales,
  order frequency, profit, and risk-ratio to label customers as
  VIP / Regular / Risk.
- **Dashboard**: built an interactive Tableau dashboard covering delivery
  performance, regional trends, category profitability, and sales-over-time.

## Result

- Logistic regression achieved **70% accuracy** (ROC-AUC 0.73, precision
  0.85 for the late-delivery class) — precision is notably higher than
  recall, meaning the model is conservative: when it flags an order as
  likely late, it's usually right, but it misses some late orders.
- Segmentation identified **~7,500 VIP customers** (high sales & profit) and
  a small but distinct **Risk segment (~500 customers, ~2.5%)** with high
  fraud/cancellation rates.
- Confirmed a very strong (0.95) correlation between shipment delay and
  final delivery-risk flag — flagged in the report as a likely feature
  redundancy rather than an independent predictive signal.

## Tech stack

Python (pandas, NumPy, scikit-learn, matplotlib, seaborn), Tableau,
kagglehub (dataset download).

## Getting started

```bash
git clone https://github.com/your-username/supply-chain-optimization.git
cd supply-chain-optimization
pip install -r requirements.txt
```

Then run the notebook top to bottom:

```bash
jupyter notebook "Supply Chain Optimization.ipynb"
```

The notebook pulls the dataset via `kagglehub` on first run — you'll need a
free Kaggle account/API token set up locally, or you can point the notebook
at the included `cleaned_supply_chain(For dashboard).xlsx` instead if you
just want the cleaned data.

To view the dashboard: open `Supply Chain Optimization Dashboard.twbx` in
[Tableau Desktop](https://www.tableau.com/products/desktop) (free trial
available), or publish it to
[Tableau Public](https://public.tableau.com/) for a shareable web link.

## Limitations

- The 0.95 correlation between two "independent" risk features likely
  inflates the model's apparent performance — a caveat noted directly in the
  project's own reflection rather than glossed over.
- Class imbalance affects recall on the late-delivery class; class weighting
  was applied but didn't fully resolve it.

## Files

- `Supply Chain Optimization.ipynb` — cleaning, feature engineering,
  modelling, visualisation
- `Supply Chain Optimization Dashboard.twbx` — Tableau dashboard workbook
- `DataCoSupplyChainDataset.csv` — Original dataset
- `cleaned_supply_chain(For dashboard).xlsb` — Cleaned dataset used for
  dashboard
- `requirements.txt` — Python dependencies
