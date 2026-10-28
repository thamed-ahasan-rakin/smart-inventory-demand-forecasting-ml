# Smart Inventory Demand Forecasting using Machine Learning

A notebook-based machine learning project that explores retail inventory data, forecasts daily units sold, evaluates a Random Forest Regressor with time-aware cross-validation, and converts predicted demand into simple inventory and reorder recommendations.

## Project contents

- `smart_inventory_demand_forecasting.ipynb` — data checks, exploratory analysis, feature engineering, model training, evaluation plots, and sample reorder recommendations.
- `docs/Smart-Inventory-Demand-Forecasting-using-ML.pdf` — accompanying project paper.
- `data/README.md` — dataset source and setup instructions.
- `requirements.txt` — Python dependencies.
- `.gitignore` — excludes the raw dataset and generated model artifacts from Git.

## Dataset

The notebook was developed with the [Retail Store Inventory Forecasting Dataset on Kaggle](https://www.kaggle.com/datasets/anirudhchauhan/retail-store-inventory-forecasting-dataset). The paper describes 73,100 rows and 15 columns, with daily observations from January 2022 through January 2024.

The CSV is **not included** in this repository. Download it from Kaggle and place it in `data/`. Either of these filenames is supported:

- `data/retail_store_inventory-2.csv`
- `data/retail_store_inventory.csv`

See [`data/README.md`](data/README.md) for details. The source data is maintained by its publisher; review the current dataset page and its terms before redistributing the raw file.

## Setup

Use Python 3.10 or later. From the repository root, run:

```bash
python -m venv .venv
```

Activate the environment, then install the dependencies:

**Windows PowerShell**

```powershell
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter notebook
```

**macOS / Linux**

```bash
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter notebook
```

Open `smart_inventory_demand_forecasting.ipynb` and run the cells in order. Start Jupyter from the repository root so the relative `data/` path is found. The notebook also checks the original `/content/` location when run in Google Colab.

## Method overview

1. Inspect missing values, dates, stores, products, and feature relationships.
2. Explore numeric relationships with Pearson and Kendall correlations and categorical associations with bias-corrected Cramér's V.
3. Engineer calendar, lag, rolling-statistic, and inventory-related features.
4. Build a scikit-learn preprocessing/model pipeline using imputation, scaling, one-hot encoding, and a Random Forest Regressor.
5. Evaluate using five-fold `TimeSeriesSplit` on unique dates and aggregate out-of-fold (OOF) predictions.
6. Use predicted daily demand to calculate a recommended inventory level and a sample reorder quantity.

## Results reported in the original project paper

The submitted report records a baseline MAE of about **8.34** and RMSE of about **10.02**; the original notebook records a five-fold average MAE of **0.58** and RMSE of **0.86**, with OOF MAE **0.58**, RMSE **0.90**, and R² **1.000**.

### Important validation note

The current notebook calculates `inventory_gap` and `sell_through_rate` using same-day `Units Sold`, and includes those columns among the predictors for `Units Sold`. Because the target is used to construct these features, this creates a **target-leakage risk** and can make validation results look much better than real forecasting performance. Treat the reported metrics as preliminary until those features are removed or recomputed using only information available at prediction time, then rerun the notebook and update the paper/results. The current paper is included as the accompanying original report and has not been recalculated as part of this upload package.

## Inventory recommendation logic

The notebook uses a simple rule-based estimate:

- Recommended inventory = predicted daily demand × lead-time days × service factor
- Reorder quantity = max(recommended inventory − current inventory, 0)

These values depend on the assumed lead time and service factor and should be calibrated before real operational use.

## Authors

The accompanying paper credits Nowshin Ara Nini, Nusrat Lamya Faruk, and Thamed Ahasan Rakin, Department of Computer Science and Engineering, BRAC University.

## Repository notes

- The trained `rf_inventory_pipeline.joblib` artifact is generated when the notebook is run and is intentionally excluded from Git. Re-run the notebook to recreate it.
- The repository does not declare a software license. Add one only after confirming the rights and permissions for the code and any team-authored materials.
