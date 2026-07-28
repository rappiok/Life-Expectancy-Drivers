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


## Key findings so far
- The model explains 87% of the variation in life expectancy across the 129 countries with complete data.
- Under-5 mortality is the strongest predictor by a wide margin, followed by water access and GDP per capita. This is expected rather than surprising — under-5 mortality feeds directly into how life expectancy is calculated, so its dominance reflects that overlap more than an independent causal story.
- Sanitation access and education spending show no statistically significant effect once the other variables are accounted for, likely because their signal overlaps with water access and under-5 mortality.
- Health spending came out with a negative coefficient. This is very unlikely to be a real effect — more plausibly, countries with worse health outcomes spend more reactively, which reverses the direction you'd expect (reverse causality).
- VIF scores for under-5 mortality, GDP per capita, and water access (5.4–7.0) confirm moderate multicollinearity among the model's strongest predictors — not severe enough to invalidate the regression, but enough that individual coefficients should be read as a cluster of related development indicators rather than fully independent effects.

## Known issues / things to watch
- Sanitation and water access have the weakest coverage of all seven indicators, which shrinks the usable sample from 244 countries down to 129.
- GDP per capita and under-5 mortality are both heavily skewed, so both were log-transformed before modeling.
- Sanitation, water access, and under-5 mortality are all fairly correlated with each other, which can make it hard for the regression to cleanly separate their individual effects.
- The 129 countries with complete data may not be representative of all 244 — countries missing sanitation/water data could skew poorer or less stable, which would bias what the model's conclusions actually apply to.
- Health spending's negative coefficient is likely reverse causality rather than a real effect, and shouldn't be read at face value.

## Status
Regression, standardized coefficients, and VIF are done. Train/test validation and residual diagnostics are next, followed by the improvement-forecast section.