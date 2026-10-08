## 🏢 1. Business Background

**NovaMart** is a fast-growing, multi-vendor e-commerce marketplace connecting thousands of independent sellers with consumers nationwide. Over its tracked operational history, the platform processed over **100,000 transactional records** (~96,000 unique customers) and generated approximately **$16 Million in gross revenue**.

While top-line customer acquisition scaled rapidly, leadership lacked visibility into post-purchase customer behavior, repeat purchase velocity, and logistical execution. The underlying data sat fragmented across **9 relational operational tables**—covering orders, line items, multi-installment payments, customer locations, product catalogs, seller profiles, and post-delivery customer reviews—making it impossible for marketing and operations teams to track true customer lifetime value (CLV).

---

## ⚠️ 2. Problem Statement

Despite strong initial revenue generation, NovaMart faced a critical long-term sustainability crisis: **a massive 97% customer churn rate**. Roughly 97% of buyers purchased a single item and never returned to the platform.

Management lacked visibility into the root cause of this single-order behavior:
* **Was it a widespread service or platform failure?**
* **Was it driven by poor product quality in specific categories?**
* **Were long shipping delays destroying brand trust?**
* **Or was it a complete lack of targeted post-purchase lifecycle marketing?**

**Goal of the Project:**  
Engineer an end-to-end data pipeline (Excel ➡️ PostgreSQL ➡️ Power BI) to clean and model the ~100,000 transaction dataset, isolate the operational root causes of the 97% churn rate, and build an interactive **RFM (Recency, Frequency, Monetary) Segmentation Tool** that allows marketing strategists and operations managers to immediately extract and re-engage high-value or at-risk customers.

---

## 📈 3. Executive Summary

By consolidating 9 raw datasets in **PostgreSQL** and deploying a **Power BI Star Schema** powered by custom **DAX RFM scoring and delivery binning**, this project uncovered three game-changing insights for NovaMart:

* **Marketing Optimization (The 4.09/5 Discovery):** Analysis of review distributions proved that the **97% churn rate was NOT caused by widespread service failure**—in fact, NovaMart's baseline customer satisfaction is high at **4.09 / 5 stars**. Most customers had a positive first experience but simply never returned due to a lack of targeted lifecycle marketing.
* **Logistics & Strategy (The 21-Day Cliff):** While baseline satisfaction is strong, extreme delivery delays destroy brand trust. Using custom 7-day DAX delivery bins, the dashboard proved that orders delivered within the first week achieve an average review score of **4.41 / 5**, whereas once shipping exceeds **21 days**, average review scores plummet to **2.37 / 5**.
* **Product & Vendor Quality:** Granular category analysis identified specific underperforming product verticals—most notably **Party Supplies** and **Fashion**—that consistently generate the lowest customer ratings, giving procurement teams concrete data to renegotiate or drop poor vendors.
* **Operational Execution via the RFM Export Matrix:** Instead of static charts, Page 2 features an operational **RFM Export Matrix** filtered by dynamic DAX segment buttons (`Champions`, `Loyal`, `At-Risk`, `Lost/Hibernating`). Marketing teams can right-click and export targeted `customer_unique_id` lists in seconds for email and retention campaigns.

---

## 🛠️ 4. Tech Stack & Data Source

### Tech Stack
* 📊 **Power BI Desktop:** Main data visualization platform used for interactive report creation, cross-filtering, and synchronized time-intelligence slicing.
* 🗃️ **PostgreSQL:** Data extraction, cleaning, and transformation layer using custom SQL views and CTEs to resolve multi-payment installment anomalies and map order-level IDs to unique customers.
* 🧠 **DAX (Data Analysis Expressions):** Authored calculated KPI measures, dynamic 1–5 RFM percentile scoring, conditional segment assignment, and 7-day shipping duration bins.
* 📑 **Microsoft Excel:** Initial data profiling, schema auditing, and null/outlier verification.
* 🗂️ **Data Modeling:** Engineered a **Star Schema** connecting a centralized Master Fact View with multiple Dimension tables (`Customers`, `Products`, `Dates`, `Geolocation`).
* 📄 **File Formats:** `.pbix` for report development, `.sql` for transformation scripts, and `.png` / `.pdf` for dashboard previews and technical documentation.

