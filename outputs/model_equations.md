# Model Equations

All models use `monthly_sales` (₹) as the dependent variable. Data: 306 store-months (14 rows dropped due to missing `competitor_distance_km` or `customer_rating`).

---

## Simple Regression Equations

**SR1 — marketing_spend**
```
monthly_sales = 567,464 + 2.05 × marketing_spend
```
R² = 0.1574

**SR2 — footfall**
```
monthly_sales = 447,699 + 35.64 × footfall
```
R² = 0.7348

**SR3 — avg_discount_pct**
```
monthly_sales = 697,538 − 114,708 × avg_discount_pct
```
R² = 0.0057 (not statistically significant, p = 0.186)

---

## Multiple Regression Equation (Final Model — MR1)

```
monthly_sales = 138,300
              + 28.79 × footfall
              + 1.15  × marketing_spend
              + 2,856.87 × inventory_availability_pct
              + 2,692.78 × staff_count
              − 57,647.99 × avg_discount_pct
              + 21,356.26 × is_Airport
              + 11,226.05 × is_Mall
              − 15,192.99 × is_Residential
```

R² = 0.8185 | N = 306 | Residual df = 297

### Coefficient Explanations

| Variable | Coefficient | Meaning |
|---|---|---|
| Intercept | 138,300 | Expected monthly sales for a High Street store with zero footfall, marketing spend, etc. (a theoretical baseline, not a realistic scenario on its own). |
| footfall | +28.79 | Each additional visitor per month is associated with ~₹28.79 more in monthly sales, holding other factors constant. |
| marketing_spend | +1.15 | Each additional ₹1 of marketing spend is associated with ~₹1.15 more in sales — a roughly break-even-or-better return, before accounting for the cost of the marketing itself. |
| inventory_availability_pct | +2,856.87 | Each 1 percentage-point increase in inventory availability is associated with ~₹2,857 more in monthly sales. |
| staff_count | +2,692.78 | Each additional staff member is associated with ~₹2,693 more in monthly sales. |
| avg_discount_pct | −57,648 | Direction is negative, but **not statistically significant** (p = 0.128) — do not treat this as proof that discounting hurts sales. |
| is_Airport | +21,356 | Airport stores sell ~₹21,356 more per month than High Street stores, holding other factors constant. |
| is_Mall | +11,226 | Mall stores sell ~₹11,226 more than High Street stores, but this is **not significant at the 5% level** (p = 0.097). |
| is_Residential | −15,193 | Residential stores sell ~₹15,193 less than High Street stores, holding other factors constant. |

### Dummy Variable Explanation

`store_type` has 4 categories: High Street, Airport, Mall, Residential. Including all four as dummies would create perfect multicollinearity (the "dummy variable trap"), since the four columns would always sum to 1 and be perfectly predictable from each other plus the intercept.

**Reference category used: High Street** (chosen because it is the most frequent store type in the dataset — 116 of 320 original rows). Three dummy columns were created:
- `is_Airport` = 1 if store_type is Airport, else 0
- `is_Mall` = 1 if store_type is Mall, else 0
- `is_Residential` = 1 if store_type is Residential, else 0

When all three dummies are 0, the record is a High Street store. Each dummy coefficient is interpreted **relative to High Street**, not in absolute terms.

---

## Final Model Selected

**MR1 (Multiple Regression)** is the final model.

### Reason for Selection
- Highest explanatory power: R² = 0.8185 vs. 0.7348 for the best simple model (footfall alone).
- Combines operational levers (footfall, marketing spend, inventory, staffing) that leadership can actually influence, rather than relying on a single variable.
- Separates the effect of store format (via dummies) from operational performance, so leadership can see both "what to do" and "where it matters most."
- Simple regressions are useful for quick, single-variable checks, but in business reality sales are driven by multiple factors at once — the multiple regression model reflects that more realistically.
