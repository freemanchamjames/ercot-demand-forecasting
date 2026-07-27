# ERCOT Forecasting Dashboards

The Tableau dashboards create a business-facing view of ERCOT electricity demand, elevated-demand risk, weather conditions, and forecasting performance.

## Demand and Risk Overview

The overview dashboard summarizes historical ERCOT demand from 2019 through 2025.

It includes:

- Weekly average electricity demand.
- Monthly percentages of high- and extreme-demand hours.
- Demand patterns by hour of day.
- Temperature and weather context for selected periods.
- Interactive filtering between the weekly trend and supporting views.

This dashboard helps identify when elevated grid demand occurs and how seasonal and weather conditions contribute to demand risk.

## Forecast Performance and Risk

The forecasting dashboard evaluates model accuracy on the 2025 test period.

It includes:

- Actual versus predicted hourly demand.
- Overall model comparison.
- Forecast error over time.
- Error across normal, high, and extreme-demand segments.
- Performance during different temperature conditions.
- Underprediction risk during operationally important periods.

HistGradientBoosting produced the best overall test performance, while Ridge Regression produced the lowest error during P99+ extreme-demand hours. This distinction highlights why overall accuracy and risk-segment performance should both be considered when selecting a forecasting model.

## Dashboard Interaction

The dashboards are designed to work together as an operational analysis:

1. Use the overview dashboard to identify a period of interest.
2. Examine its demand and weather conditions.
3. Use the forecasting dashboard to evaluate model behavior and forecast risk during comparable conditions.

## Tableau Workbook

The packaged Tableau workbook (`.twbx`) contains both completed dashboards and their supporting data. It can be opened using Tableau Desktop or Tableau Public.

Dashboard screenshots are also included in this directory so the completed work can be reviewed without opening Tableau.

## Forecasting Scope

The models are intended for short-term hourly forecasting because they rely on recently observed demand through lag and rolling-demand features. Producing longer-term forecasts would require a recursive or multi-step forecasting design in which future lagged values are generated rather than directly observed.
