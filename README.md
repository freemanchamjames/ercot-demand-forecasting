# ERCOT Hourly Electricity Demand Forecasting

## Project Overview

This project forecasts hourly electricity demand for ERCOT using historical demand data, weather features, calendar features, lag features, rolling demand features, and machine learning models.

The goal is to build an applied data science project that connects forecasting performance with energy-sector business questions such as peak demand risk, weather-driven load behavior, and forecast error during high-demand periods.

## Business Questions

This project focuses on three main questions:

1. When does ERCOT demand tend to peak?
2. How do weather conditions relate to high-demand periods?
3. Can weather-enhanced machine learning models improve short-term hourly demand forecasting?

## Data Sources

### ERCOT Demand

- Source: U.S. Energy Information Administration Open Data API
- Dataset: electricity/rto/region-data
- Respondent: ERCO
- Type: Demand
- Frequency: hourly
- Period: 2019–2025

### Weather

- Source: Open-Meteo Historical Weather API
- Frequency: hourly
- Representative ERCOT/Texas cities:
  - Houston
  - Dallas
  - Austin
  - San Antonio
  - Corpus Christi
  - Midland

City-level weather records were aggregated into statewide hourly weather features before joining to demand.

## Tools Used

- Python
- pandas
- NumPy
- seaborn
- matplotlib
- scikit-learn
- DuckDB SQL
- Jupyter notebooks

## Project Workflow

1. Pull and validate hourly ERCOT demand data.
2. Pull and validate hourly weather data.
3. Aggregate city-level weather into statewide hourly weather features.
4. Join demand and weather using DuckDB SQL.
5. Explore demand patterns, weather relationships, and high-demand risk.
6. Train and evaluate forecasting models using a time-based train/test split.
7. Compare model performance overall and during high-demand and weather-risk periods.

## Modeling Approach

The final modeling workflow used a time-based split:

- Train: 2019–2024
- Test: 2025

Models compared:

- Ridge Regression
- Random Forest
- HistGradientBoosting

The workflow is best suited for short-term hourly demand forecasting because it relies on recent lagged demand and rolling demand features.

## Main Finding

HistGradientBoosting was the strongest-performing model overall and across the specified demand-risk and weather-risk segments.

This supports the EDA finding that ERCOT demand is shaped by nonlinear relationships and interactions between lagged demand, calendar patterns, and weather conditions.

## Repository Structure

```text
ercot-demand-forecasting/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   ├── 01_ercot_demand_pull.ipynb
│   ├── 02_weather_pull_EDA.ipynb
│   └── 03_ERCOT_modeling.ipynb
│
├── data/
│   └── README.md
│
├── reports/
│   └── model_results_summary.md
│
└── dashboard/
    └── README.md
