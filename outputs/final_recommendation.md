# Final Recommendation

## Which factors appear most strongly associated with monthly sales?

Based on the multiple regression model (R² = 0.8185, explaining ~82% of the variation in monthly sales across 306 store-months):

1. **Footfall** — the single strongest driver. Each additional visitor is associated with ~₹28.79 more in monthly sales (p < 0.001). Alone, footfall explains 73.5% of sales variation.
2. **Inventory availability** — each 1 percentage-point increase in inventory availability is associated with ~₹2,857 more in monthly sales (p < 0.001).
3. **Staffing levels** — each additional staff member is associated with ~₹2,693 more in monthly sales (p = 0.040).
4. **Marketing spend** — each additional ₹1 spent is associated with ~₹1.15 more in sales (p < 0.001), a positive but comparatively modest return per rupee versus the other levers.
5. **Store format** — Airport stores significantly outsell High Street stores (+₹21,356, p = 0.025); Residential stores significantly undersell High Street stores (−₹15,193, p = 0.016).

## Which variables should leadership focus on?

- **Footfall-driving initiatives** (location visibility, store hours, local partnerships, signage) — the largest, most statistically robust lever.
- **Inventory availability** — keeping shelves stocked has a large, highly significant, and directly controllable effect on sales.
- **Staffing adequacy** — adequate staffing is associated with meaningfully higher sales, likely through better customer service and reduced lost sales from understaffing.
- **Marketing spend** — still worth investing in, but expect smaller incremental returns per rupee than the operational levers above.
- **Store-format strategy** — when planning new locations or reallocating investment, Airport-format stores show a real sales advantage over High Street, and Residential stores show a real disadvantage, worth investigating further (rent, catchment size, dwell time).

## Which variables should not be over-interpreted?

- **avg_discount_pct** — not statistically significant in either the simple (p = 0.186) or multiple regression model (p = 0.128). The data does not support a confident claim that discounting increases or decreases sales; leadership should not use this model to justify either expanding or cutting discount programs.
- **Mall store-type effect** — positive but only borderline significant (p = 0.097, just above the conventional 5% threshold). Treat any "Malls outperform High Street" claim as tentative, not confirmed.
- **competitor_distance_km and customer_rating** — these were reviewed during data preparation but were not included in the final model; they showed weak/no correlation with sales in this dataset and should not be assumed to be unimportant in general, just unsupported by this particular analysis.

## What business action would you recommend?

1. Prioritize **footfall-generation** initiatives and **inventory availability** as the two highest-confidence, highest-impact levers identified.
2. Review **staffing levels** at underperforming stores — the model suggests understaffing carries a real sales cost.
3. Maintain marketing investment but **set realistic expectations** on its standalone impact; pair it with the operational levers above rather than treating it as the primary growth driver.
4. **Do not change discounting strategy based on this analysis alone** — the data doesn't support discounting as a meaningful sales driver in either direction.
5. Investigate the **Airport store-format advantage** and **Residential store-format disadvantage** further before making expansion or closure decisions — the effect is real in this data but the underlying cause (location, catchment, rent, dwell time) is not captured here.
6. Flag the stores with the **largest negative residuals** (see `analysis/residual_analysis.md`) for local follow-up, since they underperformed even after accounting for all measured drivers.

## What risks or limitations should leadership keep in mind?

- The dataset covers only **4 months (Jan–Apr 2025)** and **80 stores** — seasonal effects (e.g. festive periods, summer) are not well captured, and the sample size limits how finely results can be sliced (e.g. by region × store type).
- **14 records were dropped** due to missing `competitor_distance_km` or `customer_rating` data; this is a small fraction (~4%) of the dataset and unlikely to bias results materially, but it is not zero.
- The model captures **association, not mechanism** — it tells us *what moves together*, not *why*.
- Some predictors (e.g. footfall and staffing) may themselves be influenced by store size or location, which are not directly measured — so part of what looks like a "staffing effect" could partly reflect store size instead.

## Why does regression show association but not automatically prove causation?

Regression coefficients describe how variables move together in the observed data — they do not, by themselves, establish that changing one variable will cause a proportional change in another. Three reasons this matters here:

1. **Reverse causality is possible.** A well-staffed, well-stocked store might be well-staffed and well-stocked *because* it's a high-performing location with more revenue to support that investment — not the other way around.
2. **Confounding variables may exist.** A store's location quality could simultaneously drive footfall, justify higher marketing spend, and support better staffing — making these variables correlated with sales for a shared underlying reason, not because each one independently causes sales to rise.
3. **No experiment was run.** This is observational data — no stores had marketing spend, staffing, or inventory levels *randomly assigned* to them. Without controlled variation, we cannot rule out that some other unmeasured factor (e.g. local market strength) is driving both the "cause" variables and the sales outcome together.

**Practical implication:** Use this analysis to prioritize *where to test*, not as proof of *what will work*. For example, before rolling out a staffing increase chain-wide, consider piloting it in a subset of stores and measuring the actual sales change — that would move the evidence from correlational to causal.
