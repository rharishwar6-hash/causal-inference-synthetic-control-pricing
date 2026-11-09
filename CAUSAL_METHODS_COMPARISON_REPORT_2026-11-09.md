# Consolidated Causal Analysis: Synthetic Control vs Double Machine Learning

**Project:** Causal Inference & Synthetic Control Engine for Algorithmic
Pricing\
**Review date:** 9 November 2026\
**Repository branch reviewed:** `Dhananjay`\
**Dataset:** `project1_algorithmic_pricing_synthetic_dataset.csv`

## 1. Executive summary

The project contains two analytical workstreams: Synthetic Control (SC),
which constructs a weighted combination of South and West as a
descriptive counterfactual for North, and exploratory Double Machine
Learning (DML), which implements a cross-fitted partialling-out workflow
using revenue as the outcome.

The two methods do **not** independently confirm a causal pricing
effect. Synthetic Control produces positive average post-period gaps
under its primary two-donor specification, but the result changes
materially under donor choice. DML executes, but its propensity
diagnostics show no common support and its grouped cross-fitting is not
chronological. A forward-chaining audit finds that only one of five
training windows contains both treatment classes.

**Overall conclusion:** the current synthetic dataset supports a
methodological demonstration and descriptive counterfactual analysis. It
does not support a defensible causal ATE or CATE claim. No causal lift
should be claimed from the current results.

## 2. Dataset and treatment design

  Attribute                Description
  ------------------------ ---------------------------
  Observations             540
  Variables                11
  Regions                  North, South, West
  Daily date range         1 January--29 June 2025
  Unique dates             180
  Treated region           North
  Intervention date        1 May 2025
  Treated observations     60
  Untreated observations   480
  Treatment rule           North on/after 1 May 2025

The treatment indicator exactly matches `region == North` and
`date >= 2025-05-01`. North is the only region that ever receives
treatment; South and West are never treated. This is the central design
limitation for both methods.

The dataset is synthetic. It is not a set of real company transactions,
and results are not evidence of real-world pricing performance.

## 3. Synthetic Control findings

### 3.1 Primary estimates

The updated robustness notebook excludes the mixed week ending 4 May
from the weekly post-treatment calculation. Daily analysis uses the
exact 1 May intervention date.

  Diagnostic                                   Result
  -------------------------------------- ------------
  Weekly mean post-period revenue gap      **+3.05%**
  Daily mean post-period revenue gap       **+4.40%**
  Weekly pre-treatment normalized RMSE      **3.98%**
  Daily pre-treatment normalized RMSE       **9.76%**
  Weekly South donor weight                **47.17%**
  Weekly West donor weight                 **52.83%**

The gap is observed North revenue minus synthetic North revenue. A
positive gap means North's observed average revenue is above the
weighted donor estimate during the selected post-period. It is not, by
itself, proof that the intervention caused the difference.

### 3.2 Donor sensitivity

  Weekly donor specification     Estimated post-period gap
  ---------------------------- ---------------------------
  South + West                                  **+3.05%**
  South only                                   **+14.49%**
  West only                                     **−5.40%**

The single-donor estimates have opposite signs. In the pre-treatment
period, South's weekly mean revenue is about 9.40% below North, while
West's is about 7.39% above North. West has a slightly lower weekly
standalone normalized RMSE (9.24% versus 10.16%); South has a higher
weekly correlation with North (0.793 versus 0.736). Neither donor is
clearly superior across all diagnostics.

The weighted combination improves pre-treatment fit relative to either
standalone donor, but better in-sample fit does not establish that the
weighted series is the true post-treatment counterfactual. The donor
sensitivity is a material limitation and must remain visible in the
final presentation.

### 3.3 Other robustness diagnostics

Placebo intervention dates of 1 February, 1 March and 1 April produced
weekly gaps of approximately −1.29%, −1.77% and −2.51%. The respective
pre-periods used to fit weights contain only 3, 7 and 12 weekly
observations. These are short descriptive checks, not formal statistical
inference.

Daily pre-treatment fit is weaker than weekly fit (normalized RMSE 9.76%
versus 3.98%). The daily and weekly estimates also use different
aggregation and fitting samples, so their difference should not be
interpreted as a precise timing effect.

### 3.4 Synthetic Control interpretation

**What it supports:** a transparent descriptive comparison of North
against a weighted combination of two donor regions, including
pre-period fit and sensitivity diagnostics.