### Data Source
* **Source:** Olist E-Commerce Public Dataset
* **Scope:** ~100,000 transactional records across 9 interconnected tables detailing customer locations, order timestamps (purchase, approval, carrier handover, customer delivery), product categories, payment installments, and post-delivery customer review scores (1 to 5 stars).

---

## 🎯 5. Key Metrics Framework (North Star & Guardrails)

To ensure marketing and operational interventions drive sustainable growth without sacrificing customer experience, the dashboard tracks a balanced hierarchy of **North Star**, **Driver**, and **Guardrail** metrics:

| Metric Type | Metric Name | Definition / Logic | Business Purpose |
| :--- | :--- | :--- | :--- |
| **North Star Metric (NSM)** | **Repeat Purchase Rate & Active RFM Cohort Share** | Percentage of `customer_unique_id` with 2+ orders and revenue share in `Champions` / `Loyal` tiers. | Measures NovaMart's transition from one-time transactions to recurring customer lifetime value (CLV). |
| **Primary Driver KPI** | **Total Revenue (Monetary)** | Sum of total payment values (~$16M baseline). | Tracks top-line financial scale across time, regions, and segments. |
| **Primary Driver KPI** | **Average Order Value (AOV)** | `Total Revenue / Total Unique Orders` | Measures basket size and spending power across RFM cohorts. |
| **Primary Driver KPI** | **Recency (Days Since Last Order)** | `Max Dataset Date - Latest Customer Purchase Date` | Evaluates customer warmth and urgency for win-back campaigns. |
| **Guardrail Metric** | **Baseline Customer Review Score (CSAT)** | Average review rating on a 1–5 scale (**4.09 / 5** baseline). | Ensures growth initiatives do not degrade product or buyer satisfaction. |
| **Guardrail Metric** | **21+ Day Delivery Breach Rate** | % of orders taking more than 21 days to reach the customer (where CSAT drops to **2.37 / 5**). | Protects brand trust by flagging severe carrier and seller fulfillment bottlenecks. |
| **Guardrail Metric** | **Customer Churn Rate** | % of unique buyers who never place a second order (**~97%** baseline). | Tracks the core retention leak across cohorts. |

---

## 🔍 6. Steps Taken in Analysis

### Step 1: Data Profiling & Schema Auditing (Excel)
* Audited all 9 source tables to understand primary/foreign key relationships and data grain.
* Uncovered a critical identifier trap: `customer_id` is generated uniquely per *order*, whereas `customer_unique_id` tracks the actual *individual customer*. Standardizing the analysis on `customer_unique_id` enabled accurate repeat-buyer tracking.

### Step 2: Relational Data Cleaning & Aggregation (PostgreSQL)
* Built custom PostgreSQL views to pre-aggregate the `order_payments` table at the `order_id` level before joining with `orders` and `order_items`. This prevented multi-installment credit card payments from duplicating order revenue (fan-out trap).
* Standardized English product category names via the translation table and calculated order-level delivery lead times (`order_delivered_customer_date - order_purchase_timestamp`).

### Step 3: Star Schema Modeling & DAX Engineering (Power BI)
* Connected the clean PostgreSQL Master Fact View to Dimension tables (`Dim_Customer`, `Dim_Product`, `Dim_Date`) in a **1-to-Many Star Schema** for high-performance cross-filtering.
* Built synchronized global dropdown slicers (**Year, Quarter, Month**) across all three report pages.
* Created DAX measures for core KPIs, weekly delivery speed bins (`0–7 Days`, `8–14 Days`, `15–21 Days`, `22+ Days`), and the customer-level RFM scoring matrix.

