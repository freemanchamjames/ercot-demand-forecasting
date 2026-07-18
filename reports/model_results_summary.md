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

## Main Findings

HGB was the best performing model overall as well as across the specified weather and demand segments. The finding supports the EDA finding that ERCOT demand is driven by non-linear
relationships along with numerous interactions between features. MAE and RMSE results for the primary model were within 2% of the mean demand suggesting reasonable forecasting 
accuracy for an hourly demand model forecasting.

## Intended Use

This modeling workflow is best used for short-term hourly demand forecasting due to its reliance on recent lag and rolling demand features. In order for the model to perform
long-term load forecasting, it would require future weather inputs, projected lag values, or a recursive forecasting strategy.