**What it does not establish:** a stable or statistically validated
causal effect. There are only two donor regions; donor choice changes
the estimate materially; and a good pre-treatment fit does not guarantee
the untreated post-period trajectory.

## 4. Double Machine Learning findings

### 4.1 Implementation

The exploratory notebook defines: - Outcome (Y): `revenue` - Treatment
(D): `treatment` - Candidate controls: `competitor_price`,
`marketing_spend`, `promotion_active`, region and calendar features -
Excluded variables: `our_price` and `units_sold`, because they may be
the intervention mechanism or downstream mediators for a total-effect
estimand

The notebook implements cross-fitted partialling-out DML and compares
regularized linear/logistic nuisance models with random-forest nuisance
models. The implementation executes, but successful execution is not
evidence that the identifying assumptions hold.

The causal timing of `marketing_spend`, `promotion_active` and
competitor price has not been established from the available
data-generation documentation. They should not automatically be treated
as valid pre-treatment confounders.

### 4.2 Exploratory model outputs

  ----------------------------------------------------------------------------
  Nuisance          Residual-on-residual     Propensity AUC Share of scores in
  specification     coefficient (revenue                            0.05--0.95
                                  units)                    
  --------------- ---------------------- ------------------ ------------------
  Ridge +                         875.48              1.000              45.6%
  logistic                                                  
  regression                                                

  Random forest +              10,001.95              1.000               0.9%
  random forest                                             
  ----------------------------------------------------------------------------

These coefficients are **diagnostic outputs only**. They are not
validated ATE estimates and must not be presented as causal lift.

### 4.3 Propensity overlap

  -------------------------------------------------------------------------
  Specification     Treated propensity Untreated propensity Common support
                                 range                range 
  --------------- -------------------- -------------------- ---------------
  Ridge +               0.2959--0.7870       0.0009--0.2790 None
  logistic                                                  

  Random forest         0.4898--1.0000       0.0000--0.0887 None
  -------------------------------------------------------------------------

Both propensity models have AUC 1.000 and the treated and untreated
score ranges do not overlap. AUC 1.000 is not a success criterion here:
it reflects the fact that treatment assignment can be distinguished
almost perfectly from region and time. The model is extrapolating rather
than comparing observations with similar treatment probabilities.

The fact that some predictions lie between 0.05 and 0.95 does not
demonstrate common support. Treated and untreated score distributions
must overlap with one another.

### 4.4 Cross-fitting audit

The exploratory notebook uses five-fold `GroupKFold` with date as the
grouping variable. This keeps all regional observations from a given
date together, but it is not chronological. In the audit, 35--36 of the
36 test dates in each fold had later dates represented in the training
data.

A separate five-fold expanding-window check found:

  ------------------------------------------------------------------------
              Fold Training end Test start    Treated rows in Both
                                                     training treatment
                                                              classes in
                                                              training?
  ---------------- ------------ ------------ ---------------- ------------
                 1 30 Jan       31 Jan                      0 No

                 2 1 Mar        2 Mar                       0 No

                 3 31 Mar       1 Apr                       0 No

                 4 30 Apr       1 May                       0 No

                 5 30 May       31 May                     30 Yes
  ------------------------------------------------------------------------

Only one of five training windows contains both treatment classes.
Therefore, simply replacing GroupKFold with forward-chaining does not
solve the problem: early training windows cannot fit a propensity model
with both classes.

Cross-fitting can reduce overfitting in nuisance estimation, but it
cannot create treatment variation or repair absent overlap.

### 4.5 DML interpretation

