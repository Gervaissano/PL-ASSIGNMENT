##Sunrise Supermarket Database##
Student Information

Name: Sano Gervais
Student ID: 20251SEN160
Database System: MySQL
Version: 5.5.28

Project Overview

This project implements a relational database for Sunrise Supermarket. It manages customers, products, orders, and order items.

The database was populated with 5 customers, 8 products across 4 categories, 15 orders, and 30 order items across multiple dates.

SQL queries were used to analyze customer orders, product purchases, customer spending, and sales trends.

Database Structure

The database contains four tables:

customers — stores customer information.
products — stores product details, categories, and prices.
orders — stores customer orders and order dates.
order_items — stores products and quantities included in each order.
Queries Implemented
1. JOIN Queries

Used to retrieve related information from multiple tables.

SELECT o.order_id, c.customer_name, c.city, o.order_date
FROM orders o
INNER JOIN customers c
ON o.customer_id = c.customer_id;
SELECT oi.order_item_id, oi.order_id, p.product_name,
       p.category, p.price, oi.quantity
FROM order_items oi
INNER JOIN products p
ON oi.product_id = p.product_id;
SELECT c.customer_id, c.customer_name, c.city,
       o.order_id, o.order_date
FROM customers c
LEFT JOIN orders o
ON c.customer_id = o.customer_id;
2. Customer Spending Analysis

A subquery was used to calculate customer spending and identify customers whose spending is above the average.

SELECT customer_id, customer_name, total_spend
FROM (
    SELECT c.customer_id, c.customer_name,
           SUM(oi.quantity * p.price) AS total_spend
    FROM customers c
    JOIN orders o ON c.customer_id = o.customer_id
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN products p ON oi.product_id = p.product_id
    GROUP BY c.customer_id, c.customer_name
) AS customer_totals
WHERE total_spend > (
    SELECT AVG(total_spend)
    FROM (
        SELECT SUM(oi.quantity * p.price) AS total_spend
        FROM orders o
        JOIN order_items oi ON o.order_id = oi.order_id
        JOIN products p ON oi.product_id = p.product_id
        GROUP BY o.customer_id
    ) AS averages
);
3. Order Analysis

Because MySQL 5.5.28 does not support CTEs or window functions, equivalent subqueries and joins were used.

The analysis includes:

Customer order ranking
Order numbering
Running revenue totals
Days between customer orders

Example for finding days between orders:

SELECT
    o.customer_id,
    o.order_id,
    o.order_date,
    p.previous_order_date,
    DATEDIFF(o.order_date, p.previous_order_date)
        AS days_between_orders
FROM orders o
LEFT JOIN (
    SELECT
        o1.customer_id,
        o1.order_id,
        o1.order_date,
        MAX(o2.order_date) AS previous_order_date
    FROM orders o1
    LEFT JOIN orders o2
        ON o1.customer_id = o2.customer_id
        AND (
            o2.order_date < o1.order_date
            OR (o2.order_date = o1.order_date
                AND o2.order_id < o1.order_id)
        )
    GROUP BY o1.customer_id, o1.order_id, o1.order_date
) p
ON o.customer_id = p.customer_id
AND o.order_id = p.order_id
WHERE p.previous_order_date IS NOT NULL
ORDER BY o.customer_id, o.order_date;
How to Run


1. Start MySQL
Open the MySQL command line or MySQL Workbench.
3. Create the database
CREATE DATABASE sunrise_supermarket;
5. Select the database
USE sunrise_supermarket;
7. Run the SQL files

If the project is organized into separate files, run them in this order:

01_create_tables.sql
02_insert_data.sql
03_join_queries.sql
04_customer_spending.sql
05_order_analysis.sql

From the MySQL command line, use:

SOURCE C:/path/to/01_create_tables.sql;
SOURCE C:/path/to/02_insert_data.sql;

Then execute the analysis files:

SOURCE C:/path/to/03_join_queries.sql;
SOURCE C:/path/to/04_customer_spending.sql;
SOURCE C:/path/to/05_order_analysis.sql;
5. Verify the database
SHOW TABLES;

Check the data:

SELECT * FROM customers;
SELECT * FROM products;
SELECT * FROM orders;
SELECT * FROM order_items;
Compatibility

This project is designed for MySQL 5.5.28. Queries were written to work with the limitations of this version, particularly the absence of CTEs and window functions.
