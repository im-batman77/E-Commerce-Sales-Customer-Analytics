# E-Commerce Sales & Customer Analytics

An end-to-end SQL-based business analytics project using **Microsoft SQL Server (T-SQL)** to analyse e-commerce sales performance, customer behaviour, retention, and product performance.

The project uses a normalized relational database containing approximately 5,000 customers, 14,200 orders, and 56,800 order items spanning three years (2022–2024).

The objective is to transform transactional data into actionable business insights that support revenue growth, customer retention, product optimisation, and data-driven decision-making.

---

## Business Objectives

- Analyse monthly and yearly revenue trends.
- Identify high-value customers using RFM segmentation.
- Evaluate customer retention using cohort analysis.
- Identify top-performing and underperforming products.
- Analyse historical customer lifetime value across acquisition channels.
- Identify customer churn patterns.
- Evaluate year-over-year revenue growth.
- Identify revenue-driving products using Pareto and ABC analysis.

---

## Tech Stack

- Microsoft SQL Server
- SQL Server Management Studio (SSMS)
- Transact-SQL (T-SQL)
- Git & GitHub

---

## SQL Techniques Demonstrated

| Technique | Application |
|---|---|
| Joins (INNER JOIN, LEFT JOIN) | Combining relational data |
| Common Table Expressions (CTEs) | Multi-stage analytical queries |
| Subqueries | Intermediate calculations |
| Aggregate functions | Revenue and customer metrics |
| GROUP BY and HAVING | Group-level analysis |
| CASE statements | Conditional aggregation and segmentation |
| ROW_NUMBER() | Top-N product analysis |
| RANK() and DENSE_RANK() | Ranking |
| NTILE() | RFM customer scoring |
| LAG() | Historical comparisons and purchase intervals |
| PERCENT_RANK() | Product performance classification |
| PERCENTILE_CONT() | Customer lifetime value distribution |
| SUM() OVER() | Running totals and cumulative revenue |
| AVG() OVER() | Moving averages |
| DATEADD(), DATEDIFF() | Date-based analysis and retention |
| NULLIF() | Division-by-zero handling |
| Primary and foreign keys | Relational database design |
| Indexing | Query performance optimisation |

---

## Dataset

The project uses a synthetic dataset representing an online retailer operating across 21 cities and four major product departments:

- Electronics
- Home & Kitchen
- Apparel
- Books

### Dataset Overview

| Table | Records | Description |
|---|---:|---|
| customers | 5,000 | Customer information and acquisition channels |
| orders | ~14,200 | Order details, status and payment methods |
| order_items | ~56,800 | Product-level transactional records |
| products | 400 | Product catalogue |
| categories | 22 | Product categories |
| returns | ~4,700 | Returned products and return reasons |
| campaigns | 11 | Marketing campaign expenditure |

**Analysis period:** January 2022 – December 2024

The synthetic dataset incorporates realistic business patterns, including:

- Year-over-year revenue growth.
- Seasonal fluctuations in customer demand.
- Customer churn and repeat purchases.
- Revenue concentration among high-value customers.
- Category-specific product return patterns.

See [Entity Relationship Diagram](docs/ER_diagram.md) for the database structure.

---

## Database Design

The database follows a normalized relational structure to maintain data integrity and support analytical queries.

### Key Relationships

- One customer can place multiple orders.
- One order can contain multiple order items.
- One product can appear in multiple order items.
- One category can contain multiple products.
- An order item may be associated with a return.

Primary keys, foreign keys, constraints and indexes are used to maintain data consistency and support efficient querying.

---

## Key Performance Indicators (KPIs)

| KPI | Calculation |
|---|---|
| Gross Revenue | Quantity × Unit Price |
| Net Revenue | Gross Revenue − Discounts |
| Realized Revenue | Net Revenue − Return Value |
| Gross Profit | Realized Revenue − Adjusted COGS |
| Gross Profit Margin | Gross Profit / Realized Revenue × 100 |
| Average Order Value | Revenue / Total Orders |
| Customer Retention Rate | Retained Customers / Initial Customers × 100 |
| Customer Churn Rate | Churned Customers / Previously Active Customers × 100 |
| Return Rate | Returned Items / Total Sold Items × 100 |
| YoY Growth | (Current Year Revenue − Previous Year Revenue) / Previous Year Revenue × 100 |

