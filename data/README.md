# Data Notes

This project uses public hourly electricity demand and weather data.

## Demand Data

- Source: U.S. Energy Information Administration Open Data API
- Dataset: electricity/rto/region-data
- Respondent: ERCO
- Type: Demand
- Frequency: hourly
- Period used: 2019–2025

## Weather Data

- Source: Open-Meteo Historical Weather API
- Frequency: hourly
- Timestamps aligned to UTC
- Representative ERCOT/Texas locations:
  - Houston
  - Dallas
  - Austin
  - San Antonio
  - Corpus Christi
  - Midland

## Local Data Policy

Raw data files, intermediate exports, and DuckDB database files are not committed to this repository.

The modeling dataset can be recreated from the notebooks.
