# Data

This directory contains the demand datasets needed to run the notebook workflow.

## Included Files

### `eia_erco_region_data_2019_2025_raw.csv`

Raw ERCOT regional data retrieved from the U.S. Energy Information Administration (EIA) API for 2019–2025.

Notebook 1 loads this file by default so the analysis can be reproduced without requiring an EIA API key. The original API extraction code remains in the notebook for documentation.

### `demand_hourly.csv`

Cleaned hourly ERCOT demand data produced by Notebook 1.

The dataset includes calendar, lag, rolling-demand, and demand-risk features. Notebook 2 loads this file before integrating the weather data.

## Generated Data

Notebook 2 retrieves hourly weather observations from Open-Meteo for six Texas cities and creates the following local DuckDB database:

```text
ercot_weather.duckdb
