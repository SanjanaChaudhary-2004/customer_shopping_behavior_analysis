# 🛍️ Customer Shopping Behavior Analysis | End-to-End Data Analytics Project

> An end-to-end data analytics project analyzing customer shopping patterns, revenue trends, segmentation, and subscription behavior — built with Python, PostgreSQL, and Power BI.

---

## 📌 Overview

This project dives into a customer shopping dataset to uncover meaningful business insights about purchasing habits, product performance, discount impact, and customer segments. The workflow covers the complete analytics pipeline — from raw data loading and cleaning in Python to SQL-based analysis, an interactive Power BI dashboard, and a stakeholder presentation.

**Key business questions answered:**
- Which gender generates more revenue?
- Do subscribed customers spend more?
- Which products and categories perform best?
- How do different age groups contribute to revenue?
- Which customers are New, Returning, or Loyal?

---

## 📁 Dataset

| Detail | Info |
|---|---|
| **File** | `customer_shopping_behavior.csv` |
| **Records** | 3,900 rows |
| **Domain** | Retail / E-commerce |

**Key columns:**
- `customer_id` — Unique customer identifier
- `age`, `gender` — Customer demographics
- `item_purchased`, `category` — Product details
- `purchase_amount` — Transaction value (USD)
- `review_rating` — Customer rating (1–5)
- `subscription_status` — Subscribed or not
- `discount_applied` — Whether a discount was used
- `frequency_of_purchases` — Purchase frequency label
- `previous_purchases` — Count of past orders
- `shipping_type` — Delivery method

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Python** (Pandas) | Data loading, EDA, cleaning, feature engineering |
| **PostgreSQL** (pgAdmin) | Data storage and SQL querying |
| **SQLAlchemy** | Python-to-PostgreSQL connection |
| **Power BI** | Interactive dashboard |
| **Gamma** | Presentation (PPT) |
| **Jupyter Notebook** | Analysis environment |

---

## 📂 Project Structure

```
📦 customer-shopping-behavior-analysis
├── 📂 dataset
│   └── customer_shopping_behavior.csv
├── 📂 notebooks
│   └── Customer_Shopping_Behavior_Analysis.ipynb
├── 📂 sql
│   └── customer_behavior_sql_queries.sql
├── 📂 dashboard
│   └── Data_Analysis_Dashboard.png
├── 📂 presentation
│   ├── 1_Customer-Shopping-Behavior-Analysis.png
│   ├── 2_Key-Findings-at-a-Glance.png
│   ├── 3_Project-Overview.png
│   ├── 4_Methodology-and-Tech-Stack.png
│   ├── 5_SQL-Analysis-Key-Results.png
│   ├── 6_Customer-Segmentation.png
│   ├── 7_Python-EDA-Key-Observations.png
│   ├── 8_Power-BI-Dashboard.png
│   ├── 9_Key-Insights-and-Recommendations.png
│   └── 10_Conclusion.png
└── README.md
```

---

## 🔄 Project Workflow

### Step 1 — Load Dataset
Loaded the CSV dataset using **Pandas** and performed an initial inspection using `.head()`, `.info()`, and `.describe()`.

```python
import pandas as pd
df = pd.read_csv('customer_shopping_behavior.csv')
df.head()
```

---

### Step 2 — Exploratory Data Analysis (EDA)
- Checked dataset shape, data types, and summary statistics
- Identified missing values using `df.isnull().sum()`
- Explored distributions of key columns like age, purchase amount, and review rating

---

### Step 3 — Data Cleaning & Feature Engineering
- **Imputed missing values** in `review_rating` using category-wise median
- **Standardized column names** to snake_case for consistency
- **Created new features:**
  - `age_group` — segmented customers into Young Adult, Adult, Middle-aged, Senior using `pd.qcut()`
  - `purchase_frequency_days` — mapped frequency labels to numeric days
- **Dropped redundant column** `promo_code_used` (identical to `discount_applied`)

```python
# Impute missing review ratings
df['Review Rating'] = df.groupby('Category')['Review Rating'].transform(lambda x: x.fillna(x.median()))

# Create age group segments
labels = ['Young Adult', 'Adult', 'Middle-aged', 'Senior']
df['age_group'] = pd.qcut(df['age'], q=4, labels=labels)
```

---

