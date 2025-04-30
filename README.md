# Assessing the Impact of the COVID-19 Recession on the S&P 500 Using Synthetic Control and Stochastic Volatility Modeling

This repository contains the full code and documentation for the project exploring the causal effects of the COVID-19 pandemic on the S&P 500 index using a combination of the Synthetic Control Method (SCM) and Stochastic Volatility (SV) Modeling via custom Gibbs sampling.

**Date:** April 25, 2025  
**Paper:** Assessing the Impact of the COVID-19 Recession on the S&P 500 Using Synthetic Control and Stochastic Volatility Modeling

## Project Overview

This study combines two advanced statistical modeling techniques to evaluate both return-level and volatility-based effects of the COVID-19 shock on the S&P 500:

- **Synthetic Control Method (SCM):** Used to estimate a counterfactual S&P 500 return series in the absence of COVID-19.
- **Stochastic Volatility Modeling:** Implemented via a custom Gibbs sampler to estimate time-varying volatility in both actual and synthetic return paths.

The goal is to quantify both the Average Treatment Effect on the Treated (ATET) and differences in market uncertainty, using a novel integration of causal inference and Bayesian time series analysis.

## Repository Structure

### `causal_inference_scm_sv.Rmd`

This is the main report notebook that integrates all parts of the analysis:
- Downloads and processes return data for S&P 500 and control assets (indices + commodities)
- Applies SCM to construct counterfactual return paths
- Calculates ATET
- Plots actual vs synthetic returns
- Calls custom SV routines to estimate volatility for both series
- Produces diagnostic plots and volatility comparisons

Run this file first to reproduce the entire pipeline.

### `synthetic.Rmd`

This notebook isolates the synthetic control analysis:
- Builds synthetic controls using two donor pools:
  - Market-only donors (e.g., IXIC, DOW)
  - Expanded donors with commodities (e.g., GC=F, CL=F)
- Includes weight optimization, pre-treatment fit, and post-treatment divergence plots
- Computes ATET and visualizes return differences

Use this file to focus only on the SCM results.

### `scGibbs.Rmd`

This notebook implements the custom Gibbs sampling routine for the Stochastic Volatility Model:
- Models latent volatility via an AR(1) log-volatility process
- Estimates posterior distributions of volatility for a given return series
- Includes:
  - Full-sample estimation (2018–2022)
  - Short-window estimation (Feb–Apr 2020)
  - Diagnostics (trace plots, ACF, convergence)
  - Volatility comparison plots

Use this to understand or modify the SV estimation step independently.

### `svdiff.Rmd`

This script performs side-by-side comparison of volatility estimates:
- Loads posterior volatility estimates from the actual and synthetic runs
- Visualizes differences:
  - Scatter plots: Actual vs. Synthetic volatility
  - Histograms of volatility differences
  - Diagnostic statistics
- Provides evidence for volatility divergence (or lack thereof)

Run this for targeted comparisons of market uncertainty between the treated and counterfactual cases.

## Key Findings

- SCM with commodities produced a more credible counterfactual than using equities alone.
- The ATET post-COVID was small but positive (~0.000647), suggesting faster recovery in actual returns.
- Stochastic Volatility paths were nearly identical overall but showed slightly elevated short-run volatility in the actual S&P 500.
- The Gibbs sampler effectively captured volatility dynamics, showing minimal divergence except in a short window during early COVID.

## Requirements

To run the code, you'll need:

- R (>= 4.2)
- Packages:
  - `Synth`
  - `quantmod`
  - `xts`
  - `coda`
  - `dplyr`
  - `ggplot2`
  - `reshape2`
  - `gridExtra`
 
You can install all necessary packages with:

```r
install.packages(c("Synth", "quantmod", "xts", "coda", "dplyr", "ggplot2", "reshape2", "gridExtra"))
```

Got questions? Reach out to rgangopa@bu.edu
