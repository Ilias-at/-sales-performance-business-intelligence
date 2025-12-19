# sales-performance-business-intelligence


My Business & Data Analysis Methodology

##  Vision & Business Intent
Every project starts with understanding **why the business needs change**.

**Project Vision:**  
Transform static sales reports into a dynamic, data-driven decision support system.

**Key questions addressed:**
- What business problem are we solving?
- Who are the users?
- What decisions must be supported?

**Output:** Clear and aligned business vision.

## Business Request
The Sales Department needs to move from static Excel reports to an interactive dashboard.  
The goal is to clearly track internet sales by product, customer, and time, and to compare actual sales against the budget in order to support faster and better decision-making.

## Business Demand Overview
- **Reporter:** Sales Manager  
- **Business Need:** Replace static Excel sales reports with an interactive dashboard  
- **Objective:** Monitor internet sales performance by product, customer, and time  
- **Expected Outcome:** Clear insights, easier analysis, and improved decision-making  
- **Required Systems:** Power BI, CRM System  
- **Other Information:** Sales budgets provided in Excel

## User Stories

| ID | As a | I want | So that |
|----|------|--------|---------|
| US-01 | Sales Manager | to see an overview of internet sales | I can track overall performance |
| US-02 | Sales Manager | to compare sales against budget | I can evaluate performance |
| US-03 | Sales Representative | to see sales by customer | I can focus on high-value customers |
| US-04 | Sales Representative | to see sales by product | I can manage best-selling products |


## Data Cleansing & Transformation (SQL)

Data is extracted from the **AdventureWorks** database using SQL, then saved as CSV files to be imported into Power BI.

**Tables used:**
- DIM_Customers  
- DIM_Products  
- DIM_Calendar  
- FACT_InternetSales  

**Note:** `FACT_Budget` is created from a separate Excel file.
---

## 1. Table DIM_Customers
```sql
SELECT 
  c.[CustomerKey], 
  c.[FirstName] + ' ' + c.[LastName] AS FullName, 
  CASE c.[Gender] WHEN 'M' THEN 'Male' ELSE 'Female' END AS Gender, 
  c.[DateFirstPurchase], 
  g.City AS [Customer City] 
FROM 
  [AdventureWorksDW2019].[dbo].[DimCustomer] c 
  LEFT JOIN [dbo].[DimGeography] g ON c.GeographyKey = g.GeographyKey 
ORDER BY 
  c.CustomerKey
```
<img width="812" height="228" alt="Capture d&#39;écran 2025-12-19 013623" src="https://github.com/user-attachments/assets/d5ea12d3-3b0d-44e8-9d3d-08c05cd605eb" />

## 2. Table DIM_Products
```sql
SELECT 
  p.[ProductKey], 
  p.[ProductAlternateKey] AS ProductItemCode, 
  p.[EnglishProductName] AS [Product Name], 
  ps.[EnglishProductSubcategoryName] AS [Sub Category], 
  pc.[EnglishProductCategoryName] AS [Product Category], 
  p.[Color] AS [Product Color], 
  p.[Size] AS [Product Size], 
  p.[ProductLine] AS [Product Line], 
  p.[ModelName] AS [Product Model Name], 
  p.[EnglishDescription] AS [Product Description], 
  ISNULL(p.[Status], 'Outdated') AS [Product Status] 
FROM 
  [AdventureWorksDW2019].[dbo].[DimProduct] p 
  LEFT JOIN [dbo].[DimProductSubcategory] ps ON p.ProductSubcategoryKey = ps.ProductSubcategoryKey 
  LEFT JOIN [dbo].[DimProductCategory] pc ON ps.ProductCategoryKey = pc.ProductCategoryKey 
ORDER BY 
  p.[ProductKey]
```
<img width="1263" height="222" alt="Table DIM_Products" src="https://github.com/user-attachments/assets/69f14002-8686-4415-bb8b-d2909a76b06b" />

## 3. Table  DIM_Calendar
```sql
SELECT 
  [DateKey], 
  [FullDateAlternateKey] AS Date, 
  [EnglishDayNameOfWeek] AS Day, 
  [EnglishMonthName] AS Month, 
  LEFT([EnglishMonthName], 3) AS MonthShort, 
  [MonthNumberOfYear] AS MonthNo, 
  [CalendarQuarter] AS Quarter, 
  [CalendarYear] AS Year 
FROM 
  [AdventureWorksDW2019].[dbo].[DimDate] 
WHERE 
  CalendarYear >= 2023
```
<img width="667" height="221" alt="Table  DIM_Calendar" src="https://github.com/user-attachments/assets/47ad6b5e-f5f8-4a2d-9591-0e03a3243fac" />

## 4. Table FACT_InternetSales
```sql
SELECT 
  [ProductKey], 
  [OrderDateKey], 
  [DueDateKey], 
  [ShipDateKey], 
  [CustomerKey], 
  [SalesOrderNumber], 
  [SalesAmount] 
FROM 
  [dbo].[FactInternetSales] 
WHERE 
  LEFT(OrderDateKey, 4) >= YEAR(GETDATE()) - 2 
ORDER BY 
  OrderDateKey ASC
```
<img width="816" height="225" alt="Table FACT_InternetSales" src="https://github.com/user-attachments/assets/dc3ce687-f5f4-4272-ac9c-2f476c42396b" />

---

## Entity Relationship Diagram
<img width="1135" height="537" alt="DataModel PNG" src="https://github.com/user-attachments/assets/d9b6a0a4-0ef0-4bbb-814e-7aa131bd015e" />

## Dashboard Overview
This Power BI dashboard gives a clear view of internet sales performance for sales managers and sales representatives.

It helps users quickly:
- See total sales over time
- Know which products sell the most
- Know which customers generate the most revenue
- Compare actual sales with the budget
- Filter data by date, product, and customer

The dashboard is updated once per day.

### Sales Overview
<img width="967" height="539" alt="Dsh SO  PNG" src="https://github.com/user-attachments/assets/657ed5c0-2d1d-4bcc-829b-9e28202ec98e" />

### Product Analysis
<img width="968" height="539" alt="Dsh PD PNG" src="https://github.com/user-attachments/assets/aef3c378-34a6-41b8-925a-f7d62bfe4c7e" />

### Customer Analysis
<img width="965" height="539" alt="Dsh CD PNG" src="https://github.com/user-attachments/assets/19db2890-9860-4a1e-bdcc-7c3ee2122a46" />


---

## Project Summary

This project provides a dynamic Power BI dashboard for Internet Sales, enabling managers and sales representatives to efficiently track top products, top customers, and overall sales trends. The dashboard is updated daily and includes interactive filters by date, product, and customer.

### Key Skills Used
- SQL (Data Cleaning & Transformation)
- Power BI (Dashboard Design & Visualization)
- Data Modeling & Relationships
- DAX for calculations

---

## Feedback 
If you find this project useful, feel free to ⭐️ this repository.  

© 2025 ILIAS 



