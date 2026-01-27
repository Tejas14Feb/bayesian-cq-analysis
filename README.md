# AQUA005 – Bayesian Concentration–Discharge (C–Q) Analysis for Phosphorus

**Author:** Tejas Budharamu  
**Affiliation:** University of South Dakota, Department of Computer Science  
**Date:** January 2026  

## Task Overview
This repository contains the complete submission for the AQUA005 Mini Research Task:  
Bayesian regression analysis of phosphorus concentration (C) vs. stream discharge (Q) relationships.

## Data (Task 1)
**Source:** Synthetic dataset (n=200 observations)  
**Reason for synthetic data:** Time constraints and insufficient paired discrete phosphorus + continuous discharge samples from USGS site 06479010 (Vermillion River near Vermillion, SD) made real-data merging impractical for this deadline.

**Generation assumptions (fully reproducible):**
- Discharge (Q): log(Q) ~ Uniform(-2, 3) → Q ranges ≈0.135 to 20.085 (scaled units, e.g. m³/s)  
- Concentration (C): log(C) = 0.5 – 0.4 × log(Q) + ε, where ε ~ Normal(0, 0.3²)  
- True parameters: α = 0.5, β = -0.4 (moderate dilution), σ = 0.3  

**Preprocessing & transformations:**  
Natural log applied to Q and C to linearize power-law relationship and stabilize variance. No missing values or outliers (clean synthetic data).

**Exploratory findings:**  
- Strong negative correlation: log(Q) vs log(C) = -0.899  
- Visualizations saved in `/figures/` (e.g., log-log scatter showing dilution trend)

## Bayesian Model (Task 2)
**Equation:**  
log(Cᵢ) = α + β log(Qᵢ) + εᵢ, εᵢ ~ Normal(0, σ²)

**Priors (vague, conjugate):**  
- α ~ Normal(0, 100)  
- β ~ Normal(0, 100)  
- σ² ~ InverseGamma(0.001, 0.001)

**Likelihood:** Normal  
**Inference:** Analytical Normal-Inverse-Gamma update (exact posterior, no MCMC)

## Model Fitting & Validation (Task 3)
**Method:** Conjugate posterior update  
**Posterior estimates:**  
- α ≈ 0.519 [95% CI: 0.478, 0.560]  
- β ≈ -0.402 [95% CI: -0.430, -0.374]  
- σ ≈ 0.289  

**Fit quality:** R² ≈ 0.81 (strong), narrow credible intervals (low uncertainty)  
**Validation:** Posterior predictive checks (simulated data overlays observed scatter well)  
**Prior sensitivity:** Vague priors ensure data dominance

## Interpretation of C–Q Relationships (Task 4)
**Slope:** β ≈ -0.402 (entire CI negative) → **clear dilution**  
**Magnitude:** ~60% decrease in concentration per 10× increase in discharge  
**Behavior:** Dilution (not enrichment or chemostasis)  
**Mechanism:** Constant phosphorus loading (e.g., point sources/baseflow) diluted by low-P high-flow water  
**Uncertainty:** Narrow credible interval → high confidence in dilution diagnosis

## Deliverables
- `notebooks/main_analysis.ipynb`: Full code (data generation, exploration, model, fit, plots)  
- `figures/`: Saved visualizations (e.g., `cq_scatter_plots.png`)  
- `AQUA005_Report_Tejas.pdf`: Scientific Reports-style manuscript  
- Presentation recording (MP4): 8–10 min narrated slides

## How to Run
1. Open `notebooks/main_analysis.ipynb` in Jupyter Notebook, JupyterLab, or Google Colab  
2. Run all cells (data generated in memory – no external files required)  
3. Plots automatically save to `/figures/` folder  
4. Review results: posterior estimates, plots, interpretation

## Limitations & Future Work
- Synthetic data only (no real seasonality, hysteresis, or multi-regime shifts)  
- Single linear model assumed  
- Future: Apply to real USGS data (Vermillion River), add regime-specific or seasonal parameters, use MCMC for flexibility

This repository contains the complete submission for the AQUA005 task:  
- main_analysis.ipynb (code & results)  
- AQUA005_Report_Tejas.pdf (manuscript)  
- Presentation recording (MP4)

Prepared for AQUA005 Mini Research Task.
