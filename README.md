[README.md]
# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
drop table retail_sales;
create table Retail_Sales(
transactions_id integer primary key ,
customer_id	INTEGER,
gender	VARCHAR2(10),
age	integer,
categories VARCHAR2(11),
quantiy	INTEGER,
price_per_unit	float,
cogs float	,
total_sale float,
datetime timestamp
);

```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT owner, table_name
FROM all_tables
WHERE table_name = 'RETAIL_SALES';


--display first 10 rows to understand the data
SELECT *
FROM retail_sales
FETCH FIRST 10 ROWS ONLY;

--check how many rows
SELECT count(*)
FROM retail_sales;
--check if the primary key is valid
SELECT * 
FROM retail_sales
where transactions_id is null;

--check for nulls
SELECT * 
FROM retail_sales
where customer_id is null
or gender is null
or age is null
or categories is null
or quantiy is null
or price_per_unit is null
or total_sale is null
or datetime is null

--delete null rows
delete from retail_sales
where customer_id is null
or gender is null
or age is null
or categories is null
or quantiy is null
or price_per_unit is null
or total_sale is null
or datetime is null;
--how many sales after cleaning
select count(*) from retail_sales;

```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE datetime>= TIMESTAMP '2022-11-05 00:00:00'
  AND datetime <  TIMESTAMP '2022-11-06 00:00:00';

```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
select *
from retail_sales
where categories='Clothing'
and datetime>= TIMESTAMP '2022-11-01 00:00:00'
  AND datetime <  TIMESTAMP '2022-12-01 00:00:00'
  and quantiy =4;


```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
select categories,sum(total_sale) as "total_sales",count(*) as "num_of_orders"
from retail_sales 
group by categories;

```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
select round(avg(age),2) as "avg_age"
from retail_sales
where categories='Beauty';

```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
select categories,gender,count(*) as "num_of_transactions"
from retail_sales
group by categories,gender
order by categories;

```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT
  years,
  months,
  total_sales
FROM (
  SELECT
    TO_CHAR(datetime, 'YYYY') AS years,
    TO_CHAR(datetime, 'MM')   AS months,
    SUM(total_sale) AS total_sales
  FROM retail_sales
  GROUP BY
    TO_CHAR(datetime, 'YYYY'),
    TO_CHAR(datetime, 'MM')
)
ORDER BY total_sales DESC;

```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT customer_id,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY SUM(total_sale) DESC
FETCH FIRST 5 ROWS ONLY;

```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
select categories,count(distinct(customer_id))
from retail_sales
group by categories;

```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hour_sale AS (
  SELECT
    rs.*,
    CASE
      WHEN EXTRACT(HOUR FROM rs.datetime) < 12 THEN 'Morning'
      WHEN EXTRACT(HOUR FROM rs.datetime) BETWEEN 12 AND 17 THEN 'Afternoon'
      ELSE 'Evening'
    END AS shift
  FROM retail_sales rs
)
SELECT
  shift,
  COUNT(*) AS "Total_orders"
FROM hour_sale
GROUP BY shift;


```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

