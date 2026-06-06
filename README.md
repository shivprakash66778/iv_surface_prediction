# Implied Volatility Surface Prediction

This project was developed for the **Finance Club IIT Roorkee Open Project 2026**. The objective is to predict missing implied volatility values in a NIFTY options dataset containing observations across timestamps, strikes, and option types.

## Project Overview

Implied volatility is an important metric in options markets because it reflects the market’s expectation of future volatility. In real trading data, IV surfaces often contain missing values due to illiquidity, sparse trades, bid-ask filtering, or data-quality issues. This project focuses on reconstructing those missing values using available option chain data.

## Assumptions

The dataset represents implied volatility values across timestamps and strikes. I assume that IV varies smoothly across nearby strikes and also evolves gradually over time. Calls and puts are modelled separately because their volatility smiles can have different shapes. Original observed values are preserved and only missing entries are predicted.

## Preprocessing

The dataset is loaded from the Kaggle input directory. Option columns are identified using their suffixes:

- `CE` for call options
- `PE` for put options

Strike prices are extracted from option column names, and log-moneyness is computed using strike and underlying price. Missing values are detected before modelling.

## Feature Engineering

The final approach uses:

- Strike price extracted from option names
- Option type separation: CE and PE
- Log-moneyness as the cross-sectional smile variable
- Timestamp ordering for past-only residual adjustment

## Methods Explored

Several approaches were tested during development:

- Time-wise linear interpolation
- Strike-wise linear interpolation
- PCHIP interpolation
- Curvature correction
- Adaptive strike correction
- SVI/SABR-inspired modelling attempts
- Local weighted quadratic smile fitting
- Past-only residual adjustment

## Final Modelling Approach

The final model reconstructs the IV surface in two stages:

1. **Cross-sectional smile fitting**  
   For each timestamp, call and put options are fitted separately across log-moneyness. Interior missing strikes are estimated using a local weighted quadratic fit, while edge missing values use safer linear extrapolation.

2. **Causal residual adjustment**  
   After the smile fit, residual behaviour is tracked per option column using only past available observations. This helps correct persistent contract-level bias without using future information.

This approach combines financial intuition from volatility smile structure with a time-aware correction step while avoiding look-ahead bias.

## Validation Approach

Since the true missing values are hidden, local validation was performed by masking known IV values and comparing predictions using Mean Squared Error. Both random hold-out and later-period hold-out validation were used to check reconstruction quality and reduce the risk of overfitting to one missingness pattern.

## Prediction and Submission Generation

After filling missing values, the filled dataset is compared with the original dataset. Only originally missing positions are extracted and converted into Kaggle’s required format:

```text
id,value
datetime||option_column,predicted_iv
