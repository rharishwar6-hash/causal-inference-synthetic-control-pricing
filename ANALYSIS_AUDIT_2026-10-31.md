# Analysis Audit — 31 October 2026

## Scope
Independent review of the synthetic dataset and the updated weekly/daily synthetic-control specification. This is a methodological audit, not validation of a real commercial intervention.

## Data checks
- 540 rows and 11 columns.
- 180 daily observations per region, January 1–June 29, 2025.
- Regions: North (treated), South and West (donors).
- No duplicate date-region observations and no missing dates in the observed range.
- Treatment is assigned to North from May 1, 2025; South and West remain untreated.
- Recorded revenue differs from `our_price × units_sold` by at most ₹0.79; zero rows exceed an absolute tolerance of ₹1. Small differences are consistent with rounding.

## Main estimates

| Specification | Post-period mean gap | Pre-period normalized RMSE | Donor weights |
|---|---:|---:|---|
| Weekly | +3.047% | 3.979% | South 47.167%; West 52.833% |
| Daily | +4.401% | 9.760% | South 52.197%; West 47.803% |

Weekly analysis excludes the mixed week ending May 4, 2025 from the post-period. The weekly post-period starts with the first complete post-intervention week. Daily analysis uses the exact May 1 boundary.

## Robustness diagnostics

### Donor sensitivity (weekly)

| Donor specification | Estimated mean gap |
|---|---:|
| South + West | +3.047% |
| South only | +14.492% |
| West only | −5.395% |

The sign changes under West-only estimation and the South-only estimate is much larger. The estimated gap is therefore materially donor-sensitive.

### Placebo dates

| False intervention date | Estimated gap | Weekly observations used to fit pre-period weights |
|---|---:|---:|
| February 1, 2025 | −1.288% | 3 |
| March 1, 2025 | −1.773% | 7 |
| April 1, 2025 | −2.506% | 12 |

These are short pre-periods and descriptive diagnostics only. They are not formal statistical significance tests.

## Interpretation
- A positive gap means North's observed revenue exceeded the modelled synthetic counterfactual in the selected post-period.
- It does **not** establish that the simulated pricing intervention caused the difference.
- The dataset is synthetic, the donor pool has only two regions, and the result changes substantially with donor selection.
- The daily and weekly specifications produce different estimates and different pre-treatment fit.
- Do not describe the result as a proven uplift or use it as a real pricing recommendation.

## Recommended next steps
1. Inspect pre-treatment trajectories and donor-specific fit before interpreting the weights.
2. Keep the donor-sensitivity table visible in the final report and presentation.
3. If adding placebo inference, use a defensible placebo design with comparable pre-period lengths and report the limitations.
4. Define treatment, outcome, confounders, and unit of analysis before attempting Double Machine Learning (DML). Do not claim DML is implemented until there is a runnable, reviewed notebook.
5. Do not merge the `Dhananjay` branch into `main` until the divergent branch history and file-level differences have been reviewed with the team.

## Provenance
Findings are based on the project's synthetic dataset and the updated robustness notebook. Reported percentages are specification-dependent and should be regenerated after any data or code changes.
