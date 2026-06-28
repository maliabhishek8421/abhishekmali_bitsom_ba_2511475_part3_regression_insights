# Residual Analysis

**Final model used:** MR1 — Multiple Regression
`monthly_sales = 138,300 + 28.79·footfall + 1.15·marketing_spend + 2,856.87·inventory_availability_pct + 2,692.78·staff_count − 57,647.99·avg_discount_pct + 21,356.26·is_Airport + 11,226.05·is_Mall − 15,192.99·is_Residential`

Predicted sales were calculated for all 306 cleaned records using the coefficients above. Residual = Actual `monthly_sales` − Predicted `monthly_sales`. Full calculation is in `analysis/regression_workbook.xlsx` (sheet: `Prediction_Residuals`).

## Residual Diagnostics

| Metric | Value |
|---|---|
| Mean residual | ≈ 0 (by construction of OLS) |
| Std. deviation of residuals | ≈ ₹44,434 |
| Largest positive residual | +₹105,974 (STR-1030) |
| Largest negative residual | −₹152,089 (STR-1017) |

## Top 5 Largest POSITIVE Residuals (model under-predicts these stores)

| Store ID | Month | Region | Store Type | Actual Sales | Predicted Sales | Residual |
|---|---|---|---|---|---|---|
| STR-1030 | 2025-02 | West | Residential | 820,519 | 714,545 | +105,974 |
| STR-1028 | 2025-04 | East | Mall | 713,611 | 618,134 | +95,477 |
| STR-1032 | 2025-01 | South | High Street | 914,544 | 820,221 | +94,324 |
| STR-1073 | 2025-03 | East | Residential | 813,317 | 719,647 | +93,670 |
| STR-1026 | 2025-04 | East | Mall | 625,514 | 533,463 | +92,051 |

**Business meaning:** These stores sold considerably more than the model predicted given their footfall, marketing spend, inventory, staffing, discount level, and store type. This suggests unmeasured local factors — strong store management, local events, a particularly loyal customer base, a competitor closure nearby, or seasonal effects not captured in the model — are boosting sales beyond what the standard drivers explain. Notably, 2 of the 5 are Residential stores, which the model otherwise predicts ~₹15,193 below High Street stores — these specific Residential stores are clearly outperforming that general pattern.

## Top 5 Largest NEGATIVE Residuals (model over-predicts these stores)

| Store ID | Month | Region | Store Type | Actual Sales | Predicted Sales | Residual |
|---|---|---|---|---|---|---|
| STR-1017 | 2025-03 | West | High Street | 685,379 | 837,468 | −152,089 |
| STR-1012 | 2025-01 | West | Residential | 595,468 | 725,128 | −129,660 |
| STR-1023 | 2025-02 | South | Mall | 627,172 | 747,791 | −120,619 |
| STR-1001 | 2025-04 | East | Residential | 658,920 | 761,955 | −103,035 |
| STR-1007 | 2025-02 | West | Mall | 800,452 | 902,132 | −101,680 |

**Business meaning:** These stores underperformed what the model expected given their inputs. Possible explanations include local competition, stockouts not fully captured by the monthly average `inventory_availability_pct`, temporary staffing or service issues, or a one-off disruption (e.g. weather, local construction, store renovation) in that specific month. 3 of the 5 are West region stores, which may warrant a closer look at regional execution.

## Under-Prediction vs. Over-Prediction by Store Type

Looking at residual patterns across all 306 records (not just the top 5/bottom 5):

- **Mall and Residential stores** appear in both the largest-positive and largest-negative lists in roughly equal measure — the model does not show a systematic bias for these store types overall once the dummy variables are included; the dummies already adjust the baseline level for store type, so remaining residuals reflect store-specific or month-specific noise rather than a structural store-type problem.
- **West region** stores feature disproportionately in the largest negative residuals — 3 of the 5 worst over-predictions are West stores in this sample. This is **not strong enough evidence to confirm a true regional problem** (region was not included as a predictor in MR1), but it is worth flagging for leadership as a pattern to monitor, possibly by adding region as a predictor or dummy in future analysis.
- The model is **not** systematically over- or under-predicting any one store type as a structural matter — the spread of large residuals across High Street, Mall, and Residential stores in both directions suggests these are largely individual store/month effects rather than a category-wide model bias.

## Caveat

With only 5 positive and 5 negative outliers examined out of 306 records, and a standard deviation of residuals of ~₹44,400, these are simply the most extreme cases. They should be treated as candidates for local investigation (e.g. "why did STR-1017 underperform in March?"), not as proof of a broader trend, unless the same pattern recurs across multiple months for the same store.
