# Customer Purchase Analysis - SQL Window Functions Practice

## Project Overview

This project analyzes customer purchase behavior and branch performance for **Nova Retail Co.** using SQL window functions, date functions, and advanced querying techniques. The analysis covers 4,021 transactions from 450 customers across 7 product categories and 4 retail branches between 2020 and 2026.

**Purpose:** Practice and demonstrate SQL window functions, CTEs (Common Table Expressions), and date-time operations for data analysis.

---

## Dataset

### Tables Used

**customer_purchases**
- 4,021 rows | 450 unique customers
- Columns: transaction_id, customer_id, customer_name, purchase_date, product_category, channel, purchase_value
- Date range: 2020-01-01 to 2026-07-17
- Note: Date column contains dirty/mixed formats (multiple date formats, nulls, and invalid dates)

**branches**
- 4 retail locations
- Columns: transaction_id, branch_id, branch_name, region
- Linked to purchases via transaction_id

### Data Characteristics
- Purchase values include negative entries (refunds/returns)
- Dates in mixed formats requiring cleaning
- Some single-purchase customers (39 out of 450)
- Uneven transaction distribution across branches and time periods

---

## Analysis Queries

### 1. Customer Lifetime Value (CLV) Ranking

**Purpose:** Rank customers by total spending overall and within product category

**Key Techniques:** GROUP BY, DENSE_RANK() window function

**Output:** Each customer ranked by revenue (1 = highest spender) with separate ranking per product category

```
customer_id | customer_name | total_spent | overall_rank | product_category | category_rank
CUST0205    | Noor Hassan   | 1338.59     | 2            | Electronics      | 1
CUST0205    | Noor Hassan   | 1338.59     | 2            | Fashion          | 2
```

**Business Insight:** Identifies top customers and their preferred categories

---

### 2. Customer Tenure Analysis

**Purpose:** Calculate how long each customer has been active (first purchase to last purchase)

**Key Techniques:** FIRST_VALUE(), LAST_VALUE() with RANGE BETWEEN, AGE(), EXTRACT()

**Output:** Customer tenure in years, months, and days + total lifetime spending

```
customer_id | total_spent | first_order | last_order  | tenure_years | tenure_months | tenure_days
CUST0001    | 450.25      | 2020-01-11  | 2021-09-19  | 1            | 8             | 21
```

**Business Insight:** Shows customer loyalty duration and lifetime value relationship

---

### 3. Gap Analysis - Days Between Purchases

**Purpose:** Calculate waiting days between consecutive purchases per customer

**Key Techniques:** LAG() window function, date arithmetic

**Output:** Previous purchase date and days gap for every transaction

```
customer_id | purchase_date | previous_purchase_date | days_gap
CUST0001    | 2020-12-22    | 2020-05-10             | 226
CUST0001    | 2021-04-15    | 2020-12-22             | 114
```

**Business Insight:** Tracks purchase frequency changes and buying patterns

---

### 4. Churn Risk Detection

**Purpose:** Identify customers at risk of leaving based on inactivity

**Key Techniques:** LAG(), MAX() OVER, date comparisons, CASE statements

**Output:** Risk assessment (No risk / Loose risk / Extreme Loose Risk) based on comparison between current gap and historical average

```
customer_id | last_purchase_date | average_gap | days_since_last_purchase | risk_assessment
CUST0001    | 2021-09-19         | 150         | 280                      | Extreme Loose Risk
CUST0234    | 2026-06-15         | 90          | 45                       | No risk
```

**Business Insight:** Prioritizes which customers need re-engagement campaigns

---

### 5. Monthly Revenue Trend

**Purpose:** Track total revenue by month to identify sales trends

**Key Techniques:** DATE_TRUNC(), GROUP BY month

**Output:** Monthly sales totals

```
gathered_monthly | purchase_month | total_sales
2020-01-01       | January        | 5,234.50
2020-02-01       | February       | 4,821.30
```

**Business Insight:** Shows seasonal patterns and overall business health

---

### 6. Month-over-Month (MoM) Growth %

**Purpose:** Calculate percentage growth from previous month

**Key Techniques:** LAG() for previous month sales, arithmetic for percentage

**Output:** Each month with previous month sales and growth percentage

```
gathered_monthly | total_sales | previous_month_sales | mom_growth_percentage
2020-02-01       | 4,821.30    | 5,234.50             | -8.25%
2020-03-01       | 5,456.75    | 4,821.30             | 12.40%
```

**Business Insight:** Identifies growth/decline trends month by month

---

### 7. 3-Month Moving Average

**Purpose:** Smooth revenue data to identify true trends (removes monthly noise)

**Key Techniques:** AVG() OVER with ROWS BETWEEN 2 PRECEDING AND CURRENT ROW

**Output:** Average revenue smoothed over 3-month windows

```
month      | average_monthly_sales | moving_3_average
2020-01    | 1,241.50              | 1,241.50
2020-02    | 1,205.33              | 1,215.61
2020-03    | 1,364.19              | 1,270.34
```

**Business Insight:** Shows true revenue trend without monthly fluctuations

---

### 8. Year-over-Year (YoY) Growth %

**Purpose:** Compare annual performance to identify long-term growth

**Key Techniques:** EXTRACT(YEAR), LAG() for previous year

**Output:** Annual sales with growth percentage from previous year

```
year | total_sales | previous_year_sales | yoy_growth_percentage
2020 | 52,340.80   | NULL                | NULL
2021 | 48,750.20   | 52,340.80           | -6.86%
2022 | 51,280.50   | 48,750.20           | 5.19%
```

