# Customer Churn Analysis & Customer Intelligence

An end-to-end customer churn analysis project built with **SQL Server, Python, Pandas, NumPy, Matplotlib, and Seaborn**. The project combines customer, subscription, and support data to measure churn, retention, revenue at risk, customer tenure, support escalations, and churn risk.

## Project Overview

Customer churn is an important business problem because losing existing customers can directly affect recurring revenue and long-term customer value.

In this project, data is stored in a SQL Server database and analyzed in Python. The workflow covers the full analytics process:

**SQL Server → Data Extraction → Data Cleaning → Feature Engineering → Data Integration → KPI Analysis → Visualization → Business Insights**

The analysis works with three source tables:

- `db_customer` – customer profile information
- `db_subscription` – subscription, plan, contract, billing, and churn information
- `db_support` – customer complaints/support activity

## Objectives

- Measure the overall customer churn and retention rate.
- Compare churn across subscription plans.
- Understand customer tenure and recurring charges.
- Estimate revenue exposed to customer churn.
- Analyze support escalations and their relationship with churn.
- Create a customer churn-risk segment using churn scores.
- Identify churn trends over time and across states.
- Combine customer, subscription, and support data into a single analytical dataset.

## Tech Stack

- **Python**
- **Pandas** – data cleaning, transformation, joins, aggregation, pivot tables
- **NumPy** – feature engineering and conditional logic
- **Matplotlib** – KPI and trend visualizations
- **Seaborn** – heatmap, pairplot, and multi-dimensional categorical analysis
- **PyODBC** – SQL Server connectivity
- **SQL Server** – source database
- **Jupyter Notebook** – development and analysis environment

## Data Preparation

The notebook performs the following data-cleaning steps:

### Customer Data

- Renamed `name` to `Customer_name` for clarity.
- Removed `interests` and `pincode` columns.
- Converted `dob` from text to datetime.
- Standardized gender values from `Men/Women` to `Male/Female`.
- Filled missing country values using the state-to-country mapping.

### Subscription Data

- Converted `subscription_start_date`, `renewal_date`, and `cancellation_date` to datetime.
- Created a binary `churn_flag`:
  - `1` = customer has a cancellation date
  - `0` = customer is not cancelled

### Support Data

- Removed the last two columns from the support table.
- Converted `complaint_date` to datetime.
- Calculated complaint count by customer.
- Kept the latest support record for each customer after calculating complaint frequency.

## Data Integration

The cleaned tables are joined using `customerid`.

```python
 df = (df_db_subscription
       .merge(df_db_customer, on='customerid', how='left')
       .merge(df_db_support, on='customerid', how='left'))
```

This creates a consolidated customer-level dataset containing subscription, demographic, billing, churn, and support information.

## Feature Engineering

The project creates several analytical features from the source data:

- **Churn Flag** – identifies churned customers.
- **Tenure Days** – calculates the number of days a customer has remained subscribed.
- **Complaint Count** – counts support complaints per customer.
- **Churn Risk** – segments customers using `churn_score`:
  - `Low` – score ≤ 50
  - `Mid` – score > 50 and < 70
  - `High` – score ≥ 70
- **Escalation Encoding** – converts escalation values from `Y/N` into numeric form for analysis.
- **Cancellation Month** – extracts the month of cancellation for churn trend analysis.

## Key KPIs Analyzed

The notebook calculates the following customer and business KPIs:

| KPI | Purpose |
|---|---|
| Churn Rate | Measures the percentage of customers who churned |
| Retention Rate | Measures the percentage of customers retained |
| Churn by Plan | Compares churn across plan types |
| ARPU / Average Monthly Charges | Measures average monthly customer charges |
| Average Customer Tenure | Measures customer lifetime duration |
| Revenue at Risk | Estimates monthly charges associated with churned customers |
| Escalation Rate | Measures the share of customers with escalations |
| Average Complaints per User | Measures average support complaints per customer |
| Escalation vs Churn Correlation | Quantifies the relationship between escalations and churn |

### Example Notebook Results

