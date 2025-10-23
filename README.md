# Customer-360-Analytics-Dashboard

## Table of contents

- [Project Overview](#project-overview)

- [Research Questions](#research-questions)

- [Data Sources](#data-sources)
  
- [Tools](#tools)
- [Data Preparation](#data-preparation)
- [Foundational Analysis (using SQL)](#foundational-analysis-(using-sql))

## Project Overview

This project builds an end-to-end analytics solution using SQL and Power BI. It integrates customer, product, transaction, and support ticket data to provide a 360° view of customer behavior, sales performance, and service effectiveness.

The main goals are:

Analyze customer segments and purchasing behavior.

Identify top-performing products and categories.

Measure customer lifetime value (LTV).

Track support ticket trends and their impact on customer engagement.

Provide interactive Power BI dashboards for data-driven decision-making.

## Research Questions

- **Sales & Product Performance**

Which products and categories generate the highest revenue?

How does average transaction value vary across customer segments and regions?

What is the distribution of Customer Lifetime Value (LTV) across different demographics?

- **Customer Insights**

Do high-engagement customers purchase more frequently or spend more per transaction than low-engagement customers?

How do age, gender, and region influence purchasing behavior?

What are the characteristics of top 10% revenue-generating customers?

- **Support & Service Effectiveness**

Which issue types are most common in support tickets, and how do they affect customer retention?

Is there a relationship between ticket resolution time and customer segment (e.g., are low-engagement customers waiting longer)?

Do customers with more open tickets contribute less to total sales compared to those with fewer/no tickets?

## Data Sources

The dataset is composed of 4 relational tables:

- **Customers**

Fields: CustomerID, Name, Age, Gender, Region, Segment

Description: Contains customer demographic and segmentation data.

- **Transactions**

Fields: TransactionID, CustomerID, ProductID, Date, Quantity, TotalAmount

Description: Records of purchases, including product bought, quantity, and total value.

- **Products**

Fields: ProductID, ProductName, Category, UnitPrice

Description: Product catalog with category and pricing information.

- **Support Tickets**

Fields: TicketID, CustomerID, IssueType, Resolution, Status

Description: Customer support interactions, resolution times, and status.

## Tools

SQL: For data extraction, cleaning, aggregations.

Power BI: For visualization and storytelling.

## Data Preparation

The raw data have been imported from Microsoft Excel into Microsoft Power BI for analysis. In PowerBI, Data profiling technique has been implemented to analyze the data to better understand their  structure, quality, and content.

![data profiling Age](https://github.com/user-attachments/assets/21687d21-21d7-4e80-9f40-b03d4c8804d7)

![Data Profiling resolution time](https://github.com/user-attachments/assets/e402d034-e113-493f-95b6-455809309593)

![Data Profiling Transactions](https://github.com/user-attachments/assets/601cf8d0-445c-480d-898c-19aa6793d6ee)

![Data Profiling Unit Price](https://github.com/user-attachments/assets/5a418e14-ef46-467c-83de-1827e928ad72)

## Foundational Analysis (using SQL)**
  
- **Customer-level Analysis**

*Customer segmentation counts*
   ```SQL
  SELECT Segment, COUNT(*) AS CustomerCount
FROM Customers
GROUP BY Segment;
```
|Segment|Customer Count|
|----------|-------|
|High-Value|136|
|Low-Engagement|111|
Medium-Value|253|

*Customer demographics*
```SQL
SELECT
    CASE
        WHEN Age BETWEEN 25 AND 34 THEN '25-34'
        WHEN Age BETWEEN 35 AND 44 THEN '35-44'
        WHEN Age BETWEEN 45 AND 54 THEN '45-54'
        WHEN Age BETWEEN 55 AND 64 THEN '55-64'
        WHEN Age BETWEEN 65 AND 69 THEN '65-69'
        ELSE 'Other'
    END AS AgeGroup,
    COUNT(*) AS CustomerCount
FROM Customers
WHERE Age BETWEEN 25 AND 69   -- filters only desired range
GROUP BY 
    CASE
        WHEN Age BETWEEN 25 AND 34 THEN '25-34'
        WHEN Age BETWEEN 35 AND 44 THEN '35-44'
        WHEN Age BETWEEN 45 AND 54 THEN '45-54'
        WHEN Age BETWEEN 55 AND 64 THEN '55-64'
        WHEN Age BETWEEN 65 AND 69 THEN '65-69'
        ELSE 'Other'
    END
ORDER BY AgeGroup;
```
|Age Group|Customer Count|
|----------|-------|
|25-34|107|
|35-44|92|
|45-54|124|
|55-64|126|
|65-69|51|

*Top regions by number of customers*
```SQL
 Select Region,
count (*) as CustomerCount
from Customers group by Region;
```
|Region|Customer Count|
|----------|-------|
|Midwest|133|
|Northwest|123|
|South|133|
|West Coast|111|

- **Sales / Transaction Analysis**

*Total revenue, quantity, average order value*
```SQL
 SELECT 
    SUM(TotalAmount) AS TotalRevenue,
    SUM(Quantity) AS TotalUnitsSold,
    AVG(TotalAmount) AS AvgOrderValue
FROM Transactions;
```
|Total Revenue|Total Units Sold|Avg Order Value|
|----------|-------|--------|
|779869375.085236|150827|259956.458361745|

*Revenue by product category*
```SQL
 Select p.Category, sum (t.TotalAmount) as Revenue from Transactions t
join Products p on t.ProductID = p.productID group by category;
```
|Category|Revenue|
|----------|-------|
|Consumables|3677784431.551971|
|Equipment|173145140.728699|
|Supplies|238939802.804565|

*Top 10 Customers by Lifetime Spend*

```SQL
SELECT TOP 10
 c.CustomerID, SUM(t.TotalAmount) as LifetimeSpend from Customers c
join Transactions T on c.CustomerID= t.CustomerID 
group by c.CustomerID order by LifetimeSpend DESC;
```
|Customer ID|Lifetime Spend|
|----------|-------|
|C3057|4845268.984375|
|C0047|4800708.828125|
|C0365|4130438.35253906|
|C0395|4096442.703125|
|C0321|3959952.01171875|
|C0347|3947771.453125|
|C0274|3834397.1875|
|C0282|3738739.57305908|
|C0493|3701594.49023438|
|C0203|3633225.65039063|

## Support Analysis

- **Tickets per customer or per region**
