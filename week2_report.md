# Weekly Progress Report — Week 2

**Project:** Causal Inference & Synthetic Control for Algorithmic Pricing  
**Reporting Period:** October 19–24, 2026  
**Project Type:** Data Analytics — Causal Inference

## 1. Work Completed

### A. Data Preparation and Documentation
- Created daily wide-format revenue data with one row per date and separate revenue columns for North, South, and West.
- Created weekly wide-format revenue data.
- Validated the generated datasets for missing values and duplicate dates.
- Updated `data_dictionary.md` with dataset details and column descriptions.
- Updated `README.md` with dataset source, structure, and intervention information.

### B. Data Validation
- Validated the original dataset containing 540 rows and 11 columns.
- Verified the 180 unique dates and the three regions: North, South, and West.
- Checked treatment assignment and pre-/post-intervention periods.
- Confirmed that North is the treated region and South and West are donor/control regions.
- Verified the intervention date: May 1, 2025.
- Completed 8 out of 8 recorded Day 4 validation checks successfully.
- Confirmed that daily and weekly regional revenue totals matched.

### C. Double Machine Learning (DML)
- Implemented the preliminary `LinearDML` model using EconML.
- Used `our_price` as the treatment and `revenue` as the outcome.
- Included competitor price, marketing spend, and promotion activity as model features.
- Built an adjusted model with region indicators and a time trend.
- Evaluated outcome-model and treatment-model predictive performance.
- Saved preliminary estimates, model diagnostics, and the original-versus-adjusted comparison.

## 2. Key Results

### Data Preparation

| Dataset | Shape | Validation |
|---|---|---|
| Original dataset | 540 × 11 | 8/8 recorded Day 4 checks passed |
| Daily wide revenue | 180 × 4 | No missing values or duplicate dates |
| Weekly wide revenue | 26 × 4 | Regional totals matched daily totals |

### DML Model Comparison

| Model | Price Change | Estimated Revenue Effect | 95% Confidence Interval |
|---|---:|---:|---:|
| Original DML | 10 | 1,722.97 | 1,216.89–2,229.06 |
| Adjusted DML | 10 | 3,199.03 | 1,077.04–5,321.02 |

The adjusted estimate changed after adding region and time controls, and its confidence interval was wider. This indicates sensitivity to model specification.

**Important:** These are preliminary estimates from a synthetic dataset. They do not establish a confirmed causal effect of the North-region intervention.

## 3. Files Created or Updated

- `daily_wide_revenue.csv`
- `weekly_wide_revenue.csv`
- `data_dictionary.md`
- `README.md`
- `05_double_ml.ipynb`
- `dml_preliminary_results.csv`
- `dml_model_diagnostics.csv`
- `dml_original_vs_adjusted_comparison.csv`

## 4. Challenges and Limitations

- Pre-treatment revenue trends differed across regions, particularly for West.
- The DML estimate changed when region and time controls were added.
- The continuous price effect is different from the causal effect of the specific North-region intervention.
- Further causal-assumption checks and robustness tests are needed before interpreting the estimates causally.

## 5. Pending Work

- Complete additional DML causal-assumption and robustness checks.
- Review treatment variation and covariate overlap.
- Perform suitable placebo or alternative-specification checks.
- Confirm that all completed notebooks, CSV outputs, and documentation are committed and pushed to the correct GitHub branch.
- Coordinate with team members to consolidate DiD, Synthetic Control, and DML findings in the final comparison.

## 6. Conclusion

During Week 2, data preparation, documentation, validation, and preliminary Double Machine Learning analysis progressed. The required data outputs and model-comparison files were generated. Further validation is still needed before making causal claims about the simulated pricing intervention.
