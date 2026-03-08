# Customer-Retention-RFM-Analytics-

### 1. Customer-Retention-RFM-Analytics-
A dynamic, interactive data visualization tool built to explore e-commerce customer behavior—focusing on RFM segmentation, revenue trends, and operational bottlenecks.

### 2. Short Description 
The NovaMart Analytics Dashboard is a visually engaging and analytical Power BI report designed to help users explore a massive e-commerce dataset of over 100,000 transactions. The dashboard focuses on highlighting customer purchasing patterns, evaluating shipping logistics, and segmenting users based on Recency, Frequency, and Monetary (RFM) values. This tool is intended for use by marketing strategists, operations analysts, and data-driven managers who seek to understand customer retention and churn.

### 3. Tech Stack
The dashboard was built using the following tools and technologies:
* 📊 **Power BI Desktop** - Main data visualization platform used for report creation.
* 🗃️ **PostgreSQL** - Data extraction, cleaning, and transformation layer using custom views to aggregate multi-payment anomalies.
* 🧠 **DAX (Data Analysis Expressions)** - Used for calculated measures, dynamic RFM scoring, and conditional binning logic.
* 🗂️ **Data Modeling** - Established a Star Schema connecting a centralized Master Fact view with multiple Dimension tables (customers, products, dates) to enable cross-filtering.
* 📄 **File Format** - `.pbix` for development and `.png`/`.pdf` for dashboard previews.

### 4. Data Source
Source: Olist E-Commerce Public Dataset

Data on ~100,000 transactional records, including comprehensive details on customer locations, order timestamps, product categories, payment installments, and a complementary table with customer review scores from 1 to 5.

### 5. Features / Highlights

* **Business Problem**
NovaMart generated high initial revenue, yet faced a critical long-term sustainability issue: a massive 97% customer churn rate. The vast majority of users bought a single item and never returned to the platform. Management lacked visibility into whether this was caused by poor customer service, bad product quality, or a lack of lifecycle marketing.

* **Goal of the dashboard**
To engineer an end-to-end data pipeline to clean the raw data, isolate the root causes of the 97% customer churn, and build an interactive RFM segmentation tool that the marketing team can use to instantly identify and re-engage high-value or at-risk customers.

* **Walk through of key visuals (briefly!)**
  * **Interactive Slicers (Dropdowns)**
  Synchronized global slicers for Year, Quarter, and Month allow stakeholders to filter all three report pages simultaneously for targeted time-intelligence reporting.
  * **RFM Export Matrix (Table Visual)**
  An operational table visual displaying individual `customer_unique_id`, `Recency`, `Frequency`, and `Monetary` values, dynamically filtered by custom DAX segmentation buttons (e.g., "Champions," "At-Risk").
  * **Avg. Review Score by Delivery Speed (Binned Bar Chart)**
  A horizontal bar chart displaying average customer review scores mapped against custom weekly delivery "bins" (0-7 Days, 8-14 Days, etc.). This helps identify exact logistical breaking points.
  * **Review Score Distribution (Column Chart)**
  Displays the total volume of 1-star through 5-star reviews to establish the baseline customer satisfaction average.

* **Business Impact & Insights**
  * **Marketing Optimization:** Discovered that the 97% churn was not a widespread service failure (baseline satisfaction was high at 4.09/5). The real issue was a lack of targeted marketing. The new RFM Export Matrix allows marketing teams to instantly right-click and extract lists of "Lost/Hibernating" customers to run targeted email campaigns.
  * **Logistics & Strategy:** Operational analysis proved that extreme delivery delays destroy brand trust. Using DAX binning, the dashboard revealed that when shipping exceeds 21 days, average review scores plummet from 4.41 down to 2.37.
  * **Product Analysis:** Identified specific low-performing product categories (e.g., party supplies, fashion) that consistently yield the worst customer reviews, allowing procurement teams to adjust vendor relations.

### 6. Screenshots / Demos

Page 1: Executive Sales Overview
![Executive Overview](Link_to_your_Page1_image.png)

Page 2: RFM Customer Segmentation
![RFM Segmentation](Link_to_your_Page2_image.png)

Page 3: Operational Insights
![Operational Insights](Link_to_your_Page3_image.png)
