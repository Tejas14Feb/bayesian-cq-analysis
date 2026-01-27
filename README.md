# AQUA005 – Bayesian Concentration–Discharge (C–Q) Analysis for Phosphorus

**Author:** Tejas  
**Date:** January 2026  
**GitHub Repository:** [link to your repo]

## Task Overview
This repository contains the complete submission for the AQUA005 Mini Research Task:  
Bayesian regression analysis of phosphorus concentration (C) vs. stream discharge (Q) relationships.

## Data (Task 1)
**Source:** Synthetic dataset (n = 200 observations)  
**Reason for synthetic data:** Limited time and insufficient paired discrete phosphorus + continuous discharge samples from USGS site 06479010 (Vermillion River near Vermillion, SD) made real-data merging impractical for this deadline. Synthetic data is generated to simulate realistic dilution-dominated behavior typical in many Midwestern U.S. tributaries with point-source or baseflow phosphorus inputs.

**Simulation assumptions (fully reproducible):**
- Discharge (Q): log(Q) ~ Uniform(-2, 3) → Q ranges from ≈0.135 to ≈20.085 (arbitrary scaled units, e.g. m³/s)
- Concentration (C): log(C) = 0.5 + (-0.4) × log(Q) + ε, where ε ~ Normal(0, 0.3²)
- True generating parameters: α = 0.5, β = -0.4 (moderate dilution), σ = 0.3

**Temporal coverage and resolution:** Not applicable (synthetic, no real time stamps). Equivalent to daily samples over ~several years for demonstration.

**Preprocessing & transformations:**
- Natural log transformation applied to both Q and C → stabilizes variance and linearizes the expected power-law C–Q relationship (C ∝ Q^β)
- No missing values or outliers removed (clean synthetic data)
- No units conversion needed

**Exploratory findings:**
- Strong negative correlation between log(Q) and log(C): r ≈ -0.899
- Scatter plots show clear power-law dilution pattern (decreasing C with increasing Q)

## Bayesian Model (Task 2)
**Model equation:**
log(Cᵢ) = α + β × log(Qᵢ) + εᵢ  
εᵢ ~ Normal(0, σ²)   (independent errors)

**Likelihood:** Normal (appropriate for continuous concentration data after log transform)

**Priors (vague, weakly informative):**
- α ~ Normal(0, 100)  
- β ~ Normal(0, 100)  
- σ² ~ InvGamma(0.001, 0.001)  (very diffuse)

**Why these priors?** Large variance ensures data dominate inference while maintaining conjugate form for analytical solution.

**Model choice rationale:** Log-linear form is standard for C–Q relationships (power-law behavior common in hydrology). No extensions (regime-specific or seasonal parameters) implemented due to time constraint and single synthetic regime.

## Model Fitting & Validation (Task 3)
**Method:** Analytical posterior via conjugate Normal-Inverse-Gamma update (no MCMC needed — exact & fast)

**Posterior estimates (mean and approximate 95% credible intervals):**
- α (baseline log-concentration) ≈ 0.519 [0.478, 0.560]  
- β (C–Q slope) ≈ -0.402 [-0.430, -0.374]  
- σ (residual standard deviation) ≈ 0.289

**Model quality:**
- Parameter uncertainty: Narrow credible intervals → strong data signal, low posterior uncertainty
- Model fit: R² ≈ 0.81 (high explanatory power on log scale)
- Convergence: Not applicable (analytical solution)
- Posterior predictive check: Simulated data from posterior closely matches observed scatter (visual check in notebook)
- Prior sensitivity: Vague priors → posterior very close to maximum likelihood (data-driven). Tighter priors would shrink β toward 0, but not realistic here.

## Interpretation of C–Q Relationships (Task 4)
**Slope interpretation:**
- β ≈ -0.402 (entire 95% CI negative) → **clear dilution behavior**
- For every 10-fold increase in discharge, phosphorus concentration decreases by a factor of ≈0.40 (≈60% reduction)

**Behavior classification:**
- Dilution (β < 0): Dominant pattern
- Not enrichment (β > 0)
- Not chemostatic (β ≈ 0)

**Transport mechanism insights:**
- Dilution suggests relatively constant phosphorus loading (e.g., point sources such as wastewater treatment plants, tile drainage, or stable groundwater/baseflow) diluted by low-phosphorus water during high-flow events.
- Limited evidence of surface runoff or erosion mobilizing additional P during storms.
- Consistent with patterns observed in some agricultural/urban-influenced Midwestern rivers.

**Uncertainty influence:**
- Narrow credible intervals around β provide high confidence in dilution diagnosis.
- In real, sparser data, wider intervals might make it harder to confidently distinguish dilution from weak chemostasis or mixed behavior.

## Limitations & Future Directions
- Synthetic data lacks real-world complexity (seasonality, hysteresis, multiple flow regimes, non-stationarity)
- Single linear model assumed; real systems often require piecewise or GAM-style approaches
- No hierarchical modeling (single “site”)
- Future work: Use real USGS/NWIS data from Vermillion River or similar sites, incorporate seasonal effects, add flow-regime splits, or use MCMC (PyMC/Stan) for non-conjugate extensions

**Notebook location:** `notebooks/main_analysis.ipynb` (full code, plots, and calculations)
