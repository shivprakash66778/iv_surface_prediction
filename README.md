# Implied Volatility Surface Prediction

This project was developed for the **Finance Club IIT Roorkee Open Project 2026**. The objective was to predict missing implied volatility values in a NIFTY options dataset containing observations across multiple timestamps, strikes, and option types.

## Project Overview

Implied volatility is an important metric in options markets, as it reflects the market’s expectation of future volatility. In real trading data, IV surfaces often contain missing values due to illiquidity, sparse trades, or data-quality issues. This project focuses on reconstructing those missing values using the available option chain data.

## Approach

I explored multiple modelling techniques, including:

* Time-wise linear interpolation
* Strike-wise interpolation
* PCHIP interpolation
* Curvature correction
* Adaptive strike correction
* SVI/SABR-inspired modelling attempts

After experimentation and Kaggle leaderboard feedback, **time-wise linear interpolation** gave the most stable and reliable performance.

## Final Method

The final pipeline:

1. Loads the given dataset.
2. Identifies missing implied volatility values.
3. Applies time-wise linear interpolation across option columns.
4. Generates a fully filled dataset.
5. Converts the predictions into Kaggle’s required `id,value` submission format.

## Tech Stack

* Python
* Pandas
* NumPy
* SciPy
* Kaggle Notebook

## Files

* `iv_surface_prediction.ipynb` — Reproducible notebook
* `submission.csv` — Final Kaggle submission file
* `README.md` — Project documentation

## Key Learnings

This project helped me understand implied volatility surfaces, missing value prediction, financial time-series interpolation, model validation, and reproducible Kaggle submission workflows.