### Step 4: Diagnostic & Operational Cross-Analysis
* Evaluated revenue concentration across RFM segments, tested the correlation between delivery speed bins and review ratings, and ranked product categories by average customer satisfaction.

---

## 🧪 7. Hypothesis Formulation & Testing

During the investigation, three core hypotheses were formulated and tested against the data:

### Hypothesis 1: Is the 97% Churn Caused by Poor Platform Experience?
* **Hypothesis:** *Customers are leaving after their first purchase because they are fundamentally unhappy with NovaMart’s overall service.*
* **Test:** Analyzed the distribution of 1-star to 5-star reviews across all ~100,000 orders.
* **Verdict (Rejected):** The platform-wide average review score is **4.09 / 5**, with 5-star and 4-star ratings dominating the distribution. The 97% churn is **not** a blanket service failure—it is a **lifecycle marketing failure** where satisfied first-time buyers are never re-engaged.

### Hypothesis 2: Do Extreme Shipping Delays Destroy Brand Trust?
* **Hypothesis:** *Orders that experience extended shipping timelines suffer a sharp drop in customer satisfaction, poisoning repeat purchase intent.*
* **Test:** Mapped Average Review Score against custom 7-day delivery duration bins (`0–7 Days`, `8–14 Days`, `15–21 Days`, `22+ Days`).
* **Verdict (Validated):** Orders delivered within `0–7 Days` earn an impressive **4.41 / 5** average review score. Satisfaction remains resilient up to 14 days, softens between 15–21 days, and **collapses to 2.37 / 5 once delivery exceeds 21 days**.

### Hypothesis 3: Are Specific Product Verticals Dragging Down Satisfaction?
* **Hypothesis:** *Certain product categories suffer from systematic quality or fulfillment issues regardless of general platform health.*
* **Test:** Ranked product categories by order volume, return/cancellation behavior, and mean review score.
* **Verdict (Validated):** Categories such as **Party Supplies** and **Fashion** consistently yielded the lowest review scores, highlighting targeted vendor quality gaps for procurement teams to address.

---

## 🧮 8. Why We Chose RFM & Calculation Methodology

### How We Reached the Conclusion to Track RFM
During exploratory analysis, a major strategic question emerged: **If 97% of customers only buy once, how can the marketing team prioritize who to target first without blowing their budget?**

Relying on **Frequency** alone was useless because 97% of the ~96,000 customers looked identical (1 order). To break that tie and segment the customer base meaningfully, we needed a three-dimensional behavioral lens:
1. **Recency (R):** Allowed us to separate customers who bought *recently* (still warm, high probability of converting to a second purchase) from those who bought *over a year ago* (cold, already churned).
2. **Frequency (F):** Allowed us to isolate and protect the elite ~3% repeat buyer cohort driving recurring revenue.
3. **Monetary (M):** Allowed us to separate high-spending customers (worth investing margin and personalized incentives into) from low-ticket bargain hunters.

