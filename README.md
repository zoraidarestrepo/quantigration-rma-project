[QuantigrationRMA_fromCSV.sql](https://github.com/user-attachments/files/23535775/QuantigrationRMA_fromCSV.sql)# Quantigration RMA

### Project Overview

A MySQL-based implementation of the Quantigration RMA database, built from the ERD and populated with CSV data. Includes table creation, foreign key relationships, data imports, multi-table joins, and analytical queries to model and analyze real RMA workflows.

### Data Sources

Customers.csv – Customer demographic and contact data

Orders.csv – Customer order transactions

RMA.csv – Return merchandise records

Collaborators.csv – Employee data for RMA processing workflows

Vendors.csv – Vendor information (used only if included in module materials)

Quantigration RMA ERD – Defines the relational schema and table relationships

### Tools

MySQL Workbench/ MySQL – Database creation, SQL queries, data imports/exports [Download Here](https://github.com/user-attachments/files/23535381/QuantigrationRMA.sql)

Codio Virtual Lab – Execution environment for building and testing SQL 

CSV Import Files – Used to populate database tables (Customers, Orders, RMA) 
Customers [Download Here](https://github.com/user-attachments/files/23535414/customers.csv)
Orders [Download Here](https://github.com/user-attachments/files/23535427/orders.csv)
RMA [Download Here](https://github.com/user-attachments/files/23535446/rma.csv)

ERD Diagram – Blueprint for table structure and relationships 

SQL Scripts – Schema creation, joins, updates, inserts, deletes, and views [Download Here](https://github.com/user-attachments/files/23535783/QuantigrationRMA_fromCSV.sql)

### Data Cleaning & Preparation

- Validated CSV structure and headers
- Checked for missing values and set NULL where appropriate
- Standardized data types to match SQL schema
- Removed duplicates to protect primary keys
- Verified CustomerID and OrderID foreign key alignment
- Cleaned text fields (spacing, casing, apostrophes, invalid chars)
- Prepared clean datasets for MySQL import
- Verified row counts and table relationships after loading

### Exploratory Data Analysis
The Exploratory Data Analysis phase focused on understanding the structure, relationships, and patterns within the Quantigration RMA datasets (Customers, Orders, and RMA). This analysis helped validate data quality, identify trends, and prepare for deeper SQL-based insights.

- Reviewed dataset structure and verified foreign key alignment
- Analyzed customer, order, and RMA distributions
- Checked data completeness, duplicates, and date formatting
- Explored return trends by product, customer, and status
- Validated relationships using SQL joins (Customers → Orders → RMA)

