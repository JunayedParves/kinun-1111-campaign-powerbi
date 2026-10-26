# Kinun 11.11 Campaign Intelligence — Power BI Executive Dashboard

> **The story in one sentence:** Kinun's 11.11 mega-sale multiplies daily sales 4x, but at an 11% discount
> it earns a 0.7% contribution margin and loses money after ad spend. The next 11.11 should grow on margin,
> not discount.

A board-level, BCG-style Power BI dashboard for **Kinun**, a Bangladesh consumer-electronics e-commerce platform.
It tracks four years of trading (2022–2025), with a focus on the 54-hour **11.11** campaign
(Nov 10 18:00 – Nov 12 23:59 BDT). The project is stored as a **Power BI Project (PBIP)**, so every page, visual,
measure and relationship is plain text that you can diff and review in Git.

> ⚠️ All figures come from a **synthetic portfolio dataset** that simulates a realistic e-commerce business.
> They are not real Kinun results.

---

## The storyline

The dashboard follows the **Situation → Complication → Resolution** structure used in consulting decks.
Every page title states its takeaway (an *action title*), and every chart title is a DAX measure that
rewrites itself when you change the filters, so the headline stays true for whatever you select.

### Situation: Kinun is growing fast

| Metric (2022–2025) | Value |
|---|---|
| Net sales (delivered) | **৳6.20B**, growing **21.3% a year** (CAGR) |
| Gross merchandise value | ৳7.01B |
| Contribution margin | ৳624M (**10.1%** of net sales) |
| Orders placed | 150,987 (6.8% cancelled, 2.9% refused at the door) |
| Average order value | ৳45.5K |

The 11.11 event is the biggest growth lever. Daily sales during the 54-hour window run at **4.0x** the
October baseline, and the lift has grown every year: **3.2x in 2022 → 4.9x in 2025**.

### Complication: 11.11 buys volume at the expense of margin

1. **Margin collapses.** 11.11 earns a **0.7%** contribution margin against **10.8%** in normal trading.
   Discounts run at **11.0%** versus 1.2% normally. After ad spend, the event **loses ৳0.75M**.
2. **The best-selling category loses money.** Smartphones are 49% of net sales at a 6.7% margin. During
   11.11 they are 51% of event sales at a **−2.1%** margin, while Accessories hold 21.8%.
3. **54 "margin-trap" SKUs** sell at a 15.6% discount in 11.11 and post a **−5.3%** margin.
   Other SKUs earn +1.9% over the same window.
4. **3 of 5 promotion mechanics lose money.** Only the Hero Flash Drop is margin-positive (2.6%).
   The sitewide voucher, the wallet cashback and the 0% EMI offer are all below breakeven.
5. **Operations break under peak load.**
   - Late deliveries rise from 30% to **68%**.
   - Payment failures reach **11.3%**; Nagad hits 15.3% and bKash 13.8%.
   - Cancellations rise from 6.6% to **11.2%**.
   - Launch night takes 16% of the window's orders in 6 hours, and demand peaks again every evening around 19:00.
6. **Part of the spike is borrowed demand.** Daily sales after the event run **34% below** the October baseline.
7. **The customers it brings are worth less.** Customers acquired in 11.11 repeat at **77%** versus 82%,
   and spend **24% less** per head over their lifetime (৳165K vs ৳217K).
8. **The media mix is inefficient.** Return on ad spend:

   | Channel | ROAS | Note |
   |---|---|---|
   | CRM (SMS & email) | **387x** | |
   | Paid Search | 81x | |
   | Paid Social | 58x | absorbs 58% of spend |
   | Affiliate | 48x | |

   Channels with no media cost (App, Direct, SEO) already deliver **52%** of net sales.

### Resolution: five moves for the next 11.11

| # | Move | Owner | Timing | Value at stake* |
|---|---|---|---|---|
| 01 | Cap discounts on margin-trap SKUs and exclude them from sitewide vouchers | Category Directors | Before Nov 10 launch | ৳1.7M contribution margin |
| 02 | Re-weight promotion funding to margin-positive mechanics | Head of Campaigns | Before Nov 10 launch | ৳0.4M contribution margin |
| 03 | Shift 20% of Paid Social budget into Search and CRM | CMO | Next 30 days | ৳113.6M attributed revenue |
| 04 | Pre-book payment-gateway and courier capacity for launch night and evening peaks | COO, Head of Payments | Before Nov 10 launch | ৳8.8M GMV kept |
| 05 | Convert 11.11 deal-seekers with a 60-day post-event CRM journey | Head of CRM | Nov 13 – Jan 31 | ৳97.7M lifetime value gap |

