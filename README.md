# Retail Demand Forecasting Pipeline on Databricks

End-to-end data engineering pipeline built on Databricks Free Edition, following a medallion architecture (Bronze → Silver → Gold) with a downstream linear regression model for category-level demand forecasting.

## Problem
Predict daily product-category sales using historical sales patterns, to support inventory and demand planning decisions.

## Architecture
Raw CSVs (Olist e-commerce dataset) 
→ Bronze: raw ingestion into Delta tables with ingestion metadata
→ Silver: cleaned, deduplicated, joined fact table (orders, items, products, customers)
→ Gold: daily category-level aggregates with lag/rolling window features
→ ML: PySpark linear regression, tracked with MLflow

## Tech Stack
- **Databricks Free Edition** (Unity Catalog, Serverless compute)
- **PySpark** / **Delta Lake** for transformations and storage
- **MLflow** for experiment tracking
- **GitHub** (Databricks Repos / Git folders) for version control

## Data Quality
Added row-level null checks after the Silver join to catch missing prices/categories before they propagate downstream.

## Results
Linear regression on 5 features (day of week, month, lag-1, lag-7, 7-day rolling average) achieved:
- RMSE: 651.91
- R²: 0.545

This is a baseline result — a natural next step would be gradient boosting (XGBoost/LightGBM) or richer features (holidays, promotions) to improve on it.

## Orchestration
Notebooks are chained via `dbutils.notebook.run()` in `00_run_pipeline`, executing Bronze → Silver → Gold → ML sequentially. [Add a note here if you also set up a Databricks Workflow job.]

## Repo structure
