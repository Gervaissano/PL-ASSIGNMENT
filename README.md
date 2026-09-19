<p align="left">
  <img src="auca logo.png"
       alt="Adventist University of Central Africa (AUCA) Logo"
       width="520"
       height="520" border=2px;>
</p>

Sunrise Supermarket Database

Student Information

Name: Sano Gervais

Student ID: 20251SEN160

Course: Database development with PL/SQL 

Database: Oracle Database 21c


Project Summary
---------------
The Sunrise Supermarket Database is a relational database developed to manage customers, products, orders, and order items. It helps supermarket management organize customer information, track products and categories, monitor purchases, and analyze sales across different dates.
The project demonstrates practical use of Oracle SQL, relational database design, constraints, data manipulation, table relationships, joins, and data retrieval.

Database Structure
------------------
The system consists of four main tables:

CUSTOMERS – stores customer details.
PRODUCTS – stores products and their categories.
ORDERS – stores customer orders and dates.
ORDER_ITEMS – stores products included in each order.

Primary keys and foreign keys are used to maintain relationships and data integrity.

Data Requirements
-----------------
The database contains:

[5+ customers](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/Order_items%20table.xls)
[8+ products](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/Product%20table.xls)
Products from 3+ categories
[15+ orders](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/Orders%20table.xls)
[25+ order items](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/Order_items%20table.xls)
Orders distributed across multiple dates


Main SQL Operations
-------------------
The project includes:

Creating the SUNRISE_SUPERMARKET Oracle user and granting required privileges.
Creating relational tables with appropriate Oracle data types and constraints.
Inserting and populating supermarket data.
Retrieving information using SELECT queries.
Joining related tables to analyze [customers](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/JOIN%20Query%201%20%E2%80%94%20Orders%20with%20customer%20information.sql), [orders](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/JOIN%20Query%203%20%E2%80%94%20All%20customers%20and%20their%20orders.sql), and [products](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/JOIN%20Query%202%20%E2%80%94%20Order%20items%20with%20product%20information.sql).
[Using queries to verify records and analyze database information](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/CTE%20Query%20%E2%80%94%20Customers%20above%20average%20spending.sql).


Window Functions

The project uses Oracle SQL window functions to perform advanced analysis of customer purchases and sales trends without grouping or losing individual records. [Window Function 1 — Rank Customer](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/Window%20Function%201%20%E2%80%94%20Rank%20customers%20by%20total%20spending.sql) uses RANK() to rank customers according to their total spending, helping identify customers based on their purchase value. [Window Function 2 — Number Each Customer's Orders](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/Window%20Function%202%20%E2%80%94%20Number%20each%20customer's%20orders.sql) uses ROW_NUMBER() with PARTITION BY to assign a sequential number to each customer's orders according to the order date, showing whether an order was the customer's first, second, third, and so on. [Window Function 3 — Running Revenue Over Time](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/Window%20Function%203%20%E2%80%94%20Running%20revenue%20over%20time.sql) uses SUM() OVER (ORDER BY ...) to calculate cumulative revenue across different dates, allowing revenue growth to be tracked over time. [Window Function 4 — Days Between Customer Orders](https://github.com/Gervaissano/PL-ASSIGNMENT/blob/test-1/Window%20Function%204%20%E2%80%94%20Days%20between%20customer%20orders.sql) uses LAG() to compare each customer's current order date with their previous order date and calculate the number of days between purchases, helping analyze customer purchasing frequency.



Example user setup:
CREATE USER sunrise_supermarket IDENTIFIED BY "kamana@2005";

GRANT CREATE SESSION, CREATE TABLE, CREATE SEQUENCE,
      CREATE VIEW, CREATE PROCEDURE, CREATE TRIGGER
TO sunrise_supermarket;

ALTER USER sunrise_supermarket QUOTA UNLIMITED ON USERS;
How to Run
Requirements
Oracle Database 21c
SQL*Plus or Oracle SQL Developer
Running Oracle database service
Appropriate privileges to create the database user
1. Connect as SYSTEM
sqlplus system/your_system_password
2. Create the Project User

Run the user setup SQL shown above.

3. Connect as SUNRISE_SUPERMARKET
sqlplus sunrise_supermarket/"kamana@2005"

Or create a connection in Oracle SQL Developer using the project username and password.

4. Run the SQL Scripts
   -------------------
Execute the scripts in this order:
01_create_tables.sql
02_insert_data.sql
03_queries.sql

In SQL*Plus:
@01_create_tables.sql
@02_insert_data.sql
@03_queries.sql

Verification
After execution, verify the tables and data:
SELECT table_name
FROM user_tables
ORDER BY table_name;

SELECT COUNT(*) FROM customers;
SELECT COUNT(*) FROM products;
SELECT COUNT(*) FROM orders;
SELECT COUNT(*) FROM order_items;

These queries confirm that the tables were created and the required data was populated.

Technologies
Oracle Database 21c
SQL
SQL*Plus
Oracle SQL Developer
Git & GitHub

Learning Outcomes
This project demonstrates skills in relational database design, Oracle database user management, privileges, table creation, primary and foreign keys, data integrity, SQL queries, joins, data insertion, aggregation, and database management.

Repository Structure
Sunrise-Supermarket-Database/
│
├── README.md
│
├── 01_database_setup.sql
│   └── Create Oracle user and grant privileges
│
├── 02_create_tables.sql
│   └── Create tables, primary keys, foreign keys, and constraints
│
├── 03_insert_data.sql
│   └── Populate customers, products, orders, and order items
│
├── 04_basic_queries.sql
│   └── Retrieve and filter database information
│
├── 05_aggregate_queries.sql
│   └── Analyze sales using COUNT, SUM, AVG, GROUP BY, and HAVING
│
├── 06_join_queries.sql
│   └── Combine related data using SQL JOIN operations
│
├── 07_window_functions.sql
│   ├── Rank Customers
│   ├── Number Each Customer's Orders
│   ├── Running Revenue Over Time
│   └── Days Between Customer Orders
│
└── 08_verification.sql
    └── Verify tables, records, and required data counts