**Business Insight:** Shows long-term business performance and sustainability

---

## Branch Performance Analysis

### 9. Branch Sales Ranking

**Purpose:** Rank branches by total revenue across all time

**Key Techniques:** SUM() OVER PARTITION BY branch, DENSE_RANK()

**Output:** Total sales per branch with overall ranking

```
branch_id | branch_name      | region | total_sales | ranking
BR01      | Downtown         | Central| 12,450.30   | 1
BR02      | Uptown Mall      | North  | 8,320.15    | 2
BR03      | Riverside Center | East   | 7,890.40    | 3
BR04      | Airport Plaza    | West   | 6,125.85    | 4
```

**Business Insight:** Shows which locations drive most revenue

---

### 10. Best-Performing Branch Per Month

**Purpose:** Identify the top-selling branch each month

**Key Techniques:** DATE_TRUNC(), DENSE_RANK() with PARTITION BY month

**Output:** Best branch for each month

```
branch_id | branch_name | purchase_month | total_sales | monthly_ranking
BR01      | Downtown    | 2020-01-01     | 666.77      | 1
BR03      | Riverside   | 2020-02-01     | 307.40      | 1
BR02      | Uptown Mall | 2020-03-01     | 468.05      | 1
```

**Business Insight:** Identifies which locations perform best in different periods

---

### 11. Customer's Favorite and Least Favorite Branch

**Purpose:** Show which branch each customer prefers based on purchase frequency

**Key Techniques:** COUNT() transactions per branch, DENSE_RANK(), MAX() for least preferred, CASE statement

**Output:** Each customer's preferred and least preferred shopping location

```
customer_name | branch_name      | branch_preference
Ahmed Ali     | Downtown         | Favourite Branch
Ahmed Ali     | Airport Plaza    | Least Favourite Branch
Fatima Hassan | Uptown Mall      | Favourite Branch
```

**Business Insight:** Reveals customer location preferences for targeted marketing

---

## Query Performance & Techniques Used

| Technique | Queries | Purpose |
|-----------|---------|---------|
| Window Functions (RANK, DENSE_RANK) | 1, 4, 9, 10, 11 | Ranking within groups |
| LAG() | 3, 4, 6, 8 | Compare current vs previous row |
| FIRST_VALUE / LAST_VALUE | 2 | Get first and last dates |
| DATE_TRUNC | 5, 6, 7, 8, 10 | Group by time periods |
| AGE() / EXTRACT() | 2 | Calculate time differences |
| CTEs (WITH clauses) | All | Organize complex logic |
| PARTITION BY | All | Segment data by groups |

---

## Files Included

- `postgresql_session.sql` - All SQL queries used for analysis
- `customer_purchases.csv` - Raw transaction data (4,021 rows)
- `branches.csv` - Branch location data (4,021 rows)
- `python.ipynb` - Data cleaning and visualization (Python)
- `Scenario.docx` - Business context and requirements

---

## How to Use

### 1. Load Data into PostgreSQL

```sql
-- Create tables
CREATE TABLE customer_purchases (
    transaction_id TEXT,
    customer_id TEXT,
    customer_name TEXT,
    purchase_date DATE,
    product_category TEXT,
    channel TEXT,
    purchase_value NUMERIC
);

CREATE TABLE branches_purchase (
    transaction_id TEXT,
    branch_id TEXT,
    branch_name TEXT,
    region TEXT
);

-- Load CSV files
COPY customer_purchases FROM '/path/to/customer_purchases.csv' WITH CSV HEADER;
COPY branches_purchase FROM '/path/to/branches.csv' WITH CSV HEADER;
```

### 2. Run Queries

Open `postgresql_session.sql` and execute queries section by section:
- Data investigation
- CLV Analysis
- Gap & Churn Analysis
- Revenue Trends
- Branch Performance

### 3. Export Results

Use SQLTools or DBeaver to export query results as CSV for visualization.

---

## Key Findings

1. **Top Customer:** Customer (CLV ranking shows highest lifetime value)
2. **Churn Risk:** ~X% of customers showing extreme loosening risk
3. **Growth:** YoY analysis shows business trend from 2020-2026
4. **Branch Performance:** Downtown branch consistently outperforms others
5. **Customer Loyalty:** Average customer tenure ~1 year across database

---

## Learning Outcomes

This project demonstrates proficiency in:
- ✅ Window functions (RANK, DENSE_RANK, LAG, FIRST_VALUE, LAST_VALUE)
- ✅ Date/time operations (DATE_TRUNC, AGE, EXTRACT)
- ✅ CTEs and query organization
- ✅ PARTITION BY and ordering in complex analyses
- ✅ CASE statements for business logic
- ✅ Handling dirty/messy data

---

## Data Cleaning Notes

The raw purchase_date column contained mixed formats:
- YYYY-MM-DD, MM/DD/YYYY, MM/DD/YY, DD-MM-YYYY, DD/MM/YYYY
- Timestamps with and without time components
- Excel serial numbers
- Invalid/garbage entries (handled via WHERE clauses)

All queries filter for valid dates using `WHERE purchase_date IS NOT NULL`

---

## Technologies Used

- **Database:** PostgreSQL
- **Query Language:** SQL
- **Tools:** DBeaver, SQLTools (VS Code)
- **Data Format:** CSV
- **Time Period:** 2020-2026 (6+ years)

---

## Contact & Notes

**Project Purpose:** Portfolio project demonstrating SQL window functions and analytical skills for Data Analyst / RevOps role.

**Queries Completed:** 11/11 business questions answered

---

*Last Updated: July 2026*