### Step 4 — Load Data to PostgreSQL
Connected Python to a local **PostgreSQL** database using **SQLAlchemy** and loaded the cleaned DataFrame as a table.

```python
from sqlalchemy import create_engine

engine = create_engine("postgresql+psycopg2://username:password@localhost:5432/customer_behavior")
df.to_sql('customer', engine, if_exists='replace', index=False)
```

---

### Step 5 — SQL Analysis (10 Queries)

Ran **10 business-focused SQL queries** on PostgreSQL covering:

| # | Query |
|---|---|
| Q1 | Total revenue by gender |
| Q2 | Discount users who spent above average |
| Q3 | Top 5 products by average review rating |
| Q4 | Avg purchase amount: Standard vs Express shipping |
| Q5 | Subscriber vs non-subscriber spend comparison |
| Q6 | Top 5 products by discount usage rate |
| Q7 | Customer segmentation — New, Returning, Loyal |
| Q8 | Top 3 products per category (Window Function) |
| Q9 | Repeat buyers and their subscription likelihood |
| Q10 | Revenue contribution by age group |

**Sample — Customer Segmentation using CTE:**
```sql
WITH customer_type AS (
    SELECT customer_id, previous_purchases,
        CASE
            WHEN previous_purchases = 1 THEN 'New'
            WHEN previous_purchases BETWEEN 2 AND 10 THEN 'Returning'
            ELSE 'Loyal'
        END AS customer_segment
    FROM customer
)
SELECT customer_segment, COUNT(*) AS "Number of Customers"
FROM customer_type
GROUP BY customer_segment;
```

---

### Step 6 — Power BI Dashboard

Built an interactive **Customer Behavior Dashboard** in Power BI with:

- **KPI Cards:** Total Customers (3.9K), Avg Purchase Amount ($59.76), Avg Review Rating (3.75)
- **Donut Chart:** % of Customers by Subscription Status (Yes 27% / No 73%)
- **Bar Charts:** Revenue by Category, Sales by Category
- **Horizontal Bar Charts:** Revenue by Age Group, Sales by Age Group
- **Slicers:** Subscription Status, Gender, Category, Shipping Type

---

### Step 7 — Presentation (Gamma)

Created a 10-slide stakeholder presentation covering:
- Project overview and objectives
- Key findings at a glance
- Methodology and tech stack
- SQL analysis results
- Customer segmentation breakdown
- Python EDA observations
- Power BI dashboard walkthrough
- Key insights and business recommendations
- Conclusion

---

## 📈 Key Insights

- 👨 **Male customers** generate slightly higher total revenue than female customers
- 💳 **Subscribed customers** (27%) have a higher average spend than non-subscribers
- 👗 **Clothing** is the top-performing category by both revenue and sales volume
- 🧑 **Young Adults** contribute the most revenue across all age groups
- 🔁 **Loyal customers** (10+ previous purchases) form the largest segment
- 🏷️ Customers who used discounts still spent at or above the average purchase amount

---

## ▶️ How to Run

### 1. Clone the Repository
```bash
git clone https://github.com/SanjanaChaudhary-2004/customer-shopping-behavior-analysis.git
cd customer-shopping-behavior-analysis
```

### 2. Install Python Dependencies
```bash
pip install pandas sqlalchemy psycopg2-binary jupyter
```

### 3. Run the Notebook
```bash
jupyter notebook
```
Open `notebooks/Customer_Shopping_Behavior_Analysis.ipynb` and run all cells.

### 4. Set Up PostgreSQL
- Create a database named `customer_behavior` in pgAdmin
- Update the connection credentials in the notebook
- Run the notebook to auto-load the cleaned data into PostgreSQL
- Execute queries from `sql/customer_behavior_sql_queries.sql`

### 5. Open the Dashboard
- Open Power BI Desktop
- Import the dataset or connect to your PostgreSQL database
- Recreate or view the dashboard using `dashboard/Data_Analysis_Dashboard.png` as reference

---

## 🤝 Connect With Me

**Sanjana Chaudhary**
💼 [LinkedIn](www.linkedin.com/in/sanjana-chaudhary-9686ab278)
🐙 [GitHub](https://github.com/SanjanaChaudhary-2004)

---

> ⭐ *If you found this project helpful, consider giving it a star!*

