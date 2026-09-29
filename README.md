# Startup Financial Health & Unit Economics Analysis (E-Commerce Marketplace)

An end-to-end data analytics project analyzing monthly revenue, burn rate, Customer Acquisition Cost (CAC), Customer Lifetime Value (LTV), and cohort retention dynamics for an early-stage e-commerce marketplace using Python and Power BI.

---

## 📌 Project Overview & Objectives

* **Problem Statement:** Early-stage startups and marketplaces often scale top-line Gross Merchandise Value (GMV) while operating under an unsustainable customer acquisition deficit.
* **Objective:** Derive net marketplace unit economics from raw transactional logs, track month-over-month customer retention decay, and determine the exact CAC Payback timeline.
* **Core Takeaway:** Growth via broad top-of-funnel paid acquisition can mask underlying retention cliffs. Strategic intervention requires shifting capital from paid search into automated lifecycle CRM and category-tiered take rates.

---

## 🛠️ Tech Stack & Tools

* **Data Modeling & Feature Engineering:** Python (`pandas`, `numpy`)
* **Business Intelligence & Dashboards:** Power BI Desktop (`DAX`, `Power Query`, Star Schema)
* **Dataset:** [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (96,000+ delivered orders across 2017–2018)

---

## 🏗️ Data Architecture & Modeling

### 1. Financial Engineering (Platform Take-Rate Model)
Public marketplace logs capture transaction item amounts but lack itemized operating costs. The platform financial layer was engineered from transactional events:
* **Total GMV:** Sum of item prices and freight values per delivered order.
* **Platform Net Revenue (16%):** Standard marketplace commission take-rate on GMV.
* **COGS & Variable Operations (4%):** Payment processing gateway fees, chargebacks, and logistics subsidies.
* **Platform Gross Margin (12%):** Net platform margin available to service customer acquisition and operating expenses.

### 2. Unit Economics & Cost Simulation
* **Cohort Base ($M_0$):** Unique customers mapped to their initial purchase month via `customer_unique_id`.
* **Acquisition Spend:** Simulated ad spend baseline:
  $$\text{Monthly Marketing Spend} = \text{R\$} 25,000 + (\text{Acquired Customers} \times \text{R\$} 18)$$
* **Blended CAC:**
  $$\text{CAC} = \frac{\text{Monthly Marketing Spend}}{\text{New Acquired Customers}}$$
* **Cumulative LTV:** Sum of cumulative platform gross profit per cohort divided by initial cohort size.

### 3. Star Schema Data Model
* **`fact_table`:** Transaction-level records (`order_id`, `customer_unique_id`, `cohort_month`, `cohort_index`, `total_gmv`, `gross_margin`).
* **`Dim_Date`:** Date dimension linking chronologically to `fact_table[cohort_month]`.
* **`dim_table`:** Monthly unit economics summary (`Acquired_Customers`, `Simulated_Ad_Spend`, `Blended_CAC`).
* **`_measures`:** Centralized DAX measures repository.

---

## 📊 Key Metrics & Portfolio Performance

| Metric | Dashboard Result | Benchmark Target | Operational Diagnostic |
| :--- | :--- | :--- | :--- |
| **Delivered GMV** | **R$ 15.37M** | N/A (Growth) | Healthy transaction volume across 96k+ completed orders. |
| **Platform Gross Margin** | **R$ 1.84M** | > 10% of GMV | Stable 12% take-rate margin servicing operating overhead. |
| **Overall Blended CAC** | **R$ 23.37** | < R$ 20.00 | Acquisition cost per unique buyer across all channels. |
| **Average Realized LTV** | **R$ 19.81** | $\ge 3.0\times$ CAC | Realized cumulative gross margin contribution per buyer. |
| **Portfolio LTV : CAC** | **0.85x** | **$\ge 3.00x$** | **Deficit Zone (< 1.0x):** Acquisition cost exceeds lifetime margin. |
| **Month 1 Retention** | **0.49% avg** | 15% – 25% (Retail) | Marketplace profile dominated by one-off durable goods. |

---

## 📈 Dashboard Architecture

### Page 1: Executive Overview & Unit Economics Health
* **KPI Header Cards:** Total GMV, Platform Gross Margin, Overall Blended CAC, Average Realized LTV, and LTV:CAC Ratio.
* **Growth & Spend Trajectory (Line & Clustered Column):** Monthly Gross Margin bars benchmarked against monthly Total Acquisition Spend. Highlights early-stage net burn (H1 2017) and scale stabilization (2018).
* **Unit Economics Margin vs. Acquisition (Clustered Column):** Monthly Cohort CAC vs. Realized LTV per user, demonstrating the persistent R$ 2.00–R$ 4.00 unit shortfall across cohorts.

### Page 2: Cohort Retention & Payback Analysis
* **Cohort Retention Matrix (Heatmap):** Dynamic triangular grid tracking active customer % from Month 0 to Month 11 with custom color scaling.
* **Cumulative LTV vs. CAC Payback Curve (Line Chart):** Cumulative margin recovery trajectory plotted against the R$ 23.37 CAC threshold line.

---

## 💡 Key Findings & Strategic Recommendations

1. **The Month 1 Retention Cliff**
   * *Finding:* Customer retention drops from **100% to an average of 0.49% in Month 1** (ranging between 0.18% and 0.72%). Over 99% of customers do not make a second purchase within 30 days.
   * *Reason:* Catalog concentration in durable goods (home decor, furniture, automotive parts) where purchase intervals naturally exceed 18 months.
2. **Extended CAC Payback Horizon**
   * *Finding:* Initial purchase gross margin contribution averages **R$ 19.10** against an acquisition cost of **R$ 23.37**, creating an immediate initial deficit of **-R$ 4.27 per customer**.
   * *Impact:* Due to sub-1% repeat rates, cumulative margin reaches only **R$ 19.81 by Month 12**, extending the full CAC payback timeline to **15.8 months** (exceeding standard 8–12 month venture runway benchmarks).
3. **Actionable Recommendations**
   * **Reallocate 25% of Acquisition Spend:** Cap top-of-funnel unbranded search ads and redirect funds into automated post-purchase CRM email/SMS triggers deployed at Day 14 and Day 45 post-delivery.
   * **Restructure Marketplace Take Rates:** Transition from a flat 16% fee to category-tiered commissions (19%–20% on one-off high-ticket items to clear CAC on order 1; lower commissions on consumables to drive re-order velocity).
   * **Target Milestones:** Compress blended CAC to **R$ 19.50** while accelerating 90-day margin recovery to **R$ 23.50**, cutting CAC payback from **15.8 months down to under 8 months**.

---

## 📂 Repository Structure

```text
├── data/
│   ├── olist_cohort_transactions.csv      # Cleaned fact table with cohort indices
│   └── olist_unit_economics_summary.csv   # Monthly acquisition cost and cohort summary
├── notebooks/
│   └── cohort_financial_modeling.ipynb    # Python ETL, margin derivation & cohort matrix logic
├── dashboard/
│   └── financial_kpi_dashboard.pbix       # Interactive 2-page Power BI report
├── docs/
│   └── executive_summary_report.pdf       # 2-page executive presentation report
└── README.md
