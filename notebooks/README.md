# Notebook Workflow

The notebooks form a sequential workflow for extracting and preparing ERCOT demand data, integrating weather conditions, and evaluating forecasting models.

## Setup

Install the required Python packages from the repository root:

```bash
pip install -r requirements.txt
```

Then start Jupyter from the repository root so the relative file paths resolve correctly:

```bash
python -m notebook
```

Run the notebooks in numerical order.

## 1. Demand Preparation and Demand-Only Modeling

`01_ERCOT_demand_pull_and_demand_only_modeling.ipynb`

- Loads the provided raw EIA dataset from `data/eia_erco_region_data_2019_2025_raw.csv`.
- Cleans and validates ERCOT hourly demand data.
- Creates calendar, lag, and rolling-demand features.
- Establishes naïve forecasting baselines.
- Evaluates demand-only Linear Regression and Random Forest models.
- Produces `data/demand_hourly.csv` for Notebook 2.

The notebook also retains the original EIA API extraction code for documentation. The supplied raw CSV allows the notebook to run without an EIA API key.

## 2. Weather Integration and Risk Analysis

`02_ERCOT_weather_integration_and_risk_EDA.ipynb`

- Loads `data/demand_hourly.csv`.
- Retrieves hourly weather observations from Open-Meteo for six Texas cities.
- Validates and aggregates the weather observations into statewide hourly features.
- Creates the local DuckDB database `ercot_weather.duckdb`.
- Stores the `demand`, `weather`, and `demand_weather_model` tables.
- Explores relationships among temperature, demand, and elevated-demand risk.

## 3. Forecast Modeling and Evaluation

`03_ERCOT_modeling.ipynb`

- Reads the `demand_weather_model` table from DuckDB.
- Uses a chronological 2025 holdout for model evaluation.
- Compares Ridge, Random Forest, HistGradientBoosting, and XGBoost models.
- Evaluates performance across demand and temperature segments.
- Creates the `forecast_results` and `model_evaluation` outputs used by Tableau.

## Data Handoffs

```text
Raw EIA CSV
    ↓
Notebook 1
    ↓
data/demand_hourly.csv
    ↓
Notebook 2
    ↓
ercot_weather.duckdb
    ↓
Notebook 3
    ↓
Forecast and model-evaluation outputs
```

The notebooks should be run from the repository root and in numerical order because each stage produces data used by the next stage.
