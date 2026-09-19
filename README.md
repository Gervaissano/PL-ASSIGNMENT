# Sunrise Supermarket Database

## Student Information

**Name:** Sano Gervais  
**Student ID:** 20251SEN160  
**Database System:** MySQL  
**MySQL Version:** 5.5.28

---

## 1. Project Summary

This project implements a relational database for **Sunrise Supermarket**.

The database is designed to help management understand:

- Who their customers are
- What products customers purchase
- The categories and prices of products
- Customer orders and order dates
- The quantity of products purchased
- Customer spending
- Sales revenue and sales trends over time
- The time between a customer's orders

The database contains four main tables:

- `customers` — stores customer information.
- `products` — stores product information, categories, and prices.
- `orders` — stores customer orders and order dates.
- `order_items` — stores the products and quantities included in each order.

The database was populated with:

- **5 customers**
- **8 products**
- **4 product categories**
- **15 orders**
- **30 order items**
- Orders distributed across multiple dates

---

# 2. Database Structure

The relationships between the tables are:

```text
customers
    |
    | 1
    |
    |------< orders
              |
              | 1
              |
              |------< order_items
                         |
                         | >------ products
```

A customer can place many orders.

An order can contain many order items.

Each order item refers to one product.

---

# 3. Files in the Project

The SQL queries can be separated into the following files:

```text
Sunrise_Supermarket/
│
├── 01_create_tables.sql
├── 02_insert_customers.sql
├── 03_insert_products.sql
├── 04_insert_orders.sql
├── 05_insert_order_items.sql
├── 06_join_queries.sql
├── 07_customer_spending.sql
└── 08_order_history.sql
```

---

# 4. How to Run the Project

## Step 1: Start MySQL

Start the MySQL server and open the MySQL command-line client.

Check the MySQL version:

```sql
SELECT VERSION();
```

Expected version:

```text
5.5.28
```

---

## Step 2: Create and select the database

Run:

```sql
CREATE DATABASE sunrise_supermarket;
```

Then select it:

```sql
USE sunrise_supermarket;
```

Check the selected database:

```sql
SELECT DATABASE();
```

Expected result:

```text
sunrise_supermarket
```

---

# 5. Create the Tables

Run the table creation script:

```sql
SOURCE C:/Sunrise_Supermarket/01_create_tables.sql;
```

The tables created are:

```sql
customers
products
orders
order_items
```

To verify them:

```sql
SHOW TABLES;
```

---

# 6. Insert the Data

Run the data files in the following order:

```sql
SOURCE C:/Sunrise_Supermarket/02_insert_customers.sql;

SOURCE C:/Sunrise_Supermarket/03_insert_products.sql;

SOURCE C:/Sunrise_Supermarket/04_insert_orders.sql;

SOURCE C:/Sunrise_Supermarket/05_insert_order_items.sql;
```

Verify the data:

```sql
SELECT * FROM customers;

SELECT * FROM products;

SELECT * FROM orders;

SELECT * FROM order_items;
```

---

# 7. JOIN Queries

The JOIN queries are stored in:

```text
06_join_queries.sql
```

Run the file:

```sql
SOURCE C:/Sunrise_Supermarket/06_join_queries.sql;
```

### Query 1: Orders with Customer Information

```sql
SELECT
    o.order_id,
    c.customer_name,
    c.city,
    o.order_date
FROM orders o
INNER JOIN customers c
    ON o.customer_id = c.customer_id
ORDER BY o.order_date;
```

### Query 2: Order Items with Product Information

```sql
SELECT
    oi.order_item_id,
    oi.order_id,
    p.product_name,
    p.category,
    p.price,
    oi.quantity
FROM order_items oi
INNER JOIN products p
    ON oi.product_id = p.product_id
ORDER BY oi.order_id;
```

### Query 3: All Customers and Their Orders

```sql
SELECT
    c.customer_id,
    c.customer_name,
    c.city,
    o.order_id,
    o.order_date
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
ORDER BY c.customer_id, o.order_date;
```

---

# 8. Customer Spending Query

The original assignment requested a CTE. However, **MySQL 5.5.28 does not support Common Table Expressions (CTEs)**.

Therefore, a subquery was used instead.

Run:

```sql
SOURCE C:/Sunrise_Supermarket/07_customer_spending.sql;
```

The query is:

```sql
SELECT
    customer_id,
    customer_name,
    total_spend
FROM
(
    SELECT
        c.customer_id,
        c.customer_name,
        SUM(oi.quantity * p.price) AS total_spend
    FROM customers c
    INNER JOIN orders o
        ON c.customer_id = o.customer_id
    INNER JOIN order_items oi
        ON o.order_id = oi.order_id
    INNER JOIN products p
        ON oi.product_id = p.product_id
    GROUP BY
        c.customer_id,
        c.customer_name
) AS customer_totals
WHERE total_spend >
(
    SELECT AVG(total_spend)
    FROM
    (
        SELECT
            c.customer_id,
            SUM(oi.quantity * p.price) AS total_spend
        FROM customers c
        INNER JOIN orders o
            ON c.customer_id = o.customer_id
        INNER JOIN order_items oi
            ON o.order_id = oi.order_id
        INNER JOIN products p
            ON oi.product_id = p.product_id
        GROUP BY c.customer_id
    ) AS averages
)
ORDER BY total_spend DESC;
```

This calculates each customer's total spending using:

```text
quantity × product price
```

and returns customers whose spending is above the average customer spending.

---

# 9. Order History Query

The original assignment requested the `LAG()` window function to calculate the number of days between a customer's current and previous order.

However, **MySQL 5.5.28 does not support window functions such as `LAG()`**.

Therefore, a self-join was used to achieve the same result.

Run:

```sql
SOURCE C:/Sunrise_Supermarket/08_order_history.sql;
```

Query:

```sql
SELECT
    o.customer_id,
    o.order_id,
    o.order_date,
    p.previous_order_date,
    DATEDIFF(
        o.order_date,
        p.previous_order_date
    ) AS days_between_orders
FROM orders o
LEFT JOIN
(
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
            OR (
                o2.order_date = o1.order_date
                AND o2.order_id < o1.order_id
            )
        )
    GROUP BY
        o1.customer_id,
        o1.order_id,
        o1.order_date
) AS p
    ON o.customer_id = p.customer_id
    AND o.order_id = p.order_id
WHERE p.previous_order_date IS NOT NULL
ORDER BY
    o.customer_id,
    o.order_date,
    o.order_id;
```

`DATEDIFF()` calculates the number of days between the current order and the customer's previous order.

---

# 10. MySQL 5.5.28 Compatibility

This project was developed using **MySQL 5.5.28**.

Some SQL features from newer MySQL versions cannot be used.

For example, MySQL 5.5.28 does **not** support:

```sql
WITH
```

Common Table Expressions (CTEs), or:

```sql
LAG()
RANK()
ROW_NUMBER()
SUM() OVER()
```

Window functions.

Therefore, the project uses compatible alternatives such as:

- Subqueries
- Self-joins
- `GROUP BY`
- Aggregate functions
- `DATEDIFF()`
- `ORDER BY`

These alternatives allow the required database operations to be performed on MySQL 5.5.28.

---

# 11. Useful Verification Commands

Check all databases:

```sql
SHOW DATABASES;
```

Select the project database:

```sql
USE sunrise_supermarket;
```

Check tables:

```sql
SHOW TABLES;
```

Describe a table:

```sql
DESCRIBE customers;
```

Check customers:

```sql
SELECT * FROM customers;
```

Check products:

```sql
SELECT * FROM products;
```

Check orders:

```sql
SELECT * FROM orders;
```

Check order items:

```sql
SELECT * FROM order_items;
```

Count customers:

```sql
SELECT COUNT(*) FROM customers;
```

Count products:

```sql
SELECT COUNT(*) FROM products;
```

Count orders:

```sql
SELECT COUNT(*) FROM orders;
```

Count order items:

```sql
SELECT COUNT(*) FROM order_items;
```

---

# 12. Execution Order

For a new installation, execute the files in this order:

```text
1. 01_create_tables.sql
2. 02_insert_customers.sql
3. 03_insert_products.sql
4. 04_insert_orders.sql
5. 05_insert_order_items.sql
6. 06_join_queries.sql
7. 07_customer_spending.sql
8. 08_order_history.sql
```

Example:

```sql
CREATE DATABASE sunrise_supermarket;

USE sunrise_supermarket;

SOURCE C:/Sunrise_Supermarket/01_create_tables.sql;

SOURCE C:/Sunrise_Supermarket/02_insert_customers.sql;

SOURCE C:/Sunrise_Supermarket/03_insert_products.sql;

SOURCE C:/Sunrise_Supermarket/04_insert_orders.sql;

SOURCE C:/Sunrise_Supermarket/05_insert_order_items.sql;

SOURCE C:/Sunrise_Supermarket/06_join_queries.sql;

SOURCE C:/Sunrise_Supermarket/07_customer_spending.sql;

SOURCE C:/Sunrise_Supermarket/08_order_history.sql;
```

---

## Author

**Sano Gervais**  
**Student ID:** 20251SEN160  
**Database:** Sunrise Supermarket  
**MySQL Version:** 5.5.28