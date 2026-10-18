# Week 1 Report: Causal Inference and Synthetic Control for Algorithmic Pricing

## 1. Project Overview

The objective of this project is to analyze the potential impact of an algorithmic pricing intervention on revenue using causal inference techniques.

The project uses a synthetic dataset to study pricing, demand, competitor prices, marketing activities, promotions, and revenue.

---

## 2. Dataset Description

| Attribute | Details |
|---|---|
| Dataset | `project1_algorithmic_pricing_synthetic_dataset.csv` |
| Total Records | 540 |
| Regions | North, South, West |
| Observation Period | January 1, 2025 – June 29, 2025 |
| Dataset Type | Synthetic / Simulated |
| Treated Region | North |
| Intervention Date | May 1, 2025 |

The dataset contains pricing, competitor prices, units sold, revenue, marketing spend, promotion status, treatment assignment, and time-period information.

---

## 3. Data Validation

Data validation was performed to check the quality and consistency of the dataset.

**Validation checks performed:**

- Missing values and duplicate records
- Date formats and data types
- Invalid or negative numeric values
- Treatment assignment consistency
- Pre-treatment and post-treatment period consistency
- Duplicate date-region-category combinations
- Missing dates and potential outliers

**Observation:** Revenue validation identified differences between recorded revenue and `our_price × units_sold`. These differences require further investigation before confirming the revenue calculation formula.

---

## 4. Exploratory Data Analysis (EDA)

The EDA phase focuses on understanding revenue patterns and pricing differences across regions.

**Analyses performed:**

1. Region-wise total revenue comparison
2. Average own price versus competitor price comparison
3. Daily revenue trend visualization

These analyses help identify regional revenue patterns, pricing differences, and changes in revenue over time.

*Actual findings should be documented after reviewing the generated charts.*

---

## 5. Causal DAG

A preliminary Directed Acyclic Graph (DAG) was developed to represent assumed causal relationships among the project variables.

**Key variables:**

- **Treatment:** Algorithmic pricing intervention
- **Own Price:** Our product price
- **Demand:** Units sold
- **Competitor Price:** Competitor's product price
- **Marketing Spend:** Marketing expenditure
- **Promotion:** Promotion activity
- **Outcome:** Revenue

DoWhy was used to specify the causal model and identify the estimand.

**Important:** The DAG represents assumed causal relationships. It does not independently prove causation.

---

## 6. Treatment Period Analysis

The intervention date was defined as **May 1, 2025**.

| Parameter | Details |
|---|---|
| Pre-treatment Period | Before May 1, 2025 |
| Post-treatment Period | May 1, 2025 onward |
| Treated Region | North |
| Comparison Regions | South and West |
| Expected Pre-treatment Records | 360 |
| Expected Post-treatment Records | 180 |

Treatment assignment was checked against the expected rule: North receives treatment from the intervention date onward, while South and West remain untreated.

---

## 7. Week 1 Deliverables

| File | Purpose |
|---|---|
| `01_data_validation.ipynb` | Data validation and quality checks |
| `02_eda.ipynb` | Exploratory data analysis |
| `03_causal_dag.ipynb` | Causal DAG and DoWhy model |
| `03_causal_dag.png` | Causal DAG diagram |
| `04_treatment_period.ipynb` | Treatment-period analysis |
| `week1_report.md` | Week 1 project report |

---

## 8. Limitations and Next Steps

The following activities remain important for the next phase:

- Investigate revenue calculation differences.
- Review causal assumptions and potential confounders.
- Document actual EDA findings from the charts.
- Evaluate whether South and West provide suitable comparison groups.
- Continue with synthetic control estimation and further causal analysis.

---

## 9. Conclusion

Week 1 established the foundation for the project through data validation, exploratory data analysis, causal DAG development, and treatment-period verification.

Further analysis is required before drawing conclusions about the causal impact of the pricing intervention.
