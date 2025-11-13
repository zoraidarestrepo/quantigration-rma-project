# Quantigration RMA

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

### Data Analysis

1. Customer Order Behavior

Analysis showed clear patterns in how customers interact with the company:

- Some customers place multiple orders, while others have only a single transaction.
- High-volume customers tend to generate more RMAs, suggesting a direct relationship between order frequency and return frequency.
- Customer information was complete and correctly mapped to Orders, ensuring reliable trend analysis.

```sql
SELECT 
    c.CustomerID,
    CONCAT(c.FirstName, ' ', c.LastName) AS CustomerName,
    COUNT(o.OrderID) AS TotalOrders
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerID, CustomerName
ORDER BY TotalOrders DESC;
```

Key insight:
Customers with more frequent purchase activity naturally exhibit higher return counts, indicating a proportional relationship rather than a quality issue.

2. Product Return Trends

By joining Orders and RMA records, product-level return patterns were identified:

- Certain products appeared more frequently in RMA records than others.
- Return reasons varied by product, suggesting differences in quality, customer expectation, or use cases.
- Products with repeated similar return reasons may require quality review or updated customer instructions.

```sql
SELECT 
    o.ProductName,
    COUNT(r.RMAID) AS TotalReturns
FROM Orders o
JOIN RMA r ON o.OrderID = r.OrderID
GROUP BY o.ProductName
ORDER BY TotalReturns DESC;
```

Key insight:
A small subset of products accounted for a large share of RMAs, following a classic “80/20” pattern—valuable for targeting improvements.

3. Return Reasons Analysis

Return reasons provided insight into why customers initiate RMAs:

- Common themes included defective components, performance issues, and incorrect orders.
- Some return reasons were operational (e.g., “ordered wrong model”), while others pointed to technical failure.
- Standardizing return reasons helped simplify classification and reporting.

```sql
SELECT 
    ReturnReason,
    COUNT(*) AS Frequency
FROM RMA
GROUP BY ReturnReason
ORDER BY Frequency DESC;
```

Key insight:
Many returns were preventable (incorrect orders, misunderstanding of product specs), suggesting opportunities for improved product descriptions or customer guidance.

4. RMA Status Workflow

The RMA Status field (“Open,” “Closed,” “In Progress,” etc.) revealed operational insights:

- Most RMAs progressed through the workflow as expected.
- Open and In-Progress RMAs clustered around certain dates, indicating workload peaks.
- Closed RMAs were consistent with proper processing procedures.

```sql
SELECT 
    Status,
    COUNT(*) AS Total
FROM RMA
GROUP BY Status
ORDER BY Total DESC;
```

Key insight:
Tracking RMA status over time can help identify bottlenecks in the return process and improve turnaround time.

5. Time Between Order and Return

The time interval between order date and return date highlighted potential issues:

- Very short return intervals may signal product quality issues or customer dissatisfaction.
- Longer intervals often indicated slow progression to identifying a defect or delayed customer action.
- Patterns varied by product type.

```SQL
SELECT 
    r.RMAID,
    o.ProductName,
    o.OrderDate,
    r.ReturnDate,
    DATEDIFF(r.ReturnDate, o.OrderDate) AS DaysBetween
FROM RMA r
JOIN Orders o ON r.OrderID = o.OrderID
ORDER BY DaysBetween DESC;
```

Key insight:
Products with consistently short order-to-return times may require deeper investigation for underlying defects.

6. End-to-End Relationship Analysis

By linking Customers → Orders → RMA, the complete return behavior could be evaluated:

- Identified which customers generated the most RMAs.
- Mapped which products were most frequently involved in returns.
- Helped determine whether returns stemmed from isolated events or persistent issues tied to specific customers, orders, or products.

```sql
SELECT
    r.RMAID,
    r.Status,
    r.ReturnReason,
    r.ReturnDate,
    o.OrderID,
    o.ProductName,
    o.OrderDate,
    c.CustomerID,
    CONCAT(c.FirstName, ' ', c.LastName) AS CustomerName
FROM RMA r
JOIN Orders o ON r.OrderID = o.OrderID
JOIN Customers c ON o.CustomerID = c.CustomerID
ORDER BY r.RMAID;
```

Key insight:
The integrated relational model enables full traceability—critical for quality control, customer support, and operational decision-making.
