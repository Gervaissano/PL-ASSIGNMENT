Sunrise Supermarket Database
Student Information

Name: Sano Gervais
Student ID: 20251SEN160
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

5+ customers
8+ products
Products from 3+ categories
15+ orders
25+ order items
Orders distributed across multiple dates


Main SQL Operations
-------------------
The project includes:

Creating the SUNRISE_SUPERMARKET Oracle user and granting required privileges.
Creating relational tables with appropriate Oracle data types and constraints.
Inserting and populating supermarket data.
Retrieving information using SELECT queries.
Joining related tables to analyze customers, orders, and products.
Using queries to verify records and analyze database information.

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
├── 01_create_tables.sql
├── 02_insert_data.sql
├── 03_queries.sql
└── screenshots/
    └── database_results.png

