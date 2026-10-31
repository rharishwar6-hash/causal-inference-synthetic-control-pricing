# Week 3 – Project Progress Report

## Project: Causal Inference & Synthetic Control for Algorithmic Pricing

### 1. Overview

During Week 3, the project focused on evaluating the reliability of causal inference methods and estimating the impact of algorithmic pricing on revenue.

The main activities included Synthetic Control robustness testing and adjusted Double Machine Learning analysis.

### 2. Work Completed

#### 2.1 Synthetic Control – Robustness Testing

**Notebook:** `06_scm_robustness.ipynb`

Activities performed:
- Loaded the daily-wide revenue dataset.
- Separated pre-treatment and post-treatment periods.
- Considered North as the treated region and South and West as donor regions.
- Tested different donor-region weight combinations.
- Compared pre-treatment Mean Squared Error (MSE).
- Calculated average and cumulative post-treatment gaps.
- Saved baseline SCM results.

**Baseline Results**

| Metric | Value |
|---|---:|
| South Weight | 0.50 |
| West Weight | 0.50 |
| Pre-treatment MSE | 36,627,510.23 |
| Average Post-treatment Gap | 2,537.58 |
| Cumulative Post-treatment Gap | 152,254.85 |

**Donor Weight Sensitivity Analysis**

| South Weight | West Weight | Pre-treatment MSE |
|---:|---:|---:|
| 0.00 | 1.00 | 80,092,516.26 |
| 0.25 | 0.75 | 48,371,671.90 |
| 0.50 | 0.50 | 36,627,510.23 |
| 0.75 | 0.25 | 44,860,031.26 |
| 1.00 | 0.00 | 73,069,234.99 |

**Observation:** The 50% South and 50% West combination achieved the lowest pre-treatment MSE among the tested weight combinations.

#### 2.2 Double Machine Learning (DML) Analysis

**Notebook:** `05_double_ml.ipynb`

Activities performed:
- Loaded and verified the cleaned dataset.
- Checked dataset dimensions and treatment distribution.
- Converted the date column into datetime format.
- Created a time-trend variable.
- Prepared outcome, treatment, and control variables.
- Created region dummy variables.
- Added region and time controls.
- Fitted the adjusted LinearDML model.
- Estimated the treatment effect for a price change from 500 to 510.
- Calculated the 95% confidence interval.
- Exported the adjusted DML results into a CSV file.

**Model Specification**

| Component | Variables |
|---|---|
| Outcome (Y) | revenue |
| Treatment (T) | our_price |
| Control Variables | competitor_price, marketing_spend, promotion_active |
| Additional Controls | region_South, region_West, time_trend |
| Estimator | LinearDML |
| Nuisance Models | RandomForestRegressor |

**Adjusted DML Results**

| Metric | Value |
|---|---:|
| Price Change | 500 → 510 |
| Estimated Treatment Effect | 1,953.85 |
| 95% CI – Lower | 296.03 |
| 95% CI – Upper | 3,611.66 |

**Interpretation:**

The adjusted DML model estimated an average revenue increase of approximately 1,953.85 units for a price increase from 500 to 510.

The 95% confidence interval ranges from 296.03 to 3,611.66. Since the interval does not include zero, the estimated effect is positive under this model specification.

These results are based on synthetic data and do not guarantee real-world business outcomes.

### 3. Files Created and Updated

| File Name | Description |
|---|---|
| `05_double_ml.ipynb` | Adjusted Double Machine Learning analysis |
| `06_scm_robustness.ipynb` | Synthetic Control robustness testing |
| `dml_adjusted_results.csv` | Adjusted DML treatment-effect results |

### 4. Pending Activities

- Compare previous and adjusted DML model results.
- Perform additional DML validation and stability checks.
- Complete remaining robustness testing.
- Finalize project documentation.
- Update and verify the GitHub repository.

### 5. Conclusion

During Week 3, Synthetic Control robustness testing was performed using different donor-region weight combinations. The adjusted Double Machine Learning model was also fitted to estimate the impact of price changes on revenue.

The analysis produced baseline SCM results, an adjusted DML treatment-effect estimate, and its confidence interval. The corresponding DML results were saved for further analysis.

The remaining activities will focus on additional validation, comparison of model results, documentation, and final project integration.
