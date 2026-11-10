# Customer Shopping Behavior Analysis
### End-to-end data analytics project: Python · PostgreSQL · Power BI

![Python](https://img.shields.io/badge/Python-pandas-blue)
![SQL](https://img.shields.io/badge/SQL-PostgreSQL-336791)
![Power BI](https://img.shields.io/badge/Dashboard-Power%20BI-F2C811)

An end-to-end analytics workflow on a retail customer dataset: clean the data in Python, store it in PostgreSQL, answer business questions with SQL, and present the findings in an interactive Power BI dashboard, a written report and a slide deck.

---

## Table of Contents
1. [Overview](#overview)
2. [Business Questions](#business-questions)
3. [Dataset](#dataset)
4. [Tools & Technologies](#tools--technologies)
5. [Project Steps](#project-steps)
6. [Dashboard](#dashboard)
7. [Key Results](#key-results)
8. [Repository Structure](#repository-structure)
9. [How to Run](#how-to-run)
10. [Skills Demonstrated](#skills-demonstrated)
11. [Credits](#credits)

---

## Overview
Retail teams want to know **who their customers are, what they buy, and what drives spending**. This project follows a full analyst workflow: raw data in, decisions out.

| Stage | What was done | Tool |
|-------|---------------|------|
| 1. Load & explore | Loaded the CSV, inspected structure, types and summary statistics | Python (pandas) |
| 2. Clean & engineer features | Handled missing values, standardised column names, created new fields | Python (pandas) |
| 3. Store | Loaded the cleaned data into a relational database | SQLAlchemy + PostgreSQL |
| 4. Analyse | Answered 10 business questions with SQL | PostgreSQL (pgAdmin) |
| 5. Visualise | Built an interactive dashboard | Power BI |
| 6. Communicate | Wrote a project report and presentation | PDF / PowerPoint |

---

## Business Questions
The SQL analysis answers questions such as:

- How much revenue comes from male vs. female customers?
- Which customers used a discount but still spent above the average?
- Which products have the highest average review rating?
- Does shipping type affect the average purchase amount?
- Do subscribers spend more than non-subscribers?
- Which products are most often bought with a discount?
- How can customers be segmented into New, Returning and Loyal?
- What are the top 3 products in each category?
- Are repeat buyers more likely to subscribe?
- Which age group contributes the most revenue?

All queries are in [`customer_behavior_queries.sql`](customer_behavior_queries.sql).

---

## Dataset
- **File:** `customer_shopping_behavior.csv`
- **Size:** 3,900 customers × 18 columns
- **Content:** one row per customer, with demographics, purchase details and behaviour

| Group | Columns |
|-------|---------|
| Customer | Customer ID, Age, Gender, Location |
| Product | Item Purchased, Category, Size, Color, Season |
| Transaction | Purchase Amount (USD), Payment Method, Shipping Type, Discount Applied, Promo Code Used |
| Behaviour | Review Rating, Subscription Status, Previous Purchases, Frequency of Purchases |

---

## Tools & Technologies
- **Python:** pandas, SQLAlchemy, psycopg2, Jupyter Notebook
- **Database:** PostgreSQL, pgAdmin
- **BI:** Power BI Desktop
- **Reporting:** PDF report, PowerPoint presentation
- **Version control:** Git & GitHub

---

## Project Steps

### 1. Data loading and exploration (Python)
- Loaded the dataset with pandas
- Reviewed structure with `head()`, `info()` and `describe()`
- Checked for missing values and duplicates

### 2. Data cleaning and feature engineering (Python)
- **Missing values:** 37 missing `Review Rating` values filled using the **median rating of each product category**
- **Column names:** converted to snake_case (e.g. `purchase_amount`)
- **New column `age_group`:** customers split into four equal-sized groups (Young Adult, Adult, Middle-aged, Senior) with `pd.qcut`
- **New column `purchase_frequency_days`:** text frequencies (Weekly, Monthly, Quarterly …) converted to numbers of days
- **Redundant column removed:** `promo_code_used` was identical to `discount_applied`, so it was dropped

### 3. Database loading
- Connected Python to PostgreSQL with SQLAlchemy
- Wrote the cleaned DataFrame to a `customer` table with `df.to_sql()`

### 4. SQL analysis (PostgreSQL)
Used aggregations, `CASE` expressions, subqueries, CTEs and window functions (`ROW_NUMBER() OVER (PARTITION BY ...)`) to answer the ten business questions above.

### 5. Dashboard (Power BI)
Connected Power BI to the data and built a single-page interactive dashboard (see below).

### 6. Report and presentation
Summarised the method, findings and recommendations in a written report and a slide deck.

---

## Dashboard
The Power BI dashboard (`customer_behavior.pbix`) contains:

- **KPI cards:** number of customers, average purchase amount, average review rating
- **Donut chart:** share of subscribers vs. non-subscribers
- **Column charts:** revenue by category and sales by category
- **Bar charts:** revenue by season and revenue by age group
- **Table:** breakdown by shipping type
- **Slicers:** category, gender, subscription status, for interactive filtering

![Customer Behavior Dashboard](images/dashboard.png)

---

## Key Results
Headline numbers from the cleaned dataset (3,900 customers):

| Metric | Value |
|--------|-------|
| Average purchase amount | **$59.76** |
| Average review rating | **3.75 / 5** |
| Revenue from male customers | $157,890 (about 68% of total) |
| Revenue from female customers | $75,191 (about 32% of total) |
| Top revenue category | **Clothing** ($104,264) |
| Highest-rated product | **Gloves** (3.86) |
| Subscribers vs. non-subscribers (avg spend) | $59.49 vs. $59.87 |

**Insights**
- **Clothing is the main revenue driver**, followed by Accessories. Outerwear contributes the least.
- **Subscribing does not increase spend per purchase.** Subscribers and non-subscribers spend almost the same, so a subscription programme should be judged on retention rather than basket size.
- **Most customers are Loyal** (over 10 previous purchases), so retention matters more than acquisition here.
- **Heavy discounting on some products:** Hat, Sneakers and Coat have discount rates near 50%, which is worth reviewing for margin impact.
- **Revenue is evenly spread across seasons and age groups**, with no single segment dominating.

**Recommendations**
1. Focus marketing and inventory on Clothing and Accessories.
2. Redesign subscriber benefits to encourage higher spend or higher purchase frequency.
3. Review discount depth on the most heavily discounted products.
4. Reward Loyal customers to protect the largest customer segment.

---

## Repository Structure
```
├── Customer_Shopping_Behavior_Analysis.ipynb   # Python: load, EDA, cleaning, DB load
├── customer_behavior_queries.sql               # 10 SQL business questions
├── customer_behavior.pbix                      # Power BI dashboard
├── customer_shopping_behavior.csv              # Dataset
├── Project Report.pdf                          # Written report
├── Presentation.pptx                           # Slide deck
└── README.md
```
*(Adjust file names to match your repo.)*

---

## How to Run

### Prerequisites
- Python 3.9+ (Anaconda recommended)
- PostgreSQL and pgAdmin
- Power BI Desktop (Windows)

### Steps
1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
   ```
2. **Install dependencies**
   ```bash
   pip install pandas sqlalchemy psycopg2-binary jupyter
   ```
3. **Create the database** in pgAdmin: right-click *Databases → Create → Database* and name it `customer_behavior`.
4. **Run the notebook.** Open `Customer_Shopping_Behavior_Analysis.ipynb` and run the cells in order. The last cell asks for your PostgreSQL password at runtime, so no credentials are stored in the notebook.
5. **Run the SQL.** Open `customer_behavior_queries.sql` in pgAdmin's Query Tool and run each query with **F5**.
6. **Open the dashboard.** Open `customer_behavior.pbix` in Power BI Desktop. If prompted, update the data source to point to your local PostgreSQL server.

---

## Skills Demonstrated
- Data cleaning and feature engineering with **pandas**
- Writing **SQL** with joins-free aggregations, CTEs, subqueries and window functions
- Connecting **Python to a relational database**
- Designing **interactive dashboards** in Power BI
- Turning data into **business recommendations** and communicating them to stakeholders

---
