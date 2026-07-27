# Data

This directory contains the demand datasets required to run the notebook workflow.

## Included Files

### `eia_erco_region_data_2019_2025_raw.csv`

Raw ERCOT regional data retrieved from the U.S. Energy Information Administration (EIA) Open Data API for 2019–2025.

Notebook 1 loads this file by default so the analysis can be reproduced without an EIA API key. The original API extraction code remains in the notebook for documentation.

### `demand_hourly.csv`

Cleaned hourly ERCOT demand data produced by Notebook 1.

The dataset includes calendar, lag, rolling-demand, and demand-risk features. Notebook 2 loads this file before integrating the weather data.

## Generated Data

Notebook 2 retrieves hourly weather observations from Open-Meteo for six Texas cities and generates the local database:

```text
ercot_weather.duckdb
```

Notebook 2 creates these tables:

- `demand`
- `weather`
- `demand_weather_model`

Notebook 3 adds these tables:

- `forecast_results`
- `model_evaluation`

The DuckDB database is generated locally and is not included in the repository.

## Data Workflow

```text
Raw EIA CSV
    ↓
Notebook 1
    ↓
demand_hourly.csv
    ↓
Notebook 2 + Open-Meteo weather data
    ↓
ercot_weather.duckdb
    ↓
Notebook 3
```

See [`../notebooks/README.md`](../notebooks/README.md) for setup instructions and the notebook run order.