The current notebook execution produced the following sample results:

- **Churn Rate:** 28.57%
- **Retention Rate:** 71.43%
- **Average Monthly Charges:** 18.85
- **Average Customer Tenure:** 1,542.29 days
- **Revenue at Risk:** 73.94K
- **Escalation Rate:** 19.05%
- **Average Complaints per User:** 0.43
- **Escalation vs Churn Correlation:** 0.77

> These values reflect the current sample data used in the notebook and should be treated as project-level results rather than general business benchmarks.

## Visual Analysis

The project uses both Matplotlib and Seaborn to explore churn behavior.

### Matplotlib

- **Monthly Churn Trend** – tracks the number of churned customers by cancellation month.
- **Churn Rate by Plan Type** – compares churn across Basic, Standard, and Premium plans.
- **Churn by State** – compares churn rates across customer locations.

### Seaborn

- **Correlation Heatmap** – evaluates relationships among plan type, contract type, churn score, churn flag, escalations, and churn risk.
- **Pairplot** – explores pairwise relationships among encoded churn-related variables.
- **Categorical / FacetGrid Analysis** – compares monthly charges by plan type, gender, and churn-risk segment.
- **Pivot Table Analysis** – summarizes monthly charges, unique customers, and churn rate by plan.

## Business Questions Answered

1. What percentage of customers are churning?
2. What percentage of customers are being retained?
3. Which subscription plans have different churn behavior?
4. How much recurring revenue is associated with churned customers?
5. What is the average customer tenure?
6. How common are support escalations and complaints?
7. Is there a relationship between support escalations and customer churn?
8. Which customers fall into low, medium, and high churn-risk groups?
9. How does churn change over time?
10. How does churn vary across customer locations?

## Project Workflow

```text
SQL Server Database
        ↓
Connect with PyODBC
        ↓
Load Source Tables
        ↓
Data Cleaning & Standardization
        ↓
Feature Engineering
        ↓
Merge Customer + Subscription + Support Data
        ↓
KPI Calculation
        ↓
EDA & Correlation Analysis
        ↓
Matplotlib / Seaborn Visualizations
        ↓
Business Insights
```

## SQL Server Connection

The notebook connects to a local SQL Server Express instance and reads the available base tables dynamically.

```python
conn = pyodbc.connect(
    'Driver={SQL Server};'
    'Server=localhost\\SQLEXPRESS;'
    'database=customer_churn;'
    'Trusted_Connection=yes;'
)
```

Before running the notebook, update the connection details according to your SQL Server environment.

## Repository Structure

```text
Customer-Churn-Analysis/
│
├── Churn_Analysis.ipynb
├── README.md
└── data/                     # Optional: local/sample data files if included
```

> The current notebook reads its data directly from SQL Server, so the database is an external project dependency unless you add exported datasets to the repository.

## How to Run the Project

1. Install Python and Jupyter Notebook.
2. Install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn pyodbc
```

3. Make sure SQL Server is running and the `customer_churn` database is available.
4. Update the database connection string in the notebook.
5. Open `Churn_Analysis.ipynb` in Jupyter Notebook.
6. Run the cells from top to bottom.

## Skills Demonstrated

This project demonstrates practical data-analytics skills in:

- SQL Server data extraction
- Python data analysis
- Pandas data cleaning and transformation
- Data standardization
- Missing-value handling
- Datetime conversion and analysis
- DataFrame merging and joins
- Feature engineering
- KPI calculation
- Customer churn analysis
- Revenue analysis
- Correlation analysis
- Data visualization
- Pivot tables
- Business-oriented insight generation

## Project Outcome

The final analytical dataset combines customer demographics, subscription details, billing metrics, churn indicators, and support activity. This makes it possible to evaluate churn from multiple business perspectives instead of looking at cancellation data alone.

The project is designed to demonstrate a practical **Data Analyst workflow**, from extracting raw data from a database to producing business-focused KPIs and visual insights in Python.

## Author

**Shahnawaz Rajput**

Data Analyst | Python | SQL | Excel | Data Visualization
