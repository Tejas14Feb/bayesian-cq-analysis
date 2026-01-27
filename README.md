# AQUA005 – Bayesian Concentration–Discharge Analysis for Phosphorus

## Overview
Bayesian regression to model phosphorus (P) concentration vs stream discharge (C–Q relationship).

## Data
**Synthetic dataset** (n=200 observations) generated to simulate realistic river conditions with **dilution behavior** (negative C–Q slope).

Generation assumptions:
- log(Q) ~ Uniform(-2, 3) → Q ≈ 0.14 to 20 (scaled units, e.g. m³/s)
- log(C) = 0.5 - 0.4 × log(Q) + ε, ε ~ N(0, 0.3²)
- True parameters: α = 0.5, β = -0.4 (dilution), σ = 0.3

File: `data/synthetic_cq_phosphorus.csv`

**Note:** This is synthetic data created for demonstration and rapid task completion.  
No real USGS/NWIS data was used due to time constraints.

## How to run
1. Open `notebooks/main_analysis.ipynb`
2. Run all cells (data generation + model + plots are included)

## Model
log(C) = α + β log(Q) + ε  
Vague conjugate priors → analytical posterior (Normal-Inverse-Gamma)

## Results summary
- Posterior: α ≈ 0.519, β ≈ -0.402, σ ≈ 0.289
- Strong dilution (negative slope)
- 95% CI for β: [-0.430, -0.374]
