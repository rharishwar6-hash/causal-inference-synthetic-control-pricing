# DML Data Feasibility Review — 3 November 2026

## Scope
This review checks whether the current project dataset can support a credible Double Machine Learning (DML) analysis for the algorithmic pricing intervention. It is a data/design assessment, not a fitted DML result.

## Dataset structure
- 540 rows, 11 columns.
- 180 daily dates from 1 January to 29 June 2025.
- Three regions: North, South, and West; 180 rows per region.
- One product category only: Home Essentials.
- 480 untreated rows and 60 treated rows.
- Treatment is assigned only to North during the 60-day post-treatment period beginning 1 May 2025.

## Candidate variables
| Role | Variable | Recommendation |
|---|---|---|
| Outcome (Y) | `revenue` | Use as the primary outcome, subject to the chosen estimand. |
| Treatment (T) | `treatment` | Use as the intervention indicator (North × post-period). |
| Candidate controls (X) | `competitor_price`, `marketing_spend`, `promotion_active` | Potential pre-treatment confounders, but check whether they are affected by treatment before adjusting. |
| Time controls | Date trend / calendar features | Consider to represent common time movement; avoid leakage and use time-aware validation. |
| Exclude as a control for total effect | `our_price` | The pricing intervention changes own price; controlling for it may block the mechanism/mediated part of the total effect. |
| No variation | `product_category` | Only one category is present, so it cannot explain variation and should be omitted. |
| Design indicator | `region`, `period` | Needed to describe assignment structure, but region/time fixed effects and the treatment interaction must be specified carefully. |

## Key design limitation
Treatment is not broadly assigned across independent units. There is one treated region (North), and South and West are the only two comparison regions. The 60 treated rows are repeated daily observations from the same treated region, not 60 independent treated markets. Standard row-level cross-fitting or standard errors can therefore overstate the effective sample size and understate uncertainty.

There is also a causal-design issue: `treatment` is exactly the North-by-post indicator. With only three regions and one treated region, DML cannot manufacture additional independent treatment variation. Flexible nuisance models do not solve the lack of treated clusters or establish conditional exchangeability/overlap.

## Recommended honest approach
1. Keep Synthetic Control as the primary design for this regional intervention, while reporting its donor sensitivity and fit limitations.
2. If the project requires DML, implement it as an explicitly **exploratory methodological comparison**, not as independent confirmation of a causal effect.
3. Define the estimand first (e.g. average effect on treated North observations after 1 May).
4. Use `revenue` as Y and `treatment` as T. Start with a small, pre-specified control set. Do not include `our_price` when estimating the total effect of the pricing intervention.
5. Use date-blocked or time-aware validation rather than randomly splitting individual daily rows. Explain that only one treated region and two donor regions limit valid cluster-level inference.
6. Report sensitivity to controls and model specification, and avoid presenting conventional DML confidence intervals as reliable unless the dependence/cluster problem is addressed.

## Conclusion
The dataset can support a small DML coding demonstration, but it does **not** provide a strong basis for a reliable high-dimensional DML causal estimate. The main limitation is the treatment design (one treated region and two donor regions), not the choice of machine-learning algorithm. Any DML result should be labelled exploratory and interpreted alongside, not instead of, the Synthetic Control analysis.
