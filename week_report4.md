# Weekly Project Progress Report
## Project: Causal Inference & Synthetic Control for Algorithmic Pricing

**Overall Status:** Completed

## 1. Executive Summary

This week, the team completed the assigned tasks related to data preparation, Synthetic Control robustness analysis, Double Machine Learning (DML), Power BI dashboard development, and final project validation.

All five team members completed their respective responsibilities. The team finalized the analytical outputs, model comparisons, dashboard visuals, documentation, and GitHub updates.

## 2. Team-wise Work Summary

### Member 1 – Data Preparation & Documentation

**Status: Completed**

- Completed data preparation and organization.
- Created `daily_wide_revenue.csv`.
- Created `weekly_wide_revenue.csv`.
- Validated treatment and control regions.
- Verified pre-treatment and post-treatment periods.
- Prepared the required data for causal analysis.

**Deliverables:**
- `02_model_data_prep.ipynb`
- `daily_wide_revenue.csv`
- `weekly_wide_revenue.csv`

### Member 2 – Synthetic Control Robustness Analysis

**Status: Completed**

- Completed `06_scm_robustness.ipynb`.
- Estimated donor weights for South and West.
- Constructed Synthetic North revenue.
- Calculated pre-treatment model error.
- Measured post-treatment revenue gaps.
- Performed donor weight sensitivity analysis.
- Completed alternative donor tests.

**Key Results:**

| Metric | Result |
|---|---:|
| South Donor Weight | 50% |
| West Donor Weight | 50% |
| Pre-treatment MSE | 36,627,510.23 |
| Average Post-treatment Gap | 2,537.58 |
| Cumulative Post-treatment Gap | 152,254.85 |

**Interpretation:**

The Synthetic Control model used an equal combination of South and West to construct the counterfactual revenue for North. Actual North revenue was higher than the synthetic estimate during the post-treatment period.

### Member 3 – DML & Model Validation

**Status: Completed**

- Completed adjusted DML model fitting.
- Performed sensitivity testing.
- Exported residual diagnostic results to CSV.
- Reviewed residual diagnostic plots.
- Saved `scm_vs_dml_comparison.csv`.
- Documented previous DML vs adjusted DML comparisons.
- Completed model interpretation and limitations.
- Verified GitHub commits and push.

**Deliverables:**
- Adjusted DML model results.
- Residual diagnostic CSV and plots.
- `scm_vs_dml_comparison.csv`.
- Model comparison documentation.

### Member 4 – Power BI Dashboard Development

**Status: Completed**

- Completed the Executive Overview page.
- Developed the remaining dashboard pages.
- Created causal impact visualizations.
- Added pricing and demand analysis.
- Added regional performance analysis.
- Integrated causal analysis results.
- Verified dashboard metrics against CSV outputs.

**Dashboard Pages:**

1. Executive Overview
2. Pricing & Demand Analysis
3. Regional Performance Analysis
4. Causal Impact Analysis
5. Model Comparison

### Member 5 – Final Validation & Documentation

**Status: Completed**

- Completed final MSPE validation.
- Completed placebo validation.
- Prepared the final project report.
- Updated the project README.
- Reviewed the GitHub repository.
- Verified project outputs and documentation.

## 3. Dataset Overview

The project uses a synthetic/simulated teaching dataset, not real company data.

| Description | Details |
|---|---|
| Total Observations | 540 |
| Total Regions | 3 |
| Regions | North, South, West |
| Observations per Region | 180 |
| Date Range | January 1 – June 29, 2025 |
| Intervention Date | May 1, 2025 |
| Treated Region | North |
| Control / Donor Regions | South and West |

## 4. Causal Inference Methods

The project implemented the following methods:

- **Synthetic Control Method (SCM):** Constructed a synthetic comparison region using donor-region revenue.
- **Double Machine Learning (DML):** Estimated treatment effects while adjusting for observed variables.
- **Difference-in-Differences (DiD):** Compared changes in the treated region against changes in control regions.
- **MSPE and Placebo Validation:** Evaluated pre-treatment fit and tested the robustness of the estimated intervention effect.

## 5. Final Deliverables

- Data preparation and validation notebooks.
- Exploratory Data Analysis (EDA).
- Causal DAG documentation.
- Synthetic Control analysis.
- SCM robustness and donor sensitivity analysis.
- MSPE and placebo validation.
- DML model and residual diagnostics.
- SCM vs DML comparison.
- Difference-in-Differences analysis.
- Power BI interactive dashboard.
- Final project report and updated README.

## 6. Conclusion

All assigned team tasks have been completed. The project successfully combines data preparation, causal inference, model validation, robustness testing, and Power BI visualization to evaluate the estimated impact of an algorithmic pricing intervention.

The completed analysis and dashboard provide a structured view of business performance and estimated causal effects.

**Final Project Status: Completed**

**Note:** All results are based on simulated data. Estimated treatment effects should be interpreted according to the assumptions and limitations of each causal inference method.
