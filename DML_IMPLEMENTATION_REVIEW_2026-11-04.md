# Exploratory DML Implementation Review — 4 November 2026

**Project:** Causal Inference & Synthetic Control Engine for Algorithmic Pricing  
**Dataset:** `project1_algorithmic_pricing_synthetic_dataset.csv`  
**Notebook:** `04_double_machine_learning_exploratory.ipynb`

## 1. Work completed

A reproducible, executed notebook was prepared implementing a cross-fitted partialling-out Double Machine Learning (DML) workflow. The notebook:

- Loads and validates the 540-row, 11-column synthetic dataset.
- Verifies the supplied treatment flag against the North × post-1-May-2025 definition.
- Uses revenue as outcome (Y) and treatment as D.
- Uses competitor price, marketing spend, promotion status, region and calendar features as candidate X variables.
- Excludes `our_price` and `units_sold` because they may be post-treatment mediators or mechanisms; the total-effect model should not automatically condition on them.
- Uses five-fold cross-fitting grouped by date so all three regions from a date remain in the same fold.
- Compares regularized linear/logistic nuisance models with random-forest nuisance models.
- Reports propensity diagnostics, exploratory residual-on-residual coefficients and a date-cluster standard-error diagnostic.

All notebook code cells executed successfully against the project CSV.

## 2. Dataset and treatment assignment audit

- 540 rows; 180 daily dates from 1 January through 29 June 2025.
- Three regions: North, South and West; three rows per date.
- No missing values in the supplied columns.
- 60 treated rows and 480 untreated rows.
- The treatment flag exactly matches `region == North` and `date >= 2025-05-01`.
- North is the only treated region. South and West are never treated.

This assignment rule creates a structural positivity/overlap problem. When region and time are included in the nuisance models, treatment status is nearly or perfectly separated from untreated observations.

## 3. Exploratory model results

The following are **diagnostic model outputs only**, not validated causal effects:

| Nuisance specification | DML coefficient (revenue units) | Date-cluster SE diagnostic | Propensity AUC | Share of scores in 0.05–0.95 |
|---|---:|---:|---:|---:|
| Ridge + logistic regression | 875.48 | 1,054.56 | 1.000 | 45.6% |
| Random forest + random forest | 10,001.95 | 4,813.27 | 1.000 | 0.9% |

More decisive than the central estimates is the common-support diagnostic:

- Ridge/logistic: treated propensity range [0.2959, 0.7870]; untreated range [0.0009, 0.2790]. These ranges do not overlap.
- Random forest: treated range [0.4898, 1.0000]; untreated range [0.0000, 0.0887]. These ranges do not overlap.

The propensity AUC of 1.000 under both specifications indicates perfect ranking/separation in this sample. The absence of common support means the model is extrapolating rather than comparing treated and untreated observations with similar estimated treatment probabilities.

The large difference between the two DML coefficients (about 875 versus 10,002 revenue units) is further evidence that the estimate is highly specification-sensitive. The reported standard-error diagnostic and intervals should not be interpreted as valid causal uncertainty under this design.

## 4. Why no causal ATE/CATE is claimed

A defensible DML ATE requires, among other assumptions, conditional exchangeability and overlap. The current dataset cannot provide treated/untreated comparisons across comparable region-time covariate profiles: all treated observations are from one region after one date, while the two donor regions are never treated. Repeated daily rows do not create additional independently treated markets.

Cross-fitting improves nuisance estimation discipline but cannot repair deterministic treatment assignment, absent common support, or unobserved region-specific shocks. For the same reason, a CATE analysis is not justified by this dataset.

The candidate controls also need causal-timing confirmation. Marketing spend and promotion status may themselves respond to the pricing intervention. They should only be treated as confounders if the data-generation process confirms they are measured before treatment or are otherwise unaffected by it. `our_price` is excluded because it is the intervention mechanism; `units_sold` may also be downstream of pricing.

## 5. Recommended next steps

1. Ask the project/data owner for the synthetic data-generation logic and confirm when marketing spend and promotion status are set relative to treatment.
2. If possible, obtain a dataset with multiple treated and untreated regions/markets and treatment variation within comparable covariate profiles.
3. Define the target estimand and valid pre-treatment covariates before rerunning DML.
4. Use inference that respects market-level clustering and the treatment-assignment design; do not rely on row-level standard errors.
5. Keep the DML notebook as an exploratory implementation and report the overlap failure transparently.
6. Do not present the DML coefficients as evidence that the pricing intervention caused a revenue increase.

## 6. Suggested project wording

> We implemented a cross-fitted partialling-out DML workflow and evaluated two nuisance-model specifications. The treatment propensity models perfectly separated treated from untreated observations, and the estimated propensity ranges had no common support. The resulting DML coefficients were highly specification-sensitive. Because the dataset contains only one treated region and treatment is determined by the North-by-post-period rule, the current data do not support a defensible causal ATE or CATE. We therefore report DML as an exploratory methodological implementation and identify additional market-level treatment variation as a requirement for causal estimation.
