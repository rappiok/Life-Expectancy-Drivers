# Life Expectancy Drivers

## Overview
This project looks at what predicts life expectancy across countries, using World Bank data from 2015–2024. It covers pulling data through their API, cleaning and merging it, exploring relationships between variables, and building a regression model with proper validation. A second part looks at which countries are improving fastest over time, with more added as the project progresses.

## Motivation
Life expectancy is shaped by a tangle of economic, health, and infrastructure factors, and untangling which ones actually carry predictive weight (versus just moving together) is a genuinely hard problem — one with real relevance to how health and development resources get prioritized. This project works through that problem end to end: pulling live data, dealing with its gaps and biases honestly, and testing whether the resulting model holds up out of sample rather than just fitting the numbers in front of it.

## Data Source
- World Bank Open Data API: https://data.worldbank.org
- Indicators used: GDP per capita, health spending (% GDP), education spending (% GDP), access to sanitation, access to clean water, under-5 mortality rate, life expectancy at birth
- Data is pulled directly through the API rather than downloaded as static files, so re-running the notebook gets the latest available numbers

## Methodology
1. Data collection — Fetched each indicator for all countries across 2015–2024 through the World Bank API.
2. Cleaning — Removed regional/income-group aggregates (e.g. "World", "Sub-Saharan Africa") that get mixed in with actual countries, and dropped any country missing a value in one of the seven indicators.
3. Coverage check — Some indicators (sanitation and water access especially) have noticeably fewer countries reporting data than others, which cuts down the final sample size once everything is merged together.
4. Exploratory analysis — Checked distributions, scatter plots of each predictor against life expectancy, and a correlation matrix across all variables.
5. Regression modeling — Multiple regression to see which predictors matter most, with standardized coefficients so effect sizes are comparable across variables measured in different units.
6. Model validation — Train/test split to check the model holds up on unseen countries rather than just the ones it was fit on, VIF to quantify the multicollinearity flagged in the correlation matrix, and residual plots to check the regression's assumptions actually hold.

## Known issues / things to watch
- Sanitation and water access have the weakest coverage of all seven indicators, which shrinks the usable sample from 244 countries down to 129.
- GDP per capita and under-5 mortality are both heavily skewed, so both were log-transformed before modeling.
- Sanitation, water access, and under-5 mortality are all fairly correlated with each other, which can make it hard for the regression to cleanly separate their individual effects.
- The 129 countries with complete data may not be representative of all 244 — countries missing sanitation/water data could skew poorer or less stable, which would bias what the model's conclusions actually apply to.

## Status
Data pipeline and exploratory analysis are done. Currently adding train/test validation, VIF, and residual diagnostics. Improvement-forecast section is next after that.
