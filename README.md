# Why Is Our Basket Getting Smaller?
### A Business Analysis Investigation — FreshMart Supermarkets

> Footfall is up 6.8%. The average basket is down 10.0%. Leadership assumed customers were simply buying less.
> They weren't. **The shape of the average shopping trip changed — and the data shows exactly how, where, and what to do about it.**

---

## The Headline

| | H1 2025 | H2 2025 | Change |
|---|---|---|---|
| Transaction volume | 31,109 | 33,221 | **+6.8%** |
| Average basket value | R290.01 | R261.10 | **−10.0%** |
| Average items per basket | 14.22 | 12.72 | **−10.6%** |
| Total sampled revenue | R9.02m | R8.67m | **−3.9%** |

Three root causes explain almost all of it:

1. **Mix shift (primary driver).** Small "top-up" trips grew from 38% to 46% of all transactions; large "stock-up" trips fell from 40% to 34%. Spend *within* each trip type barely moved — R474.50 → R472.62 for a large trip, R90.20 → R90.21 for a small one. Nobody is spending less. More people are just making the cheaper kind of trip, more often.
2. **Competitor entry (secondary driver).** The 12 stores that gained a nearby discount competitor lost **~17% more** basket value than matched stores over the same 60-day window — a difference-in-differences result consistent across all 12 stores individually.
3. **Stockouts (minor driver).** Baskets are ~16% smaller on stockout-affected days, but this touches only 6 of 42 stores and effectively one category.

Six hypotheses were tested in total. Store format, province, day of week, and payment method were all ruled out — the decline is not concentrated anywhere those factors would predict. Full reasoning, evidence, and confidence levels are in the written report.

---

## What's in This Submission

| File | What it is | Use it for |
|---|---|---|
| **`FreshMart_Basket_Decline_Report.docx`** | The full written report (13 pages) | The primary deliverable — problem framing, hypotheses, evidence, root cause, recommendations, success metrics |
| **`FreshMart_Boardroom_Deck.pptx`** | 12-slide executive presentation | The 10-minute steering-group walkthrough |
| **`FreshMart_Supporting_Analysis.ipynb`** | Reproducible Python/pandas analysis | Re-running or auditing every number in the report, cell by cell |
| **`freshmart_analysis.sql`** | The same analysis, in SQL | An alternative, fully independent reproduction of every finding |
| **`FreshMart_Dataset.db`** | SQLite database (loaded from the source workbook) | Running `freshmart_analysis.sql` directly |
| **`FreshMart_Dataset.xlsx`** | The original three-table dataset | Source data, as supplied |
| **Live dashboard** — [freshmart-basket-insight.lovable.app](https://freshmart-basket-insight.lovable.app) | Interactive one-page version of the findings, built in Lovable | A shareable, browsable alternative to the static deck |

Two independent analysis paths — Python and SQL — were built deliberately, not redundantly: every headline number (the +6.8%, the −10.0%, the −16.8% competitor effect) is cross-checked and identical in both, which is itself part of the evidence that the findings are solid rather than a scripting artifact. The live dashboard was verified line-by-line against these same source numbers after build.

---

## How to Reproduce This

**Python notebook**
```bash
pip install pandas numpy matplotlib jupyter
jupyter nbconvert --to notebook --execute --inplace FreshMart_Supporting_Analysis.ipynb
```
Requires `FreshMart_Dataset.xlsx` in the same folder. Runs top to bottom with no manual steps.

**SQL**
```bash
sqlite3 FreshMart_Dataset.db < freshmart_analysis.sql
```
Portable to PostgreSQL / BigQuery with minor date-function changes, noted inline in the script.

---

## Methodology at a Glance

| # | Hypothesis | Verdict |
|---|---|---|
| H1 | Customers are uniformly buying less per trip |  Rejected — within-segment spend is stable |
| H2 | Trip mix is shifting toward smaller, more frequent visits |  **Supported — primary driver** |
| H3 | New discount competitors are pulling basket value away |  **Supported — secondary, localised driver** |
| H4 | Stockouts are causing smaller baskets | Supported — real, but narrow (6 stores, 1 category) |
| H5 | Pricing changes are driving the decline |  Not testable — no price data in this extract |
| H6 | Store format or province explains it |  Rejected — decline is uniform across both |
| — | Day of week / payment method |  Rejected — red herrings, no differential pattern |

The competitor hypothesis was tested with a proper **event study and difference-in-differences design** — 60-day before/after windows at the 12 affected stores, benchmarked against the 30 unaffected stores over the identical calendar period — specifically to separate a genuine competitive effect from the chain-wide seasonal trend already explained by the mix shift. This is the same guard-rail the assignment brief warns about directly: don't let a shift in the mix of customers or trips masquerade as individual behaviour change.

---

## What FreshMart Should Do Next

1. **Basket-building offers** targeted at the fast-growing top-up segment (bundle deals, small-basket multi-buys) — *Merchandising Lead, high impact, low–medium effort*
2. **Loyalty-linked incentive** for consolidating trips into fewer, larger visits — *Loyalty/CRM Lead, medium–high impact*
3. **Targeted competitive response** (local price-matching, geo-targeted promotions) at the 12 competitor-affected stores — *Pricing Manager + Store Managers, concentrated impact*
4. **Fix the Household Cleaning replenishment gap** at the 6 stockout-affected stores — *Regional Ops Manager, low-cost, quick win*

Each recommendation is paired with a fair test design (pilot stores vs. matched controls, reusing the same before/after methodology validated on the competitor analysis) and a monitoring dashboard that tracks trip-type mix — not just the blended average that concealed this story in the first place.

---

## Data Notes & Limitations

- `Transactions` is a **representative sample**, not a full census — all conclusions here rely on relative comparisons (rates, shares, before/after) rather than absolute totals, which a sample supports reliably.
- No pricing, promotion, or online/delivery-channel data was available in this extract. Both are flagged as open questions and a recommended follow-up data request, not resolved claims.
- Full assumptions and caveats are documented in the report's appendix.

---
