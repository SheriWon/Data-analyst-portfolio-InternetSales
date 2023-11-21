
# ETL-PowerBIvisualisation-InternetSales




## Introduction

Adapting to the evolving business landscape, I am shifting towards an interactive visual dashboard for comprehensive internet sales insights. This dashboard offers holistic sales trends, customizable filtering, real-time performance assessment against 2023's budget, and a two-year historical analysis.

In this project, I extract data from AdventureWorksDW 2022 databases, perform transformations like data cleaning, normalization, aggregation, and calculations, and load them into the destination database or data warehouse. Implement Power BI to build an interactive dashboard.

## Table of Contents

1. Business Request & User Stories

1. Data Cleansing and Transformation	
   
  * Extracting Data
     
  * Handling Null Value and Transformation	

3. Data Modelling

1. Visualisation	

1. Requirements

## Business Request & User Stories

| As a(role) |I want(request/demand) | So that I (user value)| Acceptance criteria |
| :---:        |     :---:      |         :---: | :---:|
| Sales manager | To get a dashboard overview of internet sales    |Can follow better which customers and products sell the best|A Power BI dashboard that updates data once a day|
| Sales representative    | A detailed overview of internet sales per customer      | Can follow up with my customers who buys the most and who we can sell more    |Top customers and who we can sell more	a dashboard allows me to filter data for each customer|
| Sales representative   | A detailed overview of internet sales per product      | Can follow up my products that sell the most   |A Power BI dashboard that allows me to filter data for each product|
| Sales manager    | A dashboard overview of internet sales      | Can follow sales over time against budget   |A Power BI dashboard with graphs and KPIs comparing against budget|




## Data Cleansing and Transformation 

The following tables were extracted using SQL to build the necessary data model for doing analysis and fulfilling the business needs defined in the user stories.
One data source (sales budget) was provided in spreadsheets and connected to the data model later in the process.

**Extracting Data:**

* Dim_Calendar table: transform data type (month, weekly)
```
SELECT 
  [DateKey], 
  [FullDateAlternateKey] AS Date,  
  [EnglishDayNameOfWeek] AS Day,  
  [WeekNumberOfYear] AS WeekNr,
  [EnglishMonthName] AS Month, 
  Left([EnglishMonthName], 3) AS MonthShort,   -- Useful for front-end date navigation and front-end graphs. 
  [MonthNumberOfYear] AS MonthNo, 
  [CalendarQuarter] AS Quarter, 
  [CalendarYear] AS Year --[CalendarSemester] 
FROM 
 [AdventureWorksDW2022].[dbo].[DimDate]
WHERE 
  CalendarYear >= 2021; 
 ```
|DateKey|	Date|	Day	|WeekNr|	Month|	MonthShort|	MonthNo|	Quarter	|Year|
| :---:        |     :---:      |         :---: | :---:|:---:        |     :---:      |         :---: | :---:| :---:|
|20210101	|2021-01-01	|Friday	|1|	January|	Jan	|1|	1|	2021|
|20210102	|2021-01-02|	Saturday|	1|	January	|Jan|	1|	1|	2021|

* Dim_Customer table: Concat name string, join the table from the dim geography table for analysing different customer locations. 
* Dim_Product table: Connect with the dim main category table and subcategory table to get product details. I apply “outdated” status in the product status column to replace the Null value.
* Fact_InternetSales table: Extract data 2 years before the current year, the column field from *[ProductKey],   [OrderDateKey],  [DueDateKey],  [ShipDateKey],  [CustomerKey], [SalesOrderNumber], [SalesAmount]*


**Handling Null Value and Transformation:**

* Replacing 209 null values (firstly, product category and subcategory replace to components regarding product name. Secondly, the subcategory null value with “other”. Finally, replace null value in the product colour, size, model name, and description column field as N/A)
* Transformed customer city data into city categories for analysis and categorization.
## Data Modelling 

Measures for Analysis: Three key measures have been formulated to analyze the relationship between actual sales and sales budget:

* Total Sales: Computed as the summation of sales amounts from the Fact_InternetSales table, providing an aggregate view of sales performance.
* Total Budget: Calculated as the sum of budgetary figures retrieved from the Budget table, allowing a comprehensive overview of allocated budgets.
* Gap of Sales: Derived by computing the variance between the Total Sales and Total Budget measures, highlighting the disparity or surplus between actual sales and the allocated budget.

 
![](datamodelling.png)

This data model configuration is a foundation for in-depth analysis and visualization within Power BI. The established relationships and formulated measures empower users to explore, compare, and interpret actual sales performance against budgetary allocations. Insights drawn from these measures enable informed decision-making, facilitating strategies to bridge gaps or leverage surplus for enhanced business outcomes.


## Visualisation

* A comprehensive Power BI dashboard integrates sales data, featuring a sales overview and detailed views showcasing sales trends per customer and product over time. This implementation included 3 KPIs, over ten slicers, reducing data processing time by 20%.
* Collaboration across sales teams transformed insights into actionable strategies, fostering real-time decision-making.


## Requirements
* Language
    
    - SQL (Select, Where, left Join, Union, Case When, Order By, TOP N)

* Visualisation tool

    - Power BI (Power Query, Star Schema, DAX )
* Analysis concept 

    - Key Performance Indicator, Gap Analysis, SWOT Analysis, Value Chain Analysis, Cost-Benefit Analysis

*Data Source Files

 https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms



