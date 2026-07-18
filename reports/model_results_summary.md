# Model Results Summary

## Modeling Objective
The objective was to forecast hourly ERCOT electricity demands using historical demand patterns, calendar features, lag features, rolling demand features, and weather features.\

## Train/Test Split
A time-based split was utilized to prevent leakage within the models

Train: 2019-2024
Test: 2025

## Models Compared
- Ridge Regression
- Random Forest
- HistGradientBoosting(HGB)

## Evaluation Metrics
Models were evaluated using:
- Mean Average Error(MAE)
- Root Mean Squared Error(RMSE)

Additional segment-level evaluation was performed for:

- normal demand hours
- high-demand hours
- extreme-demand hours
- hot-weather hours
- normal-weather hours
- cold-weather hours

## Test Set Results

| Model | Test MAE | Test RMSE | MAE % of Mean Demand | RMSE % of Mean Demand |
|---|---:|---:|---:|---:|
| Ridge Regression | 1113.06 | 1405.77 | 2.00% | 2.52% |
| Random Forest | 654.01 | 893.38 | 1.17% | 1.60% |
| HistGradientBoosting (HGB) | 469.58 | 625.16 | 0.84% | 1.12% |

## Main Findings

HGB was the best performing model overall as well as across the specified weather and demand segments. The finding supports the EDA finding that ERCOT demand is driven by non-linear relationships along with numerous interactions between features. On the 2025 test set, HGB achieved a Test MAE of 469.58 and a Test RMSE of 625.16. These errors represented approximately 0.84% and 1.12% of mean 2025 demand, respectively.


## Intended Use

This modeling workflow is best used for short-term hourly demand forecasting due to its reliance on recent lag and rolling demand features. In order for the model to perform
long-term load forecasting, it would require future weather inputs, projected lag values, or a recursive forecasting strategy.
