# Model Comparison

Dependent variable: `monthly_sales`. All models fit on N = 306 cleaned store-month records.

| Item | SR1: marketing_spend | SR2: footfall | SR3: avg_discount_pct | MR1: Multiple Regression (Final) |
|---|---|---|---|---|
| **Model name** | Simple Regression 1 | Simple Regression 2 | Simple Regression 3 | Multiple Regression (Final Model) |
| **Variables used** | DV: monthly_sales / IV: marketing_spend | DV: monthly_sales / IV: footfall | DV: monthly_sales / IV: avg_discount_pct | DV: monthly_sales / IVs: footfall, marketing_spend, inventory_availability_pct, staff_count, avg_discount_pct, is_Airport, is_Mall, is_Residential |
| **R-squared** | 0.1574 (15.7%) | 0.7348 (73.5%) | 0.0057 (0.6%) | 0.8185 (81.9%) |
| **Significant variables (p < 0.05)** | marketing_spend (p = 5.6×10⁻¹³) | footfall (p = 1.3×10⁻⁸⁹) | None — avg_discount_pct not significant (p = 0.186) | footfall, marketing_spend, inventory_availability_pct, staff_count, is_Airport, is_Residential all significant. avg_discount_pct (p=0.128) and is_Mall (p=0.097) are **not** significant. |
| **Business usefulness** | Limited on its own — confirms marketing has a positive association but explains too little of sales to guide decisions alone. | Useful as a quick diagnostic — footfall is the single strongest lever, but doesn't tell leadership *why* footfall varies or what else to adjust. | Low — on its own, discount level tells leadership almost nothing about sales outcomes. | High — explains the most variation and combines multiple actionable levers (marketing, inventory, staffing) with store-format context, making it the most decision-useful model. |
| **Limitations** | Ignores footfall, inventory, staffing — likely overstates marketing's true individual contribution since it's correlated with other drivers. | Doesn't separate the effect of footfall from marketing spend or staffing that may also be driving both footfall and sales. | Small effect size and weak/no significance — discounting may matter in ways this simple model can't detect (e.g. interacting with store type or season). | Still correlational, not causal. Multicollinearity is mild (VIF ≤ 6.2 for all predictors) but not zero. Discount effect and Mall effect remain statistically uncertain. Doesn't include time trends, regional effects, or holiday timing in detail. |

## Summary

- **Footfall** is the dominant single driver of sales — by far the strongest simple-regression result (R² = 0.735).
- **Marketing spend** has a real positive association with sales but is a weak predictor in isolation (R² = 0.157).
- **Discounting (avg_discount_pct)** shows no reliable association with sales, whether tested alone or alongside other variables.
- The **multiple regression model (MR1)** is the strongest overall, explaining 81.9% of the variation in monthly sales, and is the recommended model for business decision-making because it reflects how multiple factors act together in practice.