---

## Analytical Modules

### 01. Revenue and Sales Analysis

Analyses monthly revenue, order volumes, active customers, return rates, gross profit margins and seasonal trends.

**SQL Techniques:**

- Aggregate functions
- Conditional aggregation
- Window functions
- Moving averages
- LAG()

**Key Findings:**

| Month | Net Revenue | Returned Revenue | Orders |
|---|---:|---:|---:|
| October 2024 | 2,901,828 | 209,708 | 468 |
| November 2024 | 5,690,311 | 450,680 | 950 |
| December 2024 | 6,139,069 | 496,408 | 1,027 |

- November revenue increased by approximately 96.1% compared to October.
- December recorded the highest revenue among the three months shown.
- The results reflect the seasonal demand patterns incorporated into the dataset.

**Business Recommendation:** Align inventory planning, promotional campaigns and operational capacity with anticipated seasonal demand.

[View SQL Query](analysis/01_revenue_overview.sql)

---

### 02. RFM Customer Segmentation

Segments customers based on their historical purchasing behaviour using the RFM framework.

- **Recency:** Number of days since the customer's most recent purchase.
- **Frequency:** Total number of orders placed by the customer.
- **Monetary:** Total customer spending.

Customers are assigned scores ranging from 1 to 5 using the NTILE() window function and classified into actionable customer segments.

**SQL Techniques:**

- Common Table Expressions
- NTILE()
- CASE statements
- Aggregate functions
- Joins

**Key Findings:**

| Customer Segment | Customers | Average Spend | Total Revenue |
|---|---:|---:|---:|
| Champion | 914 | 32,835 | 30,010,918 |
| At Risk | 895 | 19,008 | 17,011,880 |
| Loyal | 450 | 23,083 | 10,387,139 |
| Regular | 1,001 | 8,240 | 8,248,138 |
| Lost | 1,009 | 4,796 | 4,838,686 |
| New / Promising | 494 | 4,813 | 2,377,615 |
| **Total** | **4,763** | — | **72,874,376** |

- Approximately 19.2% of purchasing customers contribute 41.2% of RFM revenue.
- At-risk customers represent approximately 23.3% of historical RFM revenue.
- Customer value is unevenly distributed across the customer base.

**Business Recommendations:**

| Segment | Recommended Strategy |
|---|---|
| Champions | Exclusive rewards and loyalty programmes |
| Loyal | Cross-selling and upselling |
| At Risk | Personalised reactivation campaigns |
| Regular | Increase purchase frequency |
| Lost | Cost-effective win-back campaigns |
| New / Promising | Second-purchase incentives |

[View SQL Query](analysis/02_customer_rfm_segmentation.sql)

---

### 03. Cohort Retention Analysis

Groups customers based on their first purchase month and measures repeat purchase behaviour over subsequent months.

**SQL Techniques:**

- Common Table Expressions
- DATEDIFF()
- First purchase identification
- COUNT(DISTINCT)
- Conditional aggregation

**Key Findings:**

| Cohort | Customers | M0 | M1 | M2 | M3 | M6 | M12 |
|---|---:|---:|---:|---:|---:|---:|---:|
| January 2022 | 26 | 100% | 23.1% | 15.4% | 19.2% | 11.5% | 7.7% |
| April 2022 | 70 | 100% | 27.1% | 18.6% | 15.7% | 18.6% | 5.7% |

- The illustrated cohorts experience a significant decline in customer engagement after the first purchase.
- Long-term retention remains relatively low.
- Improving the first-to-second purchase conversion represents a potential growth opportunity.

**Business Recommendation:** Introduce personalised recommendations, post-purchase engagement and second-order incentives.

