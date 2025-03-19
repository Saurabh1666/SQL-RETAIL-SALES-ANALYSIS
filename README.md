📊 Retail Sales Analysis SQL Project

Project Overview

Project Title: Retail Sales Analysis
Skill Level: Beginner
Database Name: p1_retail_db
This project is designed to demonstrate SQL skills that are essential for data analysts, including data exploration, cleaning, and analysis. The project focuses on setting up a retail sales database, conducting exploratory data analysis (EDA), and answering business-related queries using SQL. It serves as a solid starting point for those who want to strengthen their SQL fundamentals.

Objectives

1️⃣ Database Setup: Create and populate a retail sales database with transaction data.
2️⃣ Data Cleaning: Identify and remove any records containing missing or null values.
3️⃣ Exploratory Data Analysis (EDA): Perform an initial analysis of the dataset to understand trends.
4️⃣ Business Insights: Use SQL queries to extract valuable business insights from sales data.

Project Structure

1️⃣ Database Setup
Creating the Database and Table

The project starts with creating a PostgreSQL database named p1_retail_db and a table retail_sales to store sales data.

CREATE DATABASE sql_project_p1;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
2️⃣ Data Exploration & Cleaning
Checking Total Records in the Dataset

SELECT COUNT(*) FROM retail_sales;
Finding Unique Customers

SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
Identifying All Unique Product Categories

SELECT DISTINCT category FROM retail_sales;
Checking for Missing Values

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
Removing Records with Missing Data

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
3️⃣ Data Analysis & Insights

The following SQL queries answer specific business-related questions based on sales data.

🔹 Retrieve All Sales Made on a Specific Date (2022-11-05)

SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
🔹 Retrieve Clothing Transactions Where Quantity Sold > 4 in November 2022

SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4;
🔹 Calculate Total Sales (total_sale) for Each Category

SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1;
🔹 Find the Average Age of Customers Who Purchased ‘Beauty’ Products

SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty';
🔹 Find Transactions Where total_sale > 1000

SELECT * FROM retail_sales
WHERE total_sale > 1000;
🔹 Count the Number of Transactions per Gender in Each Category

SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1;
🔹 Find the Best-Selling Month in Each Year Based on Average Sale

SELECT 
       year,
       month,
       avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1;
🔹 Find the Top 5 Customers Based on Total Sales

SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
🔹 Count the Unique Customers Who Purchased Items from Each Category

SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;
🔹 Classify Sales into Morning, Afternoon, and Evening Shifts

WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END as shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;
🔍 Findings & Insights

🔸 Customer Demographics: The dataset includes customers from different age groups, with purchases spread across categories like Clothing, Beauty, and Electronics.

🔸 High-Value Transactions: Multiple transactions exceed $1000, indicating the presence of high-value purchases.

🔸 Sales Trends: The best-selling month varies each year, helping identify seasonal trends.

🔸 Customer Behavior: The analysis reveals top-spending customers and the most frequently purchased categories.# SQL-RETAIL-SALES-ANALYSIS