The two exploratory coefficients differ substantially (about 875 versus
10,002 in the dataset's revenue units), reinforcing that the result is
specification-sensitive. The date-cluster standard-error diagnostics
previously calculated should not be treated as valid causal uncertainty
under this assignment design.

The current data do not support a defensible DML ATE or CATE. Repeated
daily observations from one treated region do not create multiple
independently treated markets.

## 5. Side-by-side method comparison

  -----------------------------------------------------------------------
  Dimension               Synthetic Control       Double Machine Learning
  ----------------------- ----------------------- -----------------------
  Main purpose            Construct a weighted    Estimate a treatment
                          untreated               coefficient after
                          counterfactual for      modelling outcome and
                          North                   treatment nuisance
                                                  functions

  Unit/design in this     One treated region, two Daily region
  project                 donor regions           observations; one
                                                  treated region

  Main output             Weekly +3.05%; daily    Exploratory
                          +4.40% descriptive gaps coefficients 875.48 and
                                                  10,001.95

  Key diagnostic          Pre-treatment fit and   Propensity overlap and
                          donor sensitivity       cross-fitting design

  Main warning            Donor choice changes    No common support;
                          estimate and can        treatment is determined
                          reverse its sign        by North × post

  What can be reported    Descriptive             Exploratory
                          synthetic-control gap   implementation and
                          with caveats            diagnostics only

  Causal conclusion       Not established         Not established
  -----------------------------------------------------------------------

### Why the methods do not validate each other

The methods use the same underlying treatment assignment and the same
limited set of regions. Their assumptions are not independently
satisfied merely because both methods have been implemented. Synthetic
Control's donor sensitivity and DML's lack of overlap are separate
warnings arising from the same narrow market design. The DML
coefficients should not be used to confirm the Synthetic Control gap,
and the Synthetic Control gap should not be used to validate DML.

## 6. Consolidated limitations

1.  **Synthetic data:** results are methodological demonstrations, not
    real commercial outcomes.
2.  **Only one treated region:** North is the sole treated market.
3.  **Only two donor regions:** the donor pool is too small for strong
    donor-based inference.
4.  **Donor sensitivity:** single-donor estimates have opposite signs.
5.  **Pre-treatment fit:** daily fit is weaker than weekly fit, and fit
    alone does not validate the counterfactual.
6.  **No DML common support:** treated and untreated propensity-score
    ranges do not overlap under either reported specification.
7.  **Cross-fitting limitation:** grouped folds are not chronological;
    forward-chaining folds mostly lack treated observations in training.
8.  **Covariate timing uncertainty:** the causal timing of marketing
    spend, promotions and competitor price is not documented.
9.  **No validated ATE/CATE:** neither method currently supports a
    defensible causal effect claim.

## 7. Recommendations and data needed

1.  Obtain the synthetic data-generation logic and document exactly how
    treatment, price, revenue, marketing, promotions and competitor
    prices are generated.
2.  Obtain data with multiple treated and untreated markets, ideally
    with treatment variation across markets and time rather than one
    deterministic North-by-post assignment.
3.  Confirm that adjustment variables are measured before treatment and
    are not affected by the intervention.
4.  Predefine the estimand, treatment unit, comparison unit and
    inference strategy before fitting another DML model.
5.  For Synthetic Control, expand the donor pool and assess
    pre-treatment fit and donor sensitivity with more candidate markets.
6.  Keep the current DML results labelled exploratory. Do not claim an
    ATE or CATE until overlap and design requirements are met.
7.  Preserve the daily/weekly Synthetic Control estimates, donor
    sensitivity and placebo limitations in the final report.

## 8. Suggested executive-review wording

> We implemented a synthetic-control analysis and an exploratory
> cross-fitted Double Machine Learning workflow on a simulated regional
> pricing dataset. The primary synthetic-control specification produced
> descriptive post-period revenue gaps of 3.05% weekly and 4.40% daily,
> but the result was materially sensitive to donor choice: South-only
> and West-only estimates were +14.49% and −5.40%, respectively. The DML
> workflow executed, but both propensity specifications showed no common
> support, and a validation audit identified non-chronological grouped
> folds and insufficient treatment variation for forward-chaining
> cross-fitting. Because the dataset contains only one treated region
> and treatment is assigned deterministically as North after 1 May 2025,
> we do not claim a validated causal ATE or CATE. The project
> demonstrates the methods and their diagnostics while identifying the
> additional market-level treatment variation required for defensible
> causal estimation.

## 9. Repository artifacts reviewed

The following files were inspected on the `Dhananjay` branch: -
`03_synthetic_control.ipynb` -
`05_synthetic_control_robustness_updated.ipynb` -
`04_double_machine_learning_exploratory.ipynb` -
`04b_dml_validation_audit.ipynb` - `README.md` -
`DONOR_SENSITIVITY_INVESTIGATION_2026-11-02.md` -
`DML_IMPLEMENTATION_REVIEW_2026-11-04.md` -
`DML_VALIDATION_AUDIT_2026-11-05.md`

This report consolidates findings visible in those repository artifacts.
It does not certify a real-world causal effect or substitute for
confirmation of the data-generation process.
