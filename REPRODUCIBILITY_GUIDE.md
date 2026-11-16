# Reproducibility Guide

## 1. Project Overview

This project explores causal inference techniques to estimate the impact of an algorithmic pricing intervention on regional revenue.

The analysis uses a synthetic dataset containing daily observations across three regions: North, South and West.

- **Treated region:** North
- **Control regions:** South and West
- **Intervention:** Approximately 7% price increase
- **Intervention date:** May 1, 2025
- **Outcome variable:** Revenue

The project primarily explores the Synthetic Control Method (SCM), robustness testing and Double Machine Learning (DML).

## 2. Dataset

Dataset: `project1_algorithmic_pricing_synthetic_dataset.csv`

The dataset contains:
- 540 observations
- 11 columns
- Three geographical regions
- Daily observations from January 1 to June 29, 2025

The dataset is synthetic and is intended for methodological analysis rather than real-world commercial conclusions.

## 3. Technology Stack

The project uses the following technologies:

- Python for data processing and statistical analysis.
- Pandas and NumPy for data manipulation.
- SciPy for numerical optimisation.
- Scikit-learn for machine learning and exploratory DML implementation.
- Matplotlib for analytical visualisations.
- Power BI for interactive business reporting.

## 4. Synthetic Control Methodology

The Synthetic Control Method constructs a weighted combination of control regions to estimate the counterfactual revenue of the treated region.

The implementation involves:

1. Loading and preparing the dataset.
2. Aggregating revenue by date and region.
3. Separating pre-intervention and post-intervention periods.
4. Estimating donor weights using pre-intervention revenue patterns.
5. Constructing synthetic North using the weighted control regions.
6. Comparing actual North revenue with synthetic North.
7. Calculating the revenue gap and visualising the results.

The weekly donor weights obtained in the analysis were:

| Donor Region | Weight |
|---|---:|
| South | 47.17% |
| West | 52.83% |

## 5. Robustness and Validation

The project investigates model reliability through:

- Daily versus weekly aggregation comparisons.
- Donor sensitivity analysis.
- Placebo intervention tests.
- Pre-treatment Normalised Root Mean Squared Error (NRMSE).

These checks help identify sensitivity to modelling choices and limitations in the estimated counterfactual.

## 6. Double Machine Learning Exploration

The project also explores Double Machine Learning using revenue as the outcome and the pricing intervention as the treatment.

Candidate control variables include competitor prices, marketing expenditure, promotional activity, regional information and calendar features.

The validation identified a lack of common support between treated and control observations.

Consequently, the DML implementation is exploratory and does not establish a reliable causal treatment effect with the available dataset.

## 7. Power BI Dashboard

The Power BI dashboard communicates the project's analytical findings through six pages:

1. Business Overview
2. Regional Revenue Analysis
3. Descriptive Business Analysis
4. Causal Impact Analysis
5. Model Validation and Robustness
6. Key Observations and Business Implications

The dashboard presents the model outputs alongside their methodological limitations.

## 8. Important Limitations

- The dataset is synthetic.
- Only one region receives the pricing intervention.
- Estimated SCM gaps are sensitive to donor selection.
- Placebo tests use limited pre-intervention fitting windows.
- DML validation identified insufficient common support.
- The estimated revenue gap should not be interpreted as proven causal revenue growth.

## 9. Reproducibility Notes

To reproduce the analysis:

1. Clone or download the project repository.
2. Locate the dataset and relevant analysis notebooks.
3. Install the Python libraries required by each notebook.
4. Follow the notebook execution order documented in the repository.
5. Execute the analysis and validation steps.
6. Compare the generated outputs with the documented findings.

Refer to the individual notebooks for implementation details, calculations and supporting visualisations.

