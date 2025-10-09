# Global Electronics Retailer Data Warehouse Project

## Overview

This project is a **data warehouse solution** developed as part of the *B9DA111 – Data Storage Solutions for Data Analytics* module in the **MSc in Data Analytics** program.
The project demonstrates the end-to-end design and implementation of a **data warehousing and analytics system** using the **Global Electronics Retailer dataset** (2016–2021) from Maven Analytics.

The goal was to build a robust analytical environment that supports data-driven decision-making for sales managers by enabling insights into sales trends, performance, and customer behavior.

---

## Objectives

* Design a **Star Schema** for the data warehouse.
* Implement **ETL (Extract, Transform, Load)** pipelines using **SSIS** and **SQL Server**.
* Develop **SSRS reports** and **Tableau dashboards** for sales analysis.
* Integrate a **Graph Database (Neo4j)** for relationship-based analysis.
* Compare **Relational (SQL)** and **Graph (CQL)** query performance.

---

## Architecture

* **Schema Type:** Star Schema
* **Tools Used:**

  * Microsoft SQL Server & SSMS
  * SQL Server Integration Services (SSIS)
  * SQL Server Reporting Services (SSRS)
  * Tableau
  * Neo4j

---

## ETL Process

The ETL pipelines were developed using **SSIS**:

* Data extraction from OLTP sources.
* Data cleaning, transformation, and type conversions.
* Loading data into fact and dimension tables (`Customers_Dim`, `Stores_Dim`, `Products_Dim`, `Date_Dim`, `Sales_Fact`).
* Deduplication and surrogate key lookups ensured **data integrity** and **consistency**.

---

## Visualizations & Reports

* **Sales Dashboard** (Tableau):

  * Overall sales summary by continent, year, and brand.
  * Top products and customer segmentation analysis.
  * Monthly and geographic sales trends.

* **SSRS Reports:**

  * Gender-based sales and quantity comparisons.
  * Yearly and continental performance.
  * Brand and product analysis.
  * Transaction-level detailed reports for managerial insights.

---

## Graph Database (Neo4j)

Neo4j was implemented to visualize and query relationships between customers, stores, and products.
Example queries include:

* Top 5 customers by order amount
* High-value orders by category
* Orders with quantity ≥ 5
  The graph model improved query performance and intuitiveness for relationship-heavy analyses.

---

## Key Insights

* **North America** generated ~55% of total sales.
* **Adventure Works** products dominated sales.
* **Females** had higher average spending despite fewer orders.
* **2019** marked the highest sales year.
* Potential growth areas: **Australia** and parts of **Europe**.

---

## Comparison: SQL vs CQL

| Aspect           | SQL (Relational)     | CQL / Neo4j (Graph)       |
| ---------------- | -------------------- | ------------------------- |
| Data Model       | Tables, foreign keys | Nodes, relationships      |
| Query Complexity | Increases with joins | Simpler for relationships |
| Performance      | Slower on deep joins | Faster for connected data |
| Best Use         | Structured data      | Relationship-heavy data   |

---

## Conclusion

This project successfully delivered a **comprehensive data warehouse solution** integrating **relational, ETL, BI, and graph database technologies**.
It enables sales managers to derive **data-driven insights**, identify **growth opportunities**, and improve **business decisions**.

---

## Team Members

* **Ihenacho Daniel Chiebuka** – 20067317
* **Mrunal Sunil Fulzele** – 20029230
* **Bharath Shakthivel Balaih Raveendran** – 20057027

**Lecturer:** Dr. Shazia A. Afzal
**Institution:** MSc Data Analytics – Dublin Business School - Ireland

