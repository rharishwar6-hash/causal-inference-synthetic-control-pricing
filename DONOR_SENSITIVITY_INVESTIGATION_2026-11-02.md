# Donor Sensitivity Investigation — 2 November 2026

## Objective
Investigate why the weekly synthetic-control estimate changes from a positive gap under South-only estimation to a negative gap under West-only estimation.

## Data and specification
- Source: `project1_algorithmic_pricing_synthetic_dataset.csv`
- Treated region: North
- Donor regions: South and West
- Intervention date: May 1, 2025
- Daily data: January 1–June 29, 2025
- Weekly data: complete Sunday-ending weeks only
- Weekly pre-treatment period: 16 complete weekly observations
- Weekly post-treatment period: 8 complete weekly observations, starting with the first complete post-intervention week
- Daily pre-treatment period: 120 observations

For the individual-donor diagnostic, each donor is used by itself with weight 1. The pre-treatment RMSE is the square root of the mean squared difference between North and that donor. Normalized RMSE divides RMSE by North's mean pre-treatment revenue. Correlation measures co-movement, not level agreement.

## Individual-donor pre-treatment fit

| Frequency | Donor | Pre observations | North mean revenue | Donor mean revenue | Donor level vs North | RMSE | Normalized RMSE | Correlation |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| Weekly | South | 16 | 435,165.26 | 394,249.79 | 9.40% lower | 44,200.62 | 10.16% | 0.793 |
| Weekly | West | 16 | 435,165.26 | 467,318.99 | 7.39% higher | 40,224.28 | 9.24% | 0.736 |
| Daily | South | 120 | 61,944.39 | 56,217.24 | 9.25% lower | 8,548.05 | 13.80% | 0.485 |
| Daily | West | 120 | 61,944.39 | 66,476.99 | 7.32% higher | 8,949.44 | 14.45% | 0.403 |

Revenue figures are in the dataset's revenue units.

## Individual-donor post-period gaps

Using the same weekly specification as the robustness notebook (excluding the mixed week ending May 4):

| Donor specification | Weekly post-period mean gap |
|---|---:|
| South only | +14.49% |
| West only | −5.40% |

For reference, the daily individual-donor gaps (using May 1 as the exact intervention date) are South +14.61% and West −4.85%. These daily figures are separate from the weekly sensitivity comparison.

## Interpretation

1. South's pre-treatment average revenue is about 9.4% below North's weekly average. Comparing North against South alone therefore produces a large positive post-period gap.
2. West's pre-treatment average revenue is about 7.4% above North's weekly average. Comparing North against West alone produces a negative post-period gap.
3. West has a slightly lower weekly standalone normalized RMSE (9.24% vs 10.16%), while South has a higher weekly correlation with North (0.793 vs 0.736). These diagnostics do not identify one clearly superior donor.
4. At daily frequency, both standalone fits are weaker: normalized RMSE is 13.80% for South and 14.45% for West.
5. The weighted two-donor synthetic control can balance the donors' different revenue levels and co-movement, producing a better pre-treatment fit than either donor alone. That is a mathematical fit improvement, not proof that the weighted series is the true untreated counterfactual.

## Conclusion

The donor sensitivity is not a coding error by itself. It reflects material differences in the donors' revenue levels and their imperfect pre-treatment fit to North. The opposite signs of the standalone estimates demonstrate that the estimated post-treatment gap is highly dependent on donor specification.

Do not choose a donor simply because it gives a preferred post-treatment result. Do not manually alter weights. Retain both donors in the primary specification only as a transparent modelling choice, report the sensitivity results, and describe the causal interpretation as uncertain.

## Important methodological limitations
- Only two donor regions are available.
- The dataset is synthetic.
- Correlation does not establish parallel trends or causal validity.
- A good in-sample pre-treatment fit does not guarantee post-treatment counterfactual validity.
- The analysis does not establish statistical significance or confidence intervals.
- The weekly and daily estimates use different aggregation and fitting samples.

## Recommended next steps
1. Add this diagnostic table and interpretation to the project README/report.
2. Add the diagnostic code cell below the donor sensitivity section in `05_synthetic_control_robustness_updated.ipynb`.
3. Re-run the notebook in Colab and confirm the displayed values.
4. Then move to defining the DML treatment, outcome, confounders, and unit of analysis before implementing it.
