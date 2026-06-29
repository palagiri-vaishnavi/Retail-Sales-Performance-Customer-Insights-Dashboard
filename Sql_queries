-- ============================================================
-- SUPERSTORE SALES DATA - SQL ORGANIZATION QUERIES
-- Dataset: superstore_cleaned_data.csv (9995 rows, 23 columns)
-- ============================================================


-- ============================================================
-- SECTION 1: TABLE CREATION
-- ============================================================

-- Step 1: Create the main raw orders table
CREATE TABLE superstore_orders (
    order_id        VARCHAR(20),
    order_date      DATE,
    ship_date       DATE,
    ship_mode       VARCHAR(30),
    customer_id     VARCHAR(15),
    customer_name   VARCHAR(100),
    segment         VARCHAR(20),
    city            VARCHAR(60),
    state           VARCHAR(50),
    postal_code     VARCHAR(10),
    region          VARCHAR(20),
    product_id      VARCHAR(20),
    category        VARCHAR(30),
    sub_category    VARCHAR(30),
    product_name    VARCHAR(255),
    sales           DECIMAL(10, 4),
    quantity        INT,
    discount        DECIMAL(4, 2),
    profit          DECIMAL(10, 4),
    order_year      INT,
    order_month     INT,
    order_month_name VARCHAR(15),
    shipping_days   INT
);

-- Step 2: Load data from CSV (PostgreSQL syntax)
-- COPY superstore_orders FROM '/path/to/superstore_cleaned_data.csv'
-- DELIMITER ',' CSV HEADER;

-- For MySQL:
-- LOAD DATA INFILE '/path/to/superstore_cleaned_data.csv'
-- INTO TABLE superstore_orders
-- FIELDS TERMINATED BY ',' ENCLOSED BY '"'
-- LINES TERMINATED BY '\n'
-- IGNORE 1 ROWS;


-- ============================================================
-- SECTION 2: NORMALIZED DIMENSION TABLES
-- (Best practice for BI / Power BI integration)
-- ============================================================

-- Dimension: Customers
CREATE TABLE dim_customers AS
SELECT DISTINCT
    customer_id,
    customer_name,
    segment
FROM superstore_orders;

-- Dimension: Products
CREATE TABLE dim_products AS
SELECT DISTINCT
    product_id,
    product_name,
    category,
    sub_category
FROM superstore_orders;

-- Dimension: Geography
CREATE TABLE dim_geography AS
SELECT DISTINCT
    city,
    state,
    postal_code,
    region
FROM superstore_orders;

-- Dimension: Date
CREATE TABLE dim_date AS
SELECT DISTINCT
    order_date,
    order_year,
    order_month,
    order_month_name
FROM superstore_orders
ORDER BY order_date;

-- Fact Table: Orders (numeric measures + foreign keys)
CREATE TABLE fact_orders AS
SELECT
    order_id,
    order_date,
    ship_date,
    ship_mode,
    customer_id,
    product_id,
    city,
    state,
    postal_code,
    sales,
    quantity,
    discount,
    profit,
    shipping_days
FROM superstore_orders;


-- ============================================================
-- SECTION 3: DATA EXPLORATION QUERIES
-- ============================================================

-- 3.1 Total row count
SELECT COUNT(*) AS total_rows FROM superstore_orders;

-- 3.2 Check for NULL values in key columns
SELECT
    SUM(CASE WHEN order_id       IS NULL THEN 1 ELSE 0 END) AS null_order_id,
    SUM(CASE WHEN customer_id    IS NULL THEN 1 ELSE 0 END) AS null_customer_id,
    SUM(CASE WHEN product_id     IS NULL THEN 1 ELSE 0 END) AS null_product_id,
    SUM(CASE WHEN sales          IS NULL THEN 1 ELSE 0 END) AS null_sales,
    SUM(CASE WHEN profit         IS NULL THEN 1 ELSE 0 END) AS null_profit
FROM superstore_orders;

-- 3.3 Check for duplicate orders (same order + product combination)
SELECT order_id, product_id, COUNT(*) AS occurrences
FROM superstore_orders
GROUP BY order_id, product_id
HAVING COUNT(*) > 1;

-- 3.4 Date range of the dataset
SELECT
    MIN(order_date) AS earliest_order,
    MAX(order_date) AS latest_order
FROM superstore_orders;

-- 3.5 Distinct values in categorical columns
SELECT DISTINCT segment   FROM superstore_orders;
SELECT DISTINCT region    FROM superstore_orders;
SELECT DISTINCT category  FROM superstore_orders;
SELECT DISTINCT ship_mode FROM superstore_orders;