### High-Level Calculation Methodology
*(Note: Full SQL scripts and exact DAX formulas for the RFM scoring engine are documented in the project's accompanying Technical Documentation `.pdf`).*

1. **Customer-Level Aggregation (PostgreSQL):** Consolidated order histories at the `customer_unique_id` grain to establish three base metrics for every buyer:
   * **Recency:** Days elapsed between the customer's latest purchase date and the dataset's maximum reference date.
   * **Frequency:** Total count of distinct delivered orders placed by that unique customer.
   * **Monetary:** Total lifetime payment value across all delivered orders for that customer.
2. **Dynamic 1–5 Scoring & Segment Mapping (Power BI DAX):** Ranked each customer on a scale of **1 to 5** across Recency, Frequency, and Monetary thresholds, and applied conditional branching logic to group combined RFM profiles into actionable marketing cohorts (`Champions`, `Loyal Customers`, `Potential Loyalists`, `New / Promising`, `Need Attention`, `At-Risk`, and `Lost / Hibernating`).

---

## 🖥️ 9. Dashboard Walkthrough & Screenshots

The report is structured into three specialized pages connected by **Synchronized Global Slicers (Year, Quarter, Month)**:

### Page 1: Executive Sales Overview
Provides high-level monitoring of Total Revenue (~$16M), Total Orders, Unique Customers (~96K), Average Order Value (AOV), seasonal revenue trends, and regional demand distribution.

![Executive Overview](https://github.com/YashBaghel-Analyst/Customer-Retention-RFM-Analytics-/blob/main/Page%201%20Executive%20Overview.png)

---

### Page 2: RFM Customer Segmentation
Features interactive DAX segmentation buttons alongside the **RFM Export Matrix (Table Visual)**—displaying individual `customer_unique_id`, Recency, Frequency, and Monetary metrics so marketing teams can right-click and export targeted customer lists for email campaigns.

![RFM Segmentation](https://github.com/YashBaghel-Analyst/Customer-Retention-RFM-Analytics-/blob/main/Page%202%20RFM%20Customer%20Segmentation.png)

---

### Page 3: Operational Insights
Highlights post-purchase logistics and product satisfaction, featuring the **Avg. Review Score by Delivery Speed (Binned Bar Chart)** (proving the drop from **4.41 to 2.37** after 21 days), the **Review Score Distribution (Column Chart)** (**4.09/5** baseline), and category-level quality rankings.

![Operational Insights](https://github.com/YashBaghel-Analyst/Customer-Retention-RFM-Analytics-/blob/main/Page%203%20Operational%20Insights.png)

---

## 💡 10. Strategic Recommendations & Action Plan

1. **Automated "Second-Order Bounce-Back" Marketing (Fixing the 97% Churn):**
   * Since baseline satisfaction is high (**4.09/5**), customers aren't angry—they simply lack a reason to return. Marketing should use the **RFM Export Matrix** to extract recent high-CSAT single-order buyers (`Potential Loyalists` & `New / Promising`) and trigger an automated 10–15% second-purchase incentive **7 days after delivery**.
2. **Targeted Win-Back Campaigns for `At-Risk` & `Lost/Hibernating` Tiers:**
   * Export high-monetary dormant users from Page 2 for personalized re-engagement emails while excluding low-spend churned users from paid ad retargeting to preserve marketing ROI.
3. **Enforce a Strict 21-Day Maximum Delivery SLA:**
   * Because review scores crash from **4.41 down to 2.37** past the 21-day mark, operations must audit carrier lanes and sellers falling into the `22+ Days` bin. Implement automated Day-15 transit alerts to issue proactive customer apologies and small store credits *before* day 21 is breached.
4. **Vendor Audits in Low-Performing Categories:**
   * Procurement teams should review merchant quality SLAs in **Party Supplies** and **Fashion**, requiring stricter product descriptions and quality checks from sellers with recurring 1- and 2-star reviews.

---

## 🏁 11. Conclusion

The **NovaMart Customer Retention & RFM Analytics** project bridges the gap between raw database engineering and executive decision-making. By resolving multi-table aggregation anomalies in **PostgreSQL** and building a dynamic **Power BI Star Schema**, the analysis disproved the assumption that NovaMart's 97% churn was a platform-wide service failure (**4.09/5 CSAT**). Instead, it isolated the exact logistical breaking point (**21+ days shipping dropping ratings from 4.41 to 2.37**) and equipped the marketing organization with a self-serve **RFM Export Matrix** to systematically turn one-time shoppers into repeat, high-lifetime-value customers.

---

### 👤 Author
**Yash Baghel**  
*Data Analyst | SQL • Power BI • DAX • Python • Excel*  
🔗 [GitHub Profile](https://github.com/YashBaghel-Analyst)
