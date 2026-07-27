# ERCOT Hourly Electricity Demand Forecasting

## Project Overview

This project forecasts hourly electricity demand for the Electric Reliability Council of Texas (ERCOT) using historical demand, calendar patterns, lag and rolling-demand features, and statewide weather conditions.

The project connects forecasting accuracy with operations involving peak demand, weather-driven load behavior, and model performance during high-risk periods. The completed analysis is presented through three reproducible notebooks and two interactive Tableau dashboards.

## Business Questions

This project addresses three main questions:

1. When does ERCOT electricity demand tend to peak?
2. How are temperature and weather conditions related to elevated demand?
3. Can weather-enhanced machine-learning models improve short-term hourly demand forecasting?

## Data Sources

### ERCOT Demand

- **Source:** U.S. Energy Information Administration Open Data API
- **Dataset:** `electricity/rto/region-data`
- **Respondent:** ERCO
- **Type:** Demand
- **Frequency:** Hourly
- **Period:** 2019–2025

The repository includes the extracted raw EIA dataset so the notebook workflow can be reproduced without requiring an EIA API key. The original API extraction code remains in Notebook 1 for documentation.

### Weather

- **Source:** Open-Meteo Historical Weather API
- **Frequency:** Hourly
- **Locations:**
  - Houston
  - Dallas
  - Austin
  - San Antonio
  - Corpus Christi
  - Midland

City-level observations were validated and aggregated into statewide hourly weather features before being joined with ERCOT demand.

## Tools Used

- Python
- pandas
- NumPy
- scikit-learn
- seaborn
- matplotlib
- DuckDB SQL
- Jupyter Notebook
- Tableau

## Project Workflow

1. Extract and validate hourly ERCOT demand data.
2. Create calendar, lag, and rolling-demand features.
3. Establish naïve forecasting baselines and demand-only models.
4. Retrieve and validate hourly weather observations.
5. Aggregate city-level weather into statewide hourly features.
6. Join demand and weather data using DuckDB SQL.
7. Explore demand patterns, temperature relationships, and elevated-demand risk.
8. Train and evaluate forecasting models using a chronological split.
9. Compare performance overall and across demand and temperature segments.
10. Present the results in two interactive Tableau dashboards.

## Notebook Workflow

The notebooks form a sequential pipeline:

### 1. Demand Preparation and Demand-Only Modeling

`01_ERCOT_demand_pull_and_modeling.ipynb`

- Loads the supplied raw EIA dataset.
- Cleans and validates hourly demand.
- Creates calendar, lag, and rolling-demand features.
- Establishes naïve forecasting baselines.
- Evaluates demand-only Linear Regression and Random Forest models.
- Produces `data/demand_hourly.csv`.

### 2. Weather Integration and Risk EDA

`02_ERCOT_weather_pull_EDA.ipynb`

- Loads the processed demand data.
- Retrieves weather observations from Open-Meteo.
- Validates and aggregates the six city-level weather series.
- Creates the DuckDB demand, weather, and modeling tables.
- Explores temperature, demand, and elevated-risk relationships.

### 3. Forecast Modeling and Evaluation

`03_ERCOT_modeling.ipynb`

- Loads the integrated demand and weather data from DuckDB.
- Trains Ridge Regression, Random Forest, and HistGradientBoosting models.
- Evaluates performance using a chronological 2025 holdout.
- Compares errors across demand and temperature segments.
- Produces the forecast and evaluation outputs used in Tableau.

See [`notebooks/README.md`](notebooks/README.md) for setup instructions and the complete run order.

## Modeling Approach

The final modeling workflow used a chronological train/test split to prevent future observations from influencing model training:

- **Training period:** 2019–2024
- **Test period:** 2025

The models compared were:

- Ridge Regression
- Random Forest
- HistGradientBoosting

Performance was evaluated using:

- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)
- Demand-segment performance
- Temperature-segment performance
- Mean error to identify overprediction and underprediction bias

Demand and temperature thresholds were calculated from the training period to avoid using information from the 2025 test set.

## Model Results

| Model | Test MAE | Test RMSE | MAE as % of Mean Demand | RMSE as % of Mean Demand |
|---|---:|---:|---:|---:|
| Ridge Regression | 1,113.06 | 1,405.77 | 2.00% | 2.52% |
| Random Forest | 654.01 | 893.38 | 1.17% | 1.60% |
| HistGradientBoosting | 469.58 | 625.16 | 0.84% | 1.12% |

HistGradientBoosting produced the lowest overall error on the 2025 test set. Its MAE of 469.58 MW and RMSE of 625.16 MW represented approximately 0.84% and 1.12% of mean 2025 demand.

HGB also performed best during normal- and high-demand periods and across the defined temperature segments. These results support the EDA finding that ERCOT demand is influenced by nonlinear relationships and interactions among recent demand, calendar patterns, and weather conditions.

Ridge Regression, however, performed best during P99+ extreme-demand hours. Its extreme-demand MAE was 612.03 MW, compared with 820.29 MW for HGB, an improvement of approximately 25%. Ridge also produced the smallest underprediction bias during those hours.

The results demonstrate an operational tradeoff: HistGradientBoosting is the strongest general-purpose model, while Ridge Regression is more accurate at the extreme-demand tail.

## Tableau Dashboards

Two Tableau dashboards translate the analysis into a business-facing view:

### Demand and Risk Overview

- Weekly ERCOT demand trends
- Monthly elevated-demand risk
- Hourly demand patterns
- Temperature and weather context
- Interactive period selection

### Forecast Performance and Risk

- Actual versus predicted demand
- Overall model comparison
- Forecast error over time
- Demand-segment performance
- Temperature-segment performance
- Underprediction risk during operationally important periods

The packaged Tableau workbook and dashboard screenshots are available in the [`dashboard`](dashboard/) directory.

## Reproducing the Project

From the repository root, install the required packages:

```bash
pip install -r requirements.txt
```

Start Jupyter from the repository root:

```bash
python -m notebook
```

Run the notebooks in numerical order:

```text
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
Tableau forecast and evaluation outputs
```

Running Jupyter from the repository root is important because the notebooks use repository-relative file paths.

## Forecasting Scope and Limitations

This workflow is designed for short-term hourly forecasting because it relies on recently observed lag and rolling-demand features.

Longer-term forecasting would require:

- Forecasted weather inputs
- Predicted lag and rolling-demand values
- A recursive or direct multi-step forecasting strategy
- Consideration of error accumulation across the forecast horizon

Future development could also include prediction intervals and improved geographic weighting of weather conditions across ERCOT.

## Repository Structure

```text
ercot-demand-forecasting/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   ├── README.md
│   ├── 01_ERCOT_demand_pull_and_modeling.ipynb
│   ├── 02_ERCOT_weather_pull_EDA.ipynb
│   └── 03_ERCOT_modeling.ipynb
│
├── data/
│   ├── README.md
│   ├── eia_erco_region_data_2019_2025_raw.csv
│   └── demand_hourly.csv
│
├── reports/
│   └── model_results_summary.md
│
└── dashboard/
    ├── README.md
    ├── Tableau packaged workbook
    └── Dashboard screenshots
```
