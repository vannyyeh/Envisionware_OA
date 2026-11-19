# Decision Memo: Build Mobile Printing First

---

## Recommendation

Prioritize **Mobile Printing**, allowing patrons to print from their phones or laptops through Fernwick's existing Print platform. Start with a limited release and three pilot customers.

Fernwick currently supports desktop and kiosk print release but not mobile printing (`pricing.csv`). Customer requests, lost deals, and churn support this opportunity.

Reserve is a close alternative. Mobile Printing could recover its **build-only cost**, but the current model does not recover full-team costs within 18 months. I recommend a limited investment first, with full development dependent on validated costs and payback.

## 1. Why Mobile Printing

- **Lost deals:** 32 deals cite missing Mobile Printing. Of these, 23 deals worth **$205,040** are in covered sales regions, including 13 lost new-customer deals ($137,590), 6 expansions, and 4 renewals. I excluded 9 deals ($81,570) outside sales coverage (`deals.csv`, `regions.csv`).
- **Churn:** 14 customers moved to PrintPal, representing **$158,960 in lost ARR** (`churn_events.csv`).
- **Demand:** 330 requests from 175 unique customers across five mobile printing feature titles, excluding internal submissions. Of these, **113 active Print customers represent 655 branches**. In the survey, 18.5% identify printing from personal devices as their top problem; 55% of this group would pay over $500 annually per branch (`feature_requests.csv`, `customers.csv`, `librarian_survey.csv`).
- **Trend:** Covered lost deals and churn totaled approximately $151k in the last 12 months, compared with a three-year annual average of $128k.

## 2. Market Size

TAM and SAM use **$1,200 per branch annually**, the low end of the competitor pricing range in `analyst_report_mobile_printing.md`.

| Market | Definition | Systems | Branches | Annual Value |
|---|---|---:|---:|---:|
| TAM | All public libraries | 7,800 | 18,965 | **$22.8M** |
| SAM | Covered regions, non-rural, excluding PrintPal | 2,220 | 7,115 | **$8.5M** |
| SOM | Base ARR by month 18 | | | **$146k to $228k** |

**Sources:** `market_library_systems.csv`, `regions.csv`, `pricing.csv`.

SAM excludes rural libraries because Fernwick penetration is only 2.3%, and excludes PrintPal customers who already have mobile printing. Blank or unknown vendors are assumed to lack mobile printing, which may overestimate SAM.

**SOM assumptions:** Sales begin after month 3, followed by a linear ramp. A planning assumption of **50% of eligible branches adopting** is used for existing customer upsells. Historical lost deals provide a proxy for future opportunity flow, not guaranteed recoverable sales.

| Revenue Source | Basis | Month 18 ARR |
|---|---|---:|
| New customers | Historical covered lost new-customer deals, annualized with 50% conversion | $28.7k |
| Existing customer upsell | 655 eligible branches × 50% adoption × $350 to $600 | $114.6k to $196.5k |
| Expansion / renewal | Eligible historical losses, excluding overlap with upsells | $2.5k |
| **Base SOM** | | **$146k to $228k** |
| Retention upside | Potential future churn avoided, based on historical PrintPal churn | +$37.3k |
| **SOM including upside** | | **$183k to $265k** |

The **$350 to $600 add-on price is a pricing sensitivity range**, not validated willingness to pay. I would initially target $350 so Fernwick's total Print subscription ($1,100 + $350) does not exceed PrintPal's $1,450 package. A higher price would require evidence of additional customer value.

Retention is excluded from the base case because historical churn does not confirm how many future cancellations Mobile Printing can prevent. Previously churned customers are also excluded from win-back revenue. Overlapping deals and inactive customers were removed from the upsell calculation.

## 3. Financial Payback

The model uses Fernwick Print's **84% gross margin** (`financials_monthly.csv`), annual R&D cost of $125.8k per person, and Sales cost of $135.7k (`headcount_costs.csv`). QA is estimated at one R&D FTE.

Cumulative 18-month gross profit assumes linear adoption during months 4 to 18:

**Gross profit = Month 18 ARR × 15/12 × 0.5 × 84%**

| Cost Scenario | Cost | Base Payback Ratio |
|---|---:|---:|
| Full team for 12 months: builder, QA, 0.5 sales | $319k | **0.24 to 0.37** |
| Builder + QA for 12 months | $252k | 0.30 to 0.47 |
| **Build-only: builder + QA for 3 months** | **$63k** | **1.21 to 1.90** |

Payback ratio = cumulative gross profit / cost. A ratio of 1.0 means the specified cost is recovered.

The base case generates approximately **$77k to $120k cumulative gross profit** in 18 months. It can recover the assumed three-month build cost, but not a full-year team investment.

The three-month build is an assumption, not a confirmed estimate. Every additional month of builder and QA adds approximately $21k. Costs after the first year and ongoing operating expenses are not fully modeled, so full initiative payback remains unproven.

**Decision Gate**

Proceed beyond the limited release only if:

1. Existing Print infrastructure supports Mobile Printing with limited integration costs and no required new hardware.
2. At least two of three pilot customers confirm willingness to pay.
3. The updated financial model demonstrates **18-month payback under the actual approved development budget**.

If these conditions fail, reconsider Reserve.

## 4. Strongest Alternative: Reserve Check-in and No-show Release

| Evidence | Mobile Printing | Reserve |
|---|---|---|
| Unique requesting customers | 175 | 46 |
| Active requesting customers | 113 | 38 |
| Eligible units among active requesters | 655 branches | 539 rooms |
| SAM | $8.5M | $5.3M |
| Covered lost deals | 23 / $205k | 68 / $265k |
| Lost new-customer deals attributed to missing features | **13** | **0 recorded** |
| Covered churn | 14 / $159k | 42 / $409k |
| Last 12-month recorded losses | $151k | $130k |

Reserve has larger historical churn losses, an 86% gross margin, and the highest survey pain (19.9%).

However, recorded churn to BookIt Cloud declined from $247,950 in 2024 to $123,470 in 2025 and $47,440 during Jan to Sep 2026. Also, not all departures can be attributed to missing Check-in and No-show Release features.

**Only Mobile Printing has documented lost new-customer deals directly attributed to missing features in this dataset.** Reserve may still attract new customers, but that opportunity is not demonstrated by these lost-deal records.

Mobile Printing also serves existing Print customers through add-ons and expands Fernwick's product offering. This gives it more documented ways to generate additional revenue.

The SAM values use different units, branches versus rooms, so they are not directly equivalent. I would reconsider Reserve if BookIt churn increases or Mobile Printing requires substantial integration costs.

## 5. Other Initiatives Not Selected

- **AI Chatbot:** The sponsored $4.2B market estimate has no surveyed paying deployments. Only 5.3% of staff identify it as their top pain, and 57% of those would pay nothing.
- **Events:** 287 of 329 requests were internally logged; 40 customers requested it. Lost deals total $45k.
- **Pay Rebuild:** Tickets increased after Pay 4.2, but 83.5% of post-release tickets involved user errors, guidance, or documentation, compared with 8.4% defects. Prioritize documentation and targeted workflow fixes.
- **Dark Mode:** 2,496 votes came from only three customers, with no documented renewal impact.

## 6. Data Limitations

Customer ARR does not reconcile with financial reporting, so I use `customers.csv` for customer-level analysis and `financials_monthly.csv` for margins. Support ticket categories are 52% blank; I used resolution codes. Vendor names were normalized.

The largest uncertainties are **integration cost, add-on pricing, adoption rate, and the CFO's cost definition**. The model does not yet demonstrate full-initiative payback.

Detailed calculations: `sizing_calc_v2.py`.