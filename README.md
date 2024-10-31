# ADVENTURE WORKS
This is a documentation of the AdventureWorks analysis.

# Project Overview
AdventureWorks is a product/sales data highlighting the various products, subcategories, order etc.

## Project Aim
This project seeks to conduct a comprehensive analysis of sales data, customer demographics, and product details to uncover complex relationships and provide sophisticated insights. These insights will facilitate the development of more effective sales strategies, refined product offerings, and targeted marketing campaigns, ultimately driving sustained growth and customer satisfaction.

## Project Objectives
1. Identify Sales Trends: Analyze sales data to uncover trends over time, including peak periods and sales dips.
2. Understand Customer Behavior: Examine customer demographics and purchasing 
patterns to identify key customer segments and behaviors.
3. Evaluate Product Performance: Assess the performance of different products, 
including best-sellers and underperforming items.
4. Analyze Territorial Differences: Compare sales performance across different regions to identify high and low-performing areas.
5. Seasonal Impact: Determine the influence of seasonal trends on sales for various product categories.
6. Optimize Sales Strategies: Provide recommendations for improving sales strategies based on data insights.
7. Enhance Product Offerings: Suggest optimizations for product offerings based on performance and customer preferences.
   
## Tools Used 
### DB SQLITE [Download here](https://sqlitebrowser.org/dl/)
  - Database creation and relationship
### PowerBI [Download here](https://www.microsoft.com/en-us/download/details.aspx?id=58494)
  - Visualization
### Python[Download here](https://www.python.org/downloads/)
  1. Data Cleaning and Dataframe normalization
  2. Hypothesis testing
  3. Advanced statistical Analysis
  4. Product Performance Analysis
  5. Sales Analysis
  6. Time Series Analysis
### Microsoft Powerpoint [Dpownload here](https://www.microsoft.com/en-us/microsoft-365/powerpoint)
-  Presentation Slide
### Zoom [Download here](https://zoom.us/download)
  - Live Demonstration Recording
### Github [Download here](https://desktop.github.com/download/)
- Portfolio building
  
# WEEK 1
- Set up SQL database for the Adventure Works data, ensuring data integrity and 
security.
• Create a data star schema by creating relationship keys between tables.
```SQL
-------------Creating Star Schema----
CREATE TABLE ADW_Sales(
	OrderDate	date,
	StockDate	date,
	OrderNumber	TEXT PRIMARY KEY,
	ProductKey	INTEGER,
	CustomerKey	INTEGER,
	TerritoryKey	INTEGER,
	OrderLineItem	INTEGER,
	OrderQuantity	INTEGER,
	FOREIGN KEY (CustomerKey) REFERENCES ADW_Customer(CustomerKey),
	FOREIGN KEY (TerritoryKey) REFERENCES ADW_Returns (TerritoryKey),
	FOREIGN KEY (ProductKey) REFERENCES ADW_Products(ProductKey),
	FOREIGN KEY  (TerritoryKey) REFERENCES ADW_Territories(TerritoryKey),
	FOREIGN KEY  (OrderDate) REFERENCES ADW_Calendar(OrderDate));
```
• Perform initial data cleaning (handling missing values, duplicates, inconsistent data types)
```SQL
------REPLACING NULL VALUE
UPDATE AdventureWorks_Customers
SET Prefix = "NA" WHERE Prefix IS NULL
```
```SQL
---------Identifying Duplicates
SELECT ProductKey, COUNT(ProductKey) AS count 
FROM AdventureWorks_Products GROUP BY ProductKey HAVING COUNT(ProductKey) > 1;
```
• Document data star schema procedures inform of SQL scripts and data cleaning 
procedures for reproducibility and transparency.
• ER diagram 
![nb](https://github.com/user-attachments/assets/75cb3bbf-2567-4283-8ab8-5b03858481ba)
