# DML Validation Audit — 5 November 2026

**Project:** Causal Inference & Synthetic Control Engine for Algorithmic Pricing  
**Dataset:** `project1_algorithmic_pricing_synthetic_dataset.csv`  
**Audited notebook:** `04_double_machine_learning_exploratory.ipynb`  
**Validation notebook:** `04b_dml_validation_audit.ipynb`

## Executive conclusion

The exploratory DML notebook executes, and its date-grouped folds keep the three regional rows from the same date together. However, the current `GroupKFold` design is not chronological: each test fold contains dates distributed across the full study period, while its training set includes later dates than most test observations. This allows future-period information to enter nuisance-model training when predicting earlier dates.

A strictly forward-chaining check reveals a deeper data-design limitation: all observations before 1 May 2025 are untreated. In the first four expanding-window folds, the training sample contains no treated observations, so a treatment propensity model cannot be trained on both classes. In the fifth fold, only 30 treated rows are available in the training window, and the treated market is still only North.

**The current data therefore do not support a defensible DML ATE or CATE.** This is not fixed by changing the cross-validation splitter or clipping propensity scores.

## 1. What was checked

- Re-read the DML notebook from the `Dhananjay` branch and inspected the executed code.
- Confirmed the notebook uses `GroupKFold(n_splits=5)` with `date` as the grouping variable.
- Reproduced the grouped fold assignments and checked whether training contains dates later than test dates.
- Evaluated five expanding-window `TimeSeriesSplit` folds over unique dates.
- Reconfirmed treatment assignment: `treatment = 1` exactly for North on/after 1 May 2025.
- Reviewed the previously reported propensity ranges and nuisance-model sensitivity.

## 2. GroupKFold audit

The existing splitter does keep all three regional observations from a date in the same fold. But it distributes test dates across the whole period rather than holding out a future contiguous block.

| Fold | Test date range | Train dates | Test dates | Treated rows in train | Test dates with later dates in training |
|---:|---|---:|---:|---:|---:|
| 1 | 5 Jan–29 Jun | 144 | 36 | 48 | 35 |
| 2 | 4 Jan–28 Jun | 144 | 36 | 48 | 36 |
| 3 | 3 Jan–27 Jun | 144 | 36 | 48 | 36 |
| 4 | 2 Jan–26 Jun | 144 | 36 | 48 | 36 |
| 5 | 1 Jan–25 Jun | 144 | 36 | 48 | 36 |

For nearly every test date, the training set includes later dates. The folds are grouped by date but are not time-respecting. This matters because treatment begins at a fixed date and revenue/covariates may have temporal trends.

## 3. Forward-chaining feasibility

A five-fold expanding-window split on the 180 unique dates produces:

| Fold | Training end | Test start | Treated rows in training | Treated rows in test | Both classes in training? |
|---:|---|---|---:|---:|---|
| 1 | 30 Jan | 31 Jan | 0 | 0 | No |
| 2 | 1 Mar | 2 Mar | 0 | 0 | No |
| 3 | 31 Mar | 1 Apr | 0 | 0 | No |
| 4 | 30 Apr | 1 May | 0 | 30 | No |
| 5 | 30 May | 31 May | 30 | 30 | Yes |

Only one of five training windows contains both treatment classes. A standard propensity classifier cannot be meaningfully trained in the first four folds. This means simply switching the DML workflow to forward-chaining cross-fitting is not a viable fix with the current assignment design.

## 4. Propensity overlap remains absent

The existing DML notebook reported:

| Nuisance specification | Treated propensity range | Untreated propensity range | Common-support interval |
|---|---:|---:|---|
| Ridge + logistic | 0.2959–0.7870 | 0.0009–0.2790 | None |
| Random forest | 0.4898–1.0000 | 0.0000–0.0887 | None |

The reported propensity AUC is 1.000 for both specifications. The ranges do not intersect. The fact that some individual predictions fall between 0.05 and 0.95 does **not** establish common support: treated and untreated scores must overlap with one another.

Clipping propensity predictions can prevent numerical extremes in a calculation, but it cannot create observations with comparable treatment probabilities.

The two previously reported residual-on-residual coefficients were 875.48 and 10,001.95 revenue units. Their large difference is a warning about specification sensitivity; neither should be presented as a validated causal effect. The previously calculated date-cluster standard-error diagnostics are not valid causal uncertainty measures under this assignment design.

## 5. Covariate timing

The DML notebook uses competitor price, marketing spend, promotion status, region and calendar features as candidate controls, while excluding `our_price` and `units_sold`.

The exclusion is sensible as a precaution for a total-effect estimand: own price is the intervention mechanism, and units sold may be downstream of price. However, the available project materials do not establish whether `marketing_spend` and `promotion_active` were set before treatment or may themselves respond to the pricing intervention. Their causal timing must be confirmed from the data-generation process before treating them as confounders. Competitor-price timing should also be documented.

## 6. Recommended project decision

1. Keep `04_double_machine_learning_exploratory.ipynb` as an exploratory implementation, but disclose the non-chronological grouped folds.
2. Do not report either DML coefficient as a causal ATE and do not produce a CATE claim from this dataset.
3. Do not substitute forward-chaining DML estimates: four of five expanding training windows contain no treated examples.
4. Request the data-generation logic and confirm covariate timing.
5. For a defensible DML analysis, request multiple treated and untreated markets, with treatment variation within comparable market-time profiles and sufficient overlap.
6. Continue to report Synthetic Control as a descriptive counterfactual analysis, including donor sensitivity and its own limitations. DML does not independently validate the Synthetic Control estimate here.

## Suggested review wording

> We implemented an exploratory cross-fitted partialling-out DML workflow and audited its validation design. Date-grouped folds prevented same-date regional rows from being split across train and test, but they were not chronological and allowed later dates into training for most test observations. A forward-chaining audit showed that four of five training windows contained no treated observations, preventing a usable propensity model in those folds. The propensity ranges from the existing DML specifications also showed no common support. We therefore do not claim a validated DML ATE or CATE from the current single-treated-region dataset; additional treated-market variation is required.