[View SQL Query](analysis/03_cohort_retention.sql)

---

### 04. Product Performance Analysis

Evaluates individual product performance based on sales volume, revenue, profitability and returns.

Products are classified within their respective categories into:

- Top-performing products — top 10%.
- Regular-performing products.
- Underperforming products — bottom 10%.

**SQL Techniques:**

- PERCENT_RANK()
- Aggregate functions
- Common Table Expressions
- LEFT JOIN
- CASE statements

**Business Applications:**

- Identify revenue-driving products.
- Improve inventory allocation.
- Identify slow-moving products.
- Evaluate product profitability.
- Support product assortment decisions.

[View SQL Query](analysis/04_product_performance.sql)

---

### 05. Top-N Product Analysis

Identifies the highest revenue-generating products within each category and year.

**SQL Techniques:**

- ROW_NUMBER()
- PARTITION BY
- Common Table Expressions
- Aggregate functions

**Business Applications:**

- Identify best-selling products.
- Compare category-level performance.
- Prioritise high-performing products in marketing campaigns.
- Support merchandising and inventory decisions.

[View SQL Query](analysis/05_top_n_per_group.sql)

---

### 06. Customer Lifetime Value (CLV) Analysis

Evaluates historical customer spending and purchasing behaviour across different acquisition channels.

The analysis considers:

- Total historical customer spending.
- Average order value.
- Purchase frequency.
- Average interval between purchases.
- Median and 90th percentile customer value.

**SQL Techniques:**

- LAG()
- PERCENTILE_CONT()
- DATEDIFF()
- Common Table Expressions
- Aggregate functions

**Key Findings:**

| Acquisition Channel | Customers | Average Historical CLV | Average Order Value |
|---|---:|---:|---:|
| Display | 434 | 20,098.36 | 6,093.37 |
| Social | 316 | 16,501.25 | 6,219.48 |
| Referral | 3,731 | 16,253.09 | 6,074.93 |

- Display-acquired customers demonstrate the highest average historical spending among the channels shown.
- Referral contributes the largest customer base among the channels shown.
- Customer acquisition cost and profitability should also be evaluated before making marketing budget decisions.

**Business Recommendation:** Combine customer lifetime value with acquisition cost and contribution margin to evaluate marketing channel efficiency.

The CLV metric in this project represents historical spending rather than predicted future customer lifetime value.

[View SQL Query](analysis/06_customer_ltv.sql)

---

### 07. Customer Churn Analysis

Measures quarter-over-quarter customer churn by identifying customers who purchased during one quarter but did not purchase during the following quarter.

**SQL Techniques:**

- Common Table Expressions
- Subqueries
- Date functions
- Conditional aggregation
- Joins

**Business Applications:**

- Identify declining customer engagement.
- Evaluate customer retention strategies.
- Support targeted reactivation campaigns.
- Explore relationships between marketing expenditure and customer churn.

Customers classified as churned in a particular quarter may return in subsequent quarters.

[View SQL Query](analysis/07_churn_analysis.sql)

---

### 08. Year-over-Year Growth Analysis

Evaluates annual revenue growth across product categories to identify high-growth and underperforming departments.

**SQL Techniques:**

- Conditional aggregation
- CASE statements
- YEAR()
- NULLIF()
- Common Table Expressions

**Business Applications:**

- Compare annual revenue performance.
- Identify growing and declining categories.
- Support resource allocation.
- Evaluate long-term business performance.

[View SQL Query](analysis/08_yoy_growth.sql)

---

### 09. Pareto and ABC Analysis

Evaluates revenue concentration across products using the Pareto principle (80/20 rule).

Products are ranked according to their revenue contribution, and cumulative revenue percentages are calculated using window functions.

| Classification | Cumulative Revenue Contribution | Priority |
|---|---|---|
| Class A | Up to 80% | High |
| Class B | Above 80% and up to 95% | Medium |
| Class C | Above 95% | Low |

