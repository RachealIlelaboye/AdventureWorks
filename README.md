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

## Data Source 
The datasets required for this case study were downloaded from 3signet [Official Website](https://www.3signet.com/)
   
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
- Data star schema relationship creation using keys between tables.
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
- Initial data cleaning (handling missing values, duplicates, inconsistent data types)
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
• Data star schema documentation procedures inform of SQL scripts and data cleaning 
procedures for reproducibility and transparency.

• ER diagram 

![nb](https://github.com/user-attachments/assets/75cb3bbf-2567-4283-8ab8-5b03858481ba)

# WEEK 2
- Database Connection with Python
``` python
# Connect to database 
conn = r"C:\Users\user\Desktop\3Signet\Racheal Ilelaboye ADW Task1\Adventure Works.db" 
connection = sqlite3.connect(conn)
```
- Data querying into panda dataframe
```python
# Read sqlite query results into a pandas DataFrame
connection = sqlite3.connect(r"C:\Users\user\Desktop\3Signet\Racheal Ilelaboye ADW Task1\Adventure Works.db" )
df = pd.read_sql_query("SELECT * from AdventureWorks_Customers", connection)
```
- Data Preprocessing and Validation
``` python
# Verify that SQL query is stored in the dataframe
print(df.head())
#To check for null values
df.isna().sum()
#To check table information
df.info()
# To confirm datatype consistency
print(df.dtypes)
# Removing dollar sign from the AnnualIncome column
df['AnnualIncome'] = df['AnnualIncome'].str.replace('$', '', regex=False)
```
- Normalized Dataframe
[Download csv datafrane here](https://github.com/user-attachments/files/17587640/Normalized_dataframe.csv)
[Download pdf script here](https://github.com/user-attachments/files/17587692/ADW.pdf)

- Explorative Data Analysis
``` python
# Customers by Education Level
df.EducationLevel.value_counts()
# visuals
df.EducationLevel.value_counts().plot(kind='bar')
plt.title ("Customers By EducationLevel")
```
# WEEK 3
- Time series Analysis, data analysis by year
``` python
#Product SOld by Year
df.groupby('Year')['ProductName'].sum()

#Product SOld by Month
df.groupby('Months')['ProductName'].count()
```


- Trends and patterns to discover top-performing product and region
``` python
#Selected Dates
list_of_value = ['Jul-2016', 'Jan-2015', 'Feb-2015', 'Mar-2015', 'Apr-2015', 'May-2015', 'Jun-2015', 'Jul-2015', 'Aug-2015'
df2 = df.query('Date == @list_of_value') ['ProductName'].value_counts()
df2
```
Results
ProductName
Road-150 Red, 48 179
Road-150 Red, 62 169
Road-150 Red, 52 168
Road-150 Red, 56 157
Road-150 Red, 44 139
...
Touring-1000 Yellow, 54 2
Touring-3000 Yellow, 50 1
Mountain-500 Black, 52 1
Women's Mountain Shorts, M 1
Mountain-500 Black, 48 1
- Customer demographics and purchasing patterns analysis
- Customer Segmentation
