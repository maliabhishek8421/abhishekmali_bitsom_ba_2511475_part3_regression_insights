# Part 3: Regression-Based Business Insights & Model Interpretation

## Business Problem Summary

A retail chain's leadership team wants to understand what is driving monthly sales performance across its stores, so they can decide where to focus business actions — marketing spend, inventory investment, staffing, discounting strategy, and store-format/location strategy. This project uses regression analysis on store-level monthly data to identify which factors are most strongly associated with `monthly_sales` and to provide a data-backed business recommendation.

## Dataset Description

- **File:** `data/business_regression_data.xlsx`
- **Size:** 320 rows × 14 columns — 80 unique stores, each with 4 months of data (January–April 2025).
- **Granularity:** One row per store per month.

| Column | Type | Description |
|---|---|---|
| `store_id` | Identifier | Unique store code (e.g. STR-1001). Not useful as a regression predictor — it's an ID, not a measurable factor. |
| `month` | Date | Reporting month (Jan–Apr 2025). |
| `region` | Categorical | East / North / South / West. |
| `store_type` | Categorical | High Street / Mall / Airport / Residential. |
| `marketing_spend` | Numerical | Monthly marketing spend (₹). |
| `footfall` | Numerical | Monthly visitor count. |
| `avg_discount_pct` | Numerical | Average discount percentage applied during the month. |
| `staff_count` | Numerical | Number of staff at the store that month. |
| `inventory_availability_pct` | Numerical | % of inventory in stock / available during the month. |
| `competitor_distance_km` | Numerical | Distance to nearest competitor (km). Has 6 missing values. |
| `holiday_flag` | Numerical (binary) | 1 if the month included a major holiday period, else 0. |
| `customer_rating` | Numerical | Average customer rating (1–5). Has 8 missing values. |
| `monthly_sales` | Numerical | **Dependent variable.** Total monthly sales (₹). |
| `monthly_profit` | Numerical | Monthly profit (₹). Excluded from regression — it is a downstream *outcome* of sales (correlated with the dependent variable), not an independent driver, so including it would be circular. |

### Variables Needing Cleaning / Transformation
- `competitor_distance_km` (6 missing) and `customer_rating` (8 missing) — rows with missing values in either field were dropped (listwise deletion), leaving **306 clean rows** used for all models.
- `store_type` and `region` — categorical text fields converted to dummy (0/1) variables for use in regression.
- `month` — kept as an identifier/date for reference; not used directly as a numeric predictor given the short 4-month window.

### Variables Not Useful for Regression (as-is)
- `store_id` — unique identifier, not a measurable business factor.
- `monthly_profit` — outcome variable correlated with sales, not a driver of it.
- `month` — too few distinct values (4) to model a reliable trend; used for context only.

## Dependent and Independent Variables

- **Dependent variable:** `monthly_sales`
- **Independent variables tested:** `marketing_spend`, `footfall`, `avg_discount_pct`, `inventory_availability_pct`, `staff_count`, `store_type` (as dummies)

## Regression Approach

1. **Simple linear regression** — three single-variable models against `monthly_sales`: `marketing_spend`, `footfall`, and `avg_discount_pct`.
2. **Multiple linear regression** — one combined model using `footfall`, `marketing_spend`, `inventory_availability_pct`, `staff_count`, `avg_discount_pct`, and three `store_type` dummy variables.
3. All regression statistics (slope, intercept, R², standard error, t-stat, p-value) were computed using native Excel formulas (`SLOPE`, `INTERCEPT`, `RSQ`, `CORREL`, `STEYX`, `LINEST`, `T.DIST.2T`) inside `analysis/regression_workbook.xlsx`, and independently cross-checked against a Python/statsmodels OLS run on the same cleaned data — both produced identical coefficients and R² values.

## Dummy Variable Approach

`store_type` (4 categories: High Street, Airport, Mall, Residential) was converted into 3 dummy variables — `is_Airport`, `is_Mall`, `is_Residential` — with **High Street as the reference category** (chosen as the most frequent category in the data). Including a 4th dummy for High Street would create perfect multicollinearity (the dummy variable trap), since the dummies would always sum to 1. Full explanation in `outputs/model_equations.md`.

## Model Comparison Summary

| Model | Variable(s) | R-squared | Significant? |
|---|---|---|---|
| SR1 | marketing_spend | 0.157 | Yes |
| SR2 | footfall | 0.735 | Yes |
| SR3 | avg_discount_pct | 0.006 | No |
| **MR1 (Final)** | footfall, marketing_spend, inventory_availability_pct, staff_count, avg_discount_pct, store_type dummies | **0.819** | Mostly yes (avg_discount_pct and is_Mall not significant) |

Full comparison: `analysis/model_comparison.md`. Clean comparison table: `outputs/regression_summary.xlsx`.

## Final Model Selected

**MR1 — Multiple Regression**, because it has the highest R² (0.819 vs. 0.735 for the best simple model) and combines multiple actionable business levers rather than relying on a single variable. Full equation and coefficient interpretation: `outputs/model_equations.md`.

## Business Recommendation

Footfall, inventory availability, and staffing are the strongest, most statistically reliable drivers of monthly sales. Marketing spend has a positive but comparatively modest standalone effect. Discounting shows no statistically significant relationship with sales in this data and should not be adjusted based on this analysis alone. Store format matters — Airport stores significantly outperform High Street stores, and Residential stores significantly underperform them. Full recommendation, risks, and the association-vs-causation discussion: `outputs/final_recommendation.md`.

## Assumptions and Limitations

- Listwise deletion was used for missing data (14 of 320 rows dropped, ~4%).
- Only 4 months of data are available — seasonal patterns beyond Jan–Apr cannot be assessed.
- `monthly_profit` was deliberately excluded as a predictor since it is derived from sales.
- Regression results show **association, not causation** — see `outputs/final_recommendation.md` for a full discussion of why (reverse causality, confounding variables, and the lack of a controlled experiment).
- Multicollinearity was checked (VIF ≤ 6.2 for all multiple-regression predictors) and is within an acceptable range.

## Screenshots Included

| File | Shows |
|---|---|
| `screenshots/simple_regression_output.png` | Simple regression output (footfall model) from the workbook |
| `screenshots/multiple_regression_output.png` | Multiple regression output (final model) from the workbook |
| `screenshots/residuals_preview.png` | Predicted values and residuals from the workbook |
| `screenshots/model_comparison_preview.png` | Model comparison table from the workbook |

## Repository Structure

```
part3_regression_insights/
├── data/business_regression_data.xlsx
├── analysis/
│   ├── regression_workbook.xlsx
│   ├── model_comparison.md
│   └── residual_analysis.md
├── outputs/
│   ├── regression_summary.xlsx
│   ├── final_recommendation.md
│   └── model_equations.md
├── screenshots/
└── README.md
```
