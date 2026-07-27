# Model Results Summary

## Modeling Objective

The objective was to forecast hourly ERCOT electricity demand using historical demand patterns, calendar features, lag features, rolling-demand features, and statewide weather features.

## Train/Test Split

A chronological split was used to prevent future observations from influencing model training:

- **Training period:** 2019–2024
- **Test period:** 2025

## Models Compared

- Ridge Regression
- Random Forest
- HistGradientBoosting (HGB)

## Evaluation Metrics

Models were evaluated using:

- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)

Additional evaluation was performed across:

- Normal-demand hours
- High-demand hours
- Extreme-demand hours
- Normal-temperature hours
- Hot-temperature hours
- Cold-temperature hours

Demand and temperature segments were defined using thresholds calculated from the training period.

## Test-Set Results

| Model | Test MAE | Test RMSE | MAE as % of Mean Demand | RMSE as % of Mean Demand |
|---|---:|---:|---:|---:|
| Ridge Regression | 1,113.06 | 1,405.77 | 2.00% | 2.52% |
| Random Forest | 654.01 | 893.38 | 1.17% | 1.60% |
| HistGradientBoosting (HGB) | 469.58 | 625.16 | 0.84% | 1.12% |

## Main Findings

HistGradientBoosting produced the lowest overall MAE and RMSE on the 2025 test set. Its MAE of 469.58 MW and RMSE of 625.16 MW represented approximately 0.84% and 1.12% of mean 2025 demand, respectively.

HGB also performed best during normal- and high-demand periods and across normal, hot, and cold temperature conditions. These results support the EDA finding that ERCOT demand is influenced by nonlinear relationships and interactions among historical demand, calendar, and weather features.

Ridge Regression performed best during P99+ extreme-demand hours. Its extreme-demand MAE was 612.03 MW, compared with 820.29 MW for HGB—an improvement of approximately 25%. Ridge also had the smallest underprediction bias during these hours.

The results therefore show an operational tradeoff: HGB is the strongest general-purpose forecasting model, while Ridge is more accurate at the extreme-demand tail.

## Intended Use and Limitations

This workflow is best suited to short-term hourly demand forecasting because it relies on recently observed lag and rolling-demand features.

Longer-term forecasting would require:

- Forecasted weather inputs
- Predicted lag and rolling-demand values
- A recursive or direct multi-step forecasting strategy
- Consideration of error accumulation across the forecast horizon