**SQL Techniques:**

- SUM() OVER()
- Running totals
- Window functions
- CASE statements
- Common Table Expressions

**Business Applications:**

- Identify the most important revenue-generating products.
- Prioritise inventory investment.
- Optimise product assortment.
- Allocate marketing resources more effectively.

[View SQL Query](analysis/09_running_totals_pareto.sql)

---

## Key Business Insights

| Finding | Business Recommendation |
|---|---|
| Champions contribute approximately 41.2% of RFM revenue | Prioritise loyalty and relationship management |
| At-risk customers contribute approximately 23.3% of historical RFM revenue | Implement targeted reactivation campaigns |
| November revenue increased approximately 96.1% over October | Prepare inventory and operational capacity for seasonal demand |
| Long-term cohort retention is relatively low | Focus on repeat purchase conversion |
| Display customers demonstrate higher average historical spending among the channels shown | Evaluate acquisition cost and profitability before increasing spend |
| Referral contributes a large customer base | Evaluate and strengthen referral programmes |
| Products contribute unevenly to total revenue | Prioritise high-contribution products using ABC analysis |

---

## Repository Structure

```
E-Commerce-Sales-Customer-Analytics/
├── README.md
├── run_all.sql
│
├── schema/
│   ├── 01_create_tables.sql
│   └── 02_indexes.sql
│
├── data/
│   ├── 01_seed_reference_data.sql
│   └── 02_seed_transactions.sql
│
├── analysis/
│   ├── 01_revenue_overview.sql
│   ├── 02_customer_rfm_segmentation.sql
│   ├── 03_cohort_retention.sql
│   ├── 04_product_performance.sql
│   ├── 05_top_n_per_group.sql
│   ├── 06_customer_ltv.sql
│   ├── 07_churn_analysis.sql
│   ├── 08_yoy_growth.sql
│   └── 09_running_totals_pareto.sql
│
└── docs/
    └── ER_diagram.md
```

---

## Getting Started

### Prerequisites

- Microsoft SQL Server
- SQL Server Management Studio (SSMS)

### Step 1: Clone the Repository

```
git clone https://github.com/im-batman77/E-Commerce-Sales-Customer-Analytics.git
```

### Step 2: Create the Database

Open SQL Server Management Studio and execute:

```sql
CREATE DATABASE EcommerceAnalytics;
GO

USE EcommerceAnalytics;
GO
```

### Step 3: Create Tables and Indexes

Execute the following scripts in sequence:

```
schema/01_create_tables.sql
schema/02_indexes.sql
```

### Step 4: Populate the Database

Execute the data generation scripts:

```
data/01_seed_reference_data.sql
data/02_seed_transactions.sql
```

### Step 5: Run Analytical Queries

Open and execute individual SQL scripts available in the analysis directory.

**Note:** These setup instructions describe the target SQL Server implementation. The migrated scripts must be validated in SQL Server before the setup can be considered fully tested.

---

## Assumptions and Limitations

- The dataset is synthetic and does not contain real customer information.
- Cancelled orders are excluded from revenue calculations.
- Revenue is calculated at the order-item level.
- Partial-quantity returns are not considered.
- Customer lifetime value represents historical spending rather than predicted future profitability.
- Customer segmentation is rule-based rather than machine learning-based.
- Revenue definitions may differ across analytical modules depending on the treatment of discounts and returns.
- Business recommendations are derived from descriptive analysis and have not been validated through real-world experiments.
- The numerical findings presented are from the original dataset analysis. SQL Server migration and result parity have not yet been independently verified.

---

## Conclusion

This project demonstrates the application of SQL to solve practical business analytics problems, including customer segmentation, revenue analysis, customer retention, churn identification, and product performance evaluation.

The analysis highlights how transactional data can be transformed into actionable insights to support business growth, customer engagement, inventory optimisation, and strategic decision-making.

---

## Author

**Balram Dhaker**

National Institute of Technology Rourkela

[GitHub](https://github.com/im-batman77)
