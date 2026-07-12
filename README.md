# Retail-Sales-Performance-Customer-Insights-Dashboard

## Project Overview

In the retail industry, tracking revenue and profitability across regions is critical for making smart business decisions. This project analyzes over 9,000 sales records from a US-based retail superstore to uncover key revenue and profit , consolidated into a single interactive Power BI dashboard that supports data-driven decision-making.

## Contents

-[Aim](#aim)

-[Tools Required](#tools-required)

-[Data Source](#data-source)

-[Data Cleaning](#data-cleaning)

-[Data exploration and processing](#data-exploration-and-processing)

-[Results and Observations](#results-and-observations)

-[Recommendations](#recommendations)

## Aim
 
 To analyze retail sales performance and build a single Power BI dashboard highlighting key revenue, profit, and category sales to support business decision-making.

## Tools Required

- **Python** — Pandas, NumPy
- **SQL** — MySQL
- **Power BI** — DAX, Power Query
- **Excel**

## Data Source

- **Dataset:** Sample Superstore Sales Dataset
- **Source:** Kaggle [click here to download](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)
- **Records:** 9,994 rows | 21 columns
- **Period:** 2014 – 2017
- **Domain:** US Retail / E-commerce
- **Format:** CSV

## Data Cleaning

**Tool Used:** Python (Pandas)

**Steps Performed:**
- Checked and handled missing values in Postal Code column
- Removed duplicate records from the dataset
- Converted Order Date and Ship Date to datetime format
- Dropped unnecessary columns (Row ID, Country)
- Extracted new features — Order Year, Order Month, Shipping Days
- Stripped extra whitespace from text columns

## Data exploration and processing

### SQL
- Filtered profitable orders, region-wise and category-wise data
- Sorted and ranked data by Sales and Profit
- Extracted KPIs — Total Sales, Profit, Top Customers, Monthly Trends

### Excel
- Applied filters for Region, Category, and Segment
- Used Pivot Tables for quick Sales summary
- Applied SUMIF and COUNTIF formulas for calculation

## Results and Observations

<img width="902" height="565" alt="WhatsApp Image 2026-07-11 at 11 22 37 PM" src="https://github.com/user-attachments/assets/b2c8dcd8-7ee0-482a-a4ec-b74ee737df19" />


- The store achieved $1.01M in total sales, maintaining a 14.03% profit margin.
- Office Supplies was the top-performing category, contributing of total sales.
- The Consumer segment generated the highest revenue with $344.45K (34.1%) of total sales.
- November recorded the highest monthly sales, indicating strong seasonal demand and opportunities for year-end promotions.

## Recommendations

- Focus on high-performing products by maintaining adequate inventory and promoting top-selling sub-categories like Tables and Paper to maximize revenue.
- Strengthen sales in lower-performing regions by introducing targeted marketing campaigns and region-specific discounts to improve overall sales performance.
- Leverage seasonal demand by increasing inventory and launching promotional offers before November, the highest sales month, to maximize year-end revenue.


  