-- ============================================================
-- SECTION 4: SALES & REVENUE ANALYSIS
-- ============================================================

-- 4.1 Total Sales, Profit, Orders, and Avg Discount
SELECT
    ROUND(SUM(sales),    2) AS total_sales,
    ROUND(SUM(profit),   2) AS total_profit,
    ROUND(SUM(quantity), 0) AS total_units_sold,
    COUNT(DISTINCT order_id)           AS total_orders,
    ROUND(AVG(discount) * 100, 2)      AS avg_discount_pct,
    ROUND(SUM(profit) / SUM(sales) * 100, 2) AS profit_margin_pct
FROM superstore_orders;

-- 4.2 Sales and Profit by Year
SELECT
    order_year,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit,
    COUNT(DISTINCT order_id) AS total_orders
FROM superstore_orders
GROUP BY order_year
ORDER BY order_year;

-- 4.3 Monthly Sales Trend (across all years)
SELECT
    order_year,
    order_month,
    order_month_name,
    ROUND(SUM(sales),  2) AS monthly_sales,
    ROUND(SUM(profit), 2) AS monthly_profit
FROM superstore_orders
GROUP BY order_year, order_month, order_month_name
ORDER BY order_year, order_month;

-- 4.4 Sales by Region
SELECT
    region,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit,
    COUNT(DISTINCT order_id) AS total_orders,
    ROUND(SUM(profit) / SUM(sales) * 100, 2) AS profit_margin_pct
FROM superstore_orders
GROUP BY region
ORDER BY total_sales DESC;

-- 4.5 Sales by Category and Sub-Category
SELECT
    category,
    sub_category,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit,
    SUM(quantity) AS units_sold,
    ROUND(SUM(profit) / SUM(sales) * 100, 2) AS profit_margin_pct
FROM superstore_orders
GROUP BY category, sub_category
ORDER BY category, total_sales DESC;

-- 4.6 Sales by Customer Segment
SELECT
    segment,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit,
    COUNT(DISTINCT customer_id) AS unique_customers,
    COUNT(DISTINCT order_id)    AS total_orders
FROM superstore_orders
GROUP BY segment
ORDER BY total_sales DESC;


-- ============================================================
-- SECTION 5: CUSTOMER ANALYSIS (Customer 360)
-- ============================================================

-- 5.1 Top 10 Customers by Revenue
SELECT
    customer_id,
    customer_name,
    segment,
    COUNT(DISTINCT order_id)   AS total_orders,
    ROUND(SUM(sales),  2)      AS total_sales,
    ROUND(SUM(profit), 2)      AS total_profit
FROM superstore_orders
GROUP BY customer_id, customer_name, segment
ORDER BY total_sales DESC
LIMIT 10;

-- 5.2 Customer Lifetime Value (CLV) Overview
SELECT
    customer_id,
    customer_name,
    MIN(order_date)            AS first_order_date,
    MAX(order_date)            AS last_order_date,
    COUNT(DISTINCT order_id)   AS total_orders,
    ROUND(SUM(sales), 2)       AS lifetime_sales,
    ROUND(AVG(sales), 2)       AS avg_order_value
FROM superstore_orders
GROUP BY customer_id, customer_name
ORDER BY lifetime_sales DESC;

-- 5.3 Repeat vs One-Time Customers
SELECT
    CASE
        WHEN order_count = 1 THEN 'One-Time Customer'
        WHEN order_count BETWEEN 2 AND 5 THEN 'Repeat Customer'
        ELSE 'Loyal Customer'
    END AS customer_type,
    COUNT(*) AS customer_count
FROM (
    SELECT customer_id, COUNT(DISTINCT order_id) AS order_count
    FROM superstore_orders
    GROUP BY customer_id
) customer_summary
GROUP BY customer_type;

-- 5.4 Customers with Negative Profit (Loss-Making)
SELECT
    customer_id,
    customer_name,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit
FROM superstore_orders
GROUP BY customer_id, customer_name
HAVING SUM(profit) < 0
ORDER BY total_profit ASC;


-- ============================================================
-- SECTION 6: PRODUCT ANALYSIS
-- ============================================================

-- 6.1 Top 10 Products by Sales
SELECT
    product_id,
    product_name,
    category,
    sub_category,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit,
    SUM(quantity) AS units_sold
FROM superstore_orders
GROUP BY product_id, product_name, category, sub_category
ORDER BY total_sales DESC
LIMIT 10;

-- 6.2 Loss-Making Products (Negative Profit)
SELECT
    product_id,
    product_name,
    category,
    sub_category,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit
FROM superstore_orders
GROUP BY product_id, product_name, category, sub_category
HAVING SUM(profit) < 0
ORDER BY total_profit ASC
LIMIT 15;

-- 6.3 Impact of Discount on Profit
SELECT
    CASE
        WHEN discount = 0           THEN 'No Discount'
        WHEN discount BETWEEN 0.01 AND 0.20 THEN '1–20%'
        WHEN discount BETWEEN 0.21 AND 0.40 THEN '21–40%'
        ELSE 'Above 40%'
    END AS discount_band,
    COUNT(*) AS order_lines,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit,
    ROUND(AVG(profit), 2) AS avg_profit_per_line
FROM superstore_orders
GROUP BY discount_band
ORDER BY discount_band;


-- ============================================================
-- SECTION 7: SHIPPING ANALYSIS
-- ============================================================

-- 7.1 Average Shipping Days by Ship Mode
SELECT
    ship_mode,
    ROUND(AVG(shipping_days), 1) AS avg_shipping_days,
    COUNT(*) AS total_shipments
FROM superstore_orders
GROUP BY ship_mode
ORDER BY avg_shipping_days;

-- 7.2 Orders with Unusually Long Shipping (> 7 days)
SELECT
    order_id,
    customer_name,
    ship_mode,
    order_date,
    ship_date,
    shipping_days
FROM superstore_orders
WHERE shipping_days > 7
ORDER BY shipping_days DESC;

-- 7.3 Ship Mode Preference by Segment
SELECT
    segment,
    ship_mode,
    COUNT(*) AS order_count,
    ROUND(SUM(sales), 2) AS total_sales
FROM superstore_orders
GROUP BY segment, ship_mode
ORDER BY segment, order_count DESC;


-- ============================================================
-- SECTION 8: GEOGRAPHY ANALYSIS
-- ============================================================

-- 8.1 Top 10 States by Sales
SELECT
    state,
    region,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit,
    COUNT(DISTINCT order_id) AS total_orders
FROM superstore_orders
GROUP BY state, region
ORDER BY total_sales DESC
LIMIT 10;

-- 8.2 Top 10 Cities by Profit
SELECT
    city,
    state,
    region,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit
FROM superstore_orders
GROUP BY city, state, region
ORDER BY total_profit DESC
LIMIT 10;

-- 8.3 States with Negative Profit
SELECT
    state,
    region,
    ROUND(SUM(sales),  2) AS total_sales,
    ROUND(SUM(profit), 2) AS total_profit
FROM superstore_orders
GROUP BY state, region
HAVING SUM(profit) < 0
ORDER BY total_profit ASC;


-- ============================================================
-- SECTION 9: VIEWS FOR POWER BI / REPORTING
-- ============================================================

-- View 1: Monthly KPI Summary
CREATE VIEW vw_monthly_kpis AS
SELECT
    order_year,
    order_month,
    order_month_name,
    COUNT(DISTINCT order_id)                         AS total_orders,
    COUNT(DISTINCT customer_id)                      AS unique_customers,
    ROUND(SUM(sales), 2)                             AS total_sales,
    ROUND(SUM(profit), 2)                            AS total_profit,
    ROUND(SUM(profit) / SUM(sales) * 100, 2)        AS profit_margin_pct,
    ROUND(AVG(shipping_days), 1)                     AS avg_shipping_days
FROM superstore_orders
GROUP BY order_year, order_month, order_month_name;

-- View 2: Customer 360 Summary
CREATE VIEW vw_customer_360 AS
SELECT
    customer_id,
    customer_name,
    segment,
    MIN(order_date)                  AS first_purchase_date,
    MAX(order_date)                  AS last_purchase_date,
    COUNT(DISTINCT order_id)         AS total_orders,
    SUM(quantity)                    AS total_units,
    ROUND(SUM(sales), 2)             AS total_sales,
    ROUND(SUM(profit), 2)            AS total_profit,
    ROUND(AVG(discount) * 100, 2)   AS avg_discount_pct
FROM superstore_orders
GROUP BY customer_id, customer_name, segment;

-- View 3: Product Performance Summary
CREATE VIEW vw_product_performance AS
SELECT
    product_id,
    product_name,
    category,
    sub_category,
    COUNT(DISTINCT order_id)        AS orders_containing_product,
    SUM(quantity)                   AS units_sold,
    ROUND(SUM(sales), 2)            AS total_sales,
    ROUND(SUM(profit), 2)           AS total_profit,
    ROUND(AVG(discount) * 100, 2)  AS avg_discount_pct
FROM superstore_orders
GROUP BY product_id, product_name, category, sub_category;
