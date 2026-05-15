# 🛒 Customer Behaviour Analysis

> **A full-stack data analytics project** exploring retail customer purchasing behaviour using Python, SQL Server, and Power BI — transforming raw e-commerce data into actionable business insights.

---

## 📌 Table of Contents

- [Project Overview](#project-overview)
- [Business Problem](#business-problem)
- [Tech Stack](#tech-stack)
- [Project Workflow](#project-workflow)
- [Dataset](#dataset)
- [Exploratory Data Analysis (Python)](#exploratory-data-analysis-python)
- [SQL Business Insights](#sql-business-insights)
- [Power BI Dashboard](#power-bi-dashboard)
- [Key Findings](#key-findings)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Connect with Me](#connect-with-me)

---

## 📖 Project Overview

This end-to-end data analytics project analyzes the purchasing behaviour of **5,000 retail/e-commerce customers** to help a retail company increase revenue, improve customer retention, and optimize marketing strategies.

The project follows a complete data analytics pipeline:

```
Raw CSV Data → Python (EDA & Cleaning) → SQL Server (Business Analysis) → Power BI (Dashboard)
```

---

## ❓ Business Problem

> *How can a retail company understand customer purchasing behaviour to increase revenue, improve customer retention, and optimize marketing strategies?*

**Core challenges addressed:**
- 📉 Low Customer Retention — identifying frequent vs. one-time buyers
- 🎯 Ineffective Marketing — evaluating discount and promo code effectiveness
- 📦 Product Demand Understanding — finding top-performing categories and products
- 💳 Payment Behaviour Insights — understanding preferred payment methods
- 🌦️ Seasonal Demand Variation — identifying which seasons drive maximum sales
- 👥 Customer Segmentation — differentiating high-value, frequent, and discount-sensitive customers

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) **Python** | Data cleaning, EDA, export to SQL |
| ![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white) **Pandas** | Data manipulation & transformation |
| ![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927?style=flat&logo=microsoftsqlserver&logoColor=white) **SQL Server** | Business insights & advanced queries |
| ![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black) **Power BI** | Interactive dashboards & visualizations |

---

## 🔄 Project Workflow

```
┌─────────────────┐     ┌─────────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   Raw CSV Data  │────▶│  Python (EDA &       │────▶│  SQL Server      │────▶│  Power BI       │
│                 │     │  Data Cleaning)      │     │  (12 Business    │     │  (Interactive   │
│  5,000 Records  │     │  Jupyter Notebook    │     │   Questions)     │     │   Dashboard)    │
└─────────────────┘     └─────────────────────┘     └──────────────────┘     └─────────────────┘
```

---

## 📊 Dataset

| Attribute | Details |
|-----------|---------|
| **Type** | Retail / E-commerce |
| **Nature** | Structured (Tabular) |
| **Level** | Customer Transaction Level |
| **Records** | 5,000 customers |
| **Unique Items** | 30 products |

**Key Columns:**

| Column | Description |
|--------|-------------|
| `customer_id` | Unique identifier for each customer |
| `age` | Age of the customer |
| `gender` | Gender (Male / Female / Other) |
| `item_purchased` | Specific product purchased |
| `category` | Product category (Electronics, Clothing, etc.) |
| `purchase_amount` | Amount spent (USD) |
| `season` | Season of purchase |
| `review_rating` | Customer rating for the product |
| `subscription_status` | Whether customer is subscribed (Yes/No) |
| `shipping_type` | Type of shipping selected |
| `discount_applied` | Whether a discount was applied |
| `previous_purchases` | Number of past purchases |
| `payment_method` | Mode of payment |

---

## 🐍 Exploratory Data Analysis (Python)

Performed in **Jupyter Notebook** (`Data_Analyst_portfolioproject_1_0_keval.ipynb`).

### Steps Performed:

**1. Data Loading**
```python
import pandas as pd
df = pd.read_csv('customer_shopping_behavior.csv')
```

**2. Initial Exploration**
- `df.head()`, `df.tail()`, `df.describe(include='all')`, `df.info()`
- Missing values check: `df.isnull().sum()`

**3. Data Consistency Check**
- Identified and corrected mismatched `item_purchased` ↔ `category` mappings
- Created a standardized mapping dictionary

**4. Missing Value Treatment**

| Column | Strategy |
|--------|----------|
| `size` (Electronics/Accessories) | Filled with `"Not Applicable"` |
| `size` (Footwear) | Filled with `"Free Size"` |
| `size` (Clothing) | Filled with mode |
| `review_rating` | Filled with **product-level mean** (advanced imputation) |
| `purchase_amount` | Filled with mean value |
| `previous_purchases` | Filled with `0` (implies new customer) |

**5. Duplicate Handling**
- Identified and removed duplicates by `customer_id`

**6. Column Standardization**
```python
df.columns = df.columns.str.replace(" ", "_").str.lower()
```

**7. Export to SQL Server**
```python
pip install pyodbc sqlalchemy
# Connected and exported cleaned DataFrame to SQL Server
```

---

## 🗄️ SQL Business Insights

12 business questions answered using **SQL Server** (`BUSINESS_INSIGHTS.sql`).

| # | Business Question | Key Insight |
|---|-------------------|-------------|
| 1 | Which category generates the highest revenue? | Electronics leads with **$486K** |
| 2 | Are discounts actually increasing purchase value? | Discounted orders average **$219** vs. $182 without |
| 3 | Revenue by gender? | Male: **$390K** \| Female: **$352K** \| Other: **$251K** |
| 4 | Discount users who spent above average? | Identified high-value discount-sensitive customers |
| 5 | Top/Bottom 5 products by review rating? | Gloves (3.86 ⭐) best; Phone (2.94 ⭐) worst |
| 6 | Standard vs Express shipping spend? | Express avg: **$358** vs Standard: **$340** |
| 7 | Do subscribers spend more? | Subscribers avg **$270** vs non-subscribers **$165** |
| 8 | Top 5 products by discount usage %? | Laptop leads at **~51.7%** discount rate |
| 9 | Customer segmentation? | Loyal: **3,085** \| Returning: **1,354** \| New: **561** |
| 10 | Top 3 products per category? | Shirt #1 in Clothing; Phone #1 in Electronics |
| 11 | Repeat buyers likely to subscribe? | Repeat buyers: **30%** subscribe; Normal: **37%** |
| 12 | Revenue by age group? | 51+ age group contributes the most: **$399K** |

### Sample Query — Customer Segmentation:
```sql
SELECT 
    CASE 
        WHEN previous_purchases = 0 THEN 'New Customer'
        WHEN previous_purchases BETWEEN 1 AND 15 THEN 'Returning Customer'
        ELSE 'Loyal Customers'
    END AS customer_segment,
    COUNT(*) AS customer_count
FROM customer_behaviour_analysis
GROUP BY CASE 
        WHEN previous_purchases = 0 THEN 'New Customer'
        WHEN previous_purchases BETWEEN 1 AND 15 THEN 'Returning Customer'
        ELSE 'Loyal Customers'
    END;
```

---

## 📈 Power BI Dashboard

Two-page interactive dashboard built in **Power BI Desktop** (`Customer_behaviour_analysis.pbix`).

### Page 1 — Overview Dashboard
- KPI Cards: Total Customers, Unique Items, Average Rating, Total Spend, Average Spend
- Revenue by Gender (Donut Chart)
- Revenue by Category (Bar Chart)
- Shipping Mode Analysis
- Revenue by Season
- Revenue by Age Distribution
- Payment Method Breakdown
- **Filters:** Category, Gender, Discount Applied, Subscription Status

### Page 2 — Product & Location Analysis
- Top 5 Items by Rating
- Bottom 5 Items by Rating
- Top 5 Items by Revenue
- Bottom 5 Items by Revenue
- Top 5 Locations by Revenue
- Bottom 5 Locations by Revenue

### Dashboard Preview

> 📊 *5,000 customers | 30 unique items | $994.18K total spend | 3.62 avg rating*

---

## 💡 Key Findings

- 🏆 **Electronics** is the highest-revenue category ($486K), nearly 2.5× more than the next category
- 💰 **Discounts work** — discounted orders average 20% higher purchase value
- 🎯 **Subscribed customers** spend 63% more on average than non-subscribers
- 👴 The **51+ age group** is the most valuable customer segment ($399K revenue)
- 🔄 **60%+ of customers** are loyal (16+ previous purchases), indicating strong retention
- 📦 **Express shipping** users spend slightly more, suggesting a premium customer base
- 📍 **New York** is the top revenue-generating city ($143K)

---

## 📁 Project Structure

```
customer-behaviour-analysis/
│
├── 📓 Data_Analyst_portfolioproject_1_0_keval.ipynb   # Python EDA & Data Cleaning
├── 🗄️ BUSINESS_INSIGHTS.sql                           # SQL Server Queries (12 Questions)
├── 📊 Customer_behaviour_analysis.pbix                # Power BI Dashboard
├── 📄 Business_Problem_Customer_Behaviour_Analysis.pdf # Project Brief
├── 📄 Report-Customer_Behaviour_Analysis.pdf          # Analysis Report
└── 📝 README.md
```

---

## ▶️ How to Run

### Python (EDA)
```bash
# Install dependencies
pip install pandas pyodbc sqlalchemy jupyter

# Launch notebook
jupyter notebook Data_Analyst_portfolioproject_1_0_keval.ipynb
```

### SQL Server
```sql
-- Run in SQL Server Management Studio (SSMS)
-- 1. Create database
CREATE DATABASE data_analysis_project;
USE data_analysis_project;

-- 2. Import cleaned CSV (via Python export or SSMS import wizard)
-- 3. Run queries from BUSINESS_INSIGHTS.sql
```

### Power BI
```
1. Open Customer_behaviour_analysis.pbix in Power BI Desktop
2. Update data source connection if needed
3. Refresh data
4. Explore the interactive dashboard
```

---

## 🤝 Connect with Me

If you found this project helpful or have suggestions, feel free to connect!

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/keval-patil-a840b3378)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/patilkeval4477-cyber)

---

<p align="center">
  ⭐ <strong>If you found this project useful, please give it a star!</strong> ⭐
</p>
