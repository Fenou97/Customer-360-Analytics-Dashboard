# Customer-360-Analytics-Dashboard

## Table of contents

- [Project Overview](#project-overview)

- [Research Questions](#research-questions)

- [Data Sources](#data-sources)
  
- [Tools](#tools)

-  [Data Manipulation](#data-manipulaation)

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

## Data Manipulation

- **Data Preparation**

The raw data have been imported from Microsoft Excel into Microsoft Power BI for analysis. In PowerBI, Data profiling technique has been implemented to analyze the data to better understand their  structure, quality, and content.

![data profiling Age](https://github.com/user-attachments/assets/21687d21-21d7-4e80-9f40-b03d4c8804d7)

![Data Profiling resolution time](https://github.com/user-attachments/assets/e402d034-e113-493f-95b6-455809309593)

![Data Profiling Transactions](https://github.com/user-attachments/assets/601cf8d0-445c-480d-898c-19aa6793d6ee)

![Data Profiling Unit Price](https://github.com/user-attachments/assets/5a418e14-ef46-467c-83de-1827e928ad72)

- **Exploratory Data Analysis**
  
*Customer-level Analysis*

Customer segmentation counts
   ```SQL
  SELECT Segment, COUNT(*) AS CustomerCount
FROM Customers
GROUP BY Segment;
```
![Customer segmentation counts](https://github.com/user-attachments/assets/c0722fd0-be6d-43c5-8e28-74cbb1a7c99c)

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