\*Indicative estimates calculated live in the model. They overlap, so do not add them together.

---

## Dashboard pages

| # | Page | Question it answers |
|---|---|---|
| 1 | **Executive Summary** | How fast are we growing, and what does 11.11 do to margin? |
| 2 | **11.11 Command Center** | How big is the lift, when does demand peak, and which promotions pay? |
| 3 | **Marketing & Channels** | Where does the funnel leak, and which channels earn their spend? |
| 4 | **Product & Margin** | Which categories and SKUs destroy margin during the event? |
| 5 | **Customers & Operations** | Where do delivery, payments and retention break under peak load? |
| 6 | **Strategic Priorities** | What should management do before the next launch, and what is it worth? |
| 7 | *Category Deep Dive* | A hidden drill-through page: right-click any category and choose **Drill through** |

### Interactivity

- **Synced filters** for Year, Event Window, Division and Product Category, plus a one-click **Reset filters** button.
- **Scoped filtering.** Visuals that compare years (the seasonality heatmap, lift by year, hourly and November curves, and the
  cohort chart) deliberately ignore the Year filter, and each says so in its subtitle. Pages whose data has no
  division or product split (marketing spend, payments, delivery) don't offer those filters, so filtered and
  unfiltered numbers never get mixed.
- **Dynamic text.** Headlines, score-card subtitles and chart titles are DAX measures, so the narrative updates with every selection.
- **Navigation.** A side menu moves between pages, "See the evidence" buttons link each recommendation to its proof,
  clicking any chart filters the others, and the category drill-through has a back button.

---

## Semantic model

- **Star schema.** 8 fact tables and 10 dimensions, with single-direction relationships from dimensions to facts and **no fact-to-fact joins**.
- **Role-playing dates.** `DimDate` is the marked date table. Order date is the active relationship everywhere. Payment, return,
  dispatch, delivery and promised dates are inactive and switched on with `USERELATIONSHIP` in dedicated measures.
- **183 measures** in a `_Measures` table, organised into display folders:
  Sales, Profitability, Orders & Customers, Marketing Funnel, 11.11 Campaign, Payments, Delivery, Returns, Inventory,
  Time Intelligence, Dashboard Text and Strategic Priorities.
- **Design choices worth knowing:**
  - Gross merchandise value (all placed demand) is kept separate from delivered net sales and contribution margin.
  - Attributed marketing revenue is never mixed with transactional revenue.
  - Inventory is semi-additive: closing stock is never summed across weeks.
  - Cohort measures always use full customer history, so a late-year cohort isn't cut short.

---

## Getting started

1. Install **Power BI Desktop** (recent version, with PBIP and PBIR support).
2. Clone this repo and open `Kinun Campaign Report.pbip`.
3. Set the data source: **Transform data → Edit parameters → `BigQueryProjectId`**. Enter the Google Cloud project
   that holds a `kinun_dw` dataset with the 18 source tables (`DimDate`, `FactOrderLine`, and so on).
4. Click **Refresh** and sign in to BigQuery with your **own** Google account when prompted.

### Security: what is *not* in this repository

| Excluded | Why |
|---|---|
| Google / BigQuery credentials | Power BI Desktop keeps sign-ins in your local, encrypted credential store, never in project files |
| Data cache (`.pbi/cache.abf`) | Holds the imported data, so it is excluded by `.gitignore` |
| Machine settings (`.pbi/localSettings.json`) | User- and machine-specific, excluded by `.gitignore` |
| Real GCP project ID | Committed as the placeholder `YOUR_GCP_PROJECT_ID` in `definition/expressions.tmdl` |
| `.pbix` files, CSV/XLSX extracts, local backups | Excluded by `.gitignore` |

Commits use a GitHub no-reply e-mail address, so no personal e-mail appears in the history.

---

## Repository structure

```
Kinun Campaign Report.pbip                 # open this in Power BI Desktop
Kinun Campaign Report.SemanticModel/
  definition/
    expressions.tmdl                       # BigQueryProjectId parameter (placeholder)
    relationships.tmdl                     # star-schema relationships
    tables/*.tmdl                          # tables, columns and the 183 measures (_Measures.tmdl)
Kinun Campaign Report.Report/
  definition/pages/*/                      # one folder per page, one visual.json per visual
  StaticResources/RegisteredResources/     # BCG-style theme (KinunBCGExecutive.json) and logo
```

**Tech:** Power BI Desktop (PBIP / PBIR / TMDL) · DAX · Power Query (M) · Google BigQuery
