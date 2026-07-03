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
- **Table Creation**: A table named `retail_sales_analysis` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE TABLE IF NOT EXISTS retail_sales_analysis (
		transactions_id TEXT PRIMARY KEY,
		sale_date TEXT,
		sale_time TEXT,
		customer_id INTEGER,
		gender TEXT,
		age INTEGER,
		category TEXT, 
		quantity INTEGER,
		price_per_unit INTEGER,
		cogs INTEGER,
		total_sale INTEGER
		);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.
- **Sales Count**: Determine the total number of sales in the dataset.
- **Customer Count**: Determine the total number of unique customers in the dataset.
- **Category Count**: Determine the categories in the dataset.



```sql
SELECT COUNT(*)
FROM retail_sales_analysis

SELECT * FROM retail_sales_analysis
WHERE 
	transactions_id IS NULL 
	OR
	sale_date IS NULL
	OR 
	sale_time IS NULL
	OR
	gender IS NULL 
	OR 
	category IS NULL
	OR 
	quantity IS NULL
	OR 
	cogs IS NULL
	OR
	total_sale IS NULL;

DELETE FROM retail_sales_analysis
WHERE 
	transactions_id IS NULL 
	OR
	sale_date IS NULL
	OR 
	sale_time IS NULL
	OR
	gender IS NULL 
	OR 
	category IS NULL
	OR 
	quantity IS NULL
	OR 
	cogs IS NULL
	OR
	total_sale IS NULL;

SELECT COUNT(*) as total_sale FROM retail_sales_analysis

SELECT COUNT(DISTINCT customer_id) as total_sale FROM retail_sales_analysis

SELECT DISTINCT category FROM retail_sales_analysis
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales_analysis
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT *
FROM retail_sales_analysis
WHERE 
category = 'Clothing'
AND 
quantity >= 4
AND
sale_date LIKE '2022-11-%';
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales_analysis
GROUP BY category;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales_analysis
WHERE category = 'Beauty';
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales_analysis
WHERE total_sale > 1000
ORDER BY total_sale;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales_analysis
GROUP 
    BY 
    category,
    gender
ORDER BY 1;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT 
year,
month,
avg_sale
 FROM 
(
	SELECT 
		strftime('%Y',sale_date) as year, 
		strftime('%m',sale_date) as month,
		avg(total_sale) as avg_sale,
		RANK() OVER(
		PARTITION BY strftime('%Y',sale_date) 
		ORDER BY AVG(total_sale) DESC
		) AS sale_rank
	FROM retail_sales_analysis
	GROUP BY year, month
	ORDER BY year, avg_sale DESC
) as t1
WHERE sale_rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT customer_id, sum(total_sale) as highest_total_sale
FROM retail_sales_analysis
GROUP BY customer_id
ORDER BY highest_total_sale DESC 
LIMIT 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_customers
FROM retail_sales_analysis
GROUP BY category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sales
AS 
(
SELECT *,
	CASE	
		WHEN strftime('%H',sale_time) < '12' THEN 'Morning'
		WHEN strftime('%H',sale_time) BETWEEN '12' AND '17' THEN 'Afternoon'
		ELSE 'Evening'
		END as shift
FROM retail_sales_analysis
)
SELECT 
	shift,
	COUNT(*) as total_orders
FROM hourly_sales
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

## Author - **Erin McKenna (erin-mckenna) - Note project adapted from Zero Analyst**
This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. Please note that is project is not original content. It was adapted from Zero Analyst with slight modifcations made due to the fact that I used DB Browser for SQLite. 

### Stay Updated

- **LinkedIn**: [Connect with me professionally](www.linkedin.com/in/erin-mckenna-a78127387)


Thank you for your support, and I look forward to connecting with you!
