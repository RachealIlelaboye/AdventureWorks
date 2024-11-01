# ADVENTURE WORKS
This is a documentation of the AdventureWorks analysis.

## Table of Contents
- [Project Overview](#project-overview)
- [Project Aim](#project-aim)
- [Project Objectives](#[project-objective)
- [Data Source](#data-source)
- [Tools Used](#tools-used)
- [WEEK 1](#week-1)
- [WEEK 2](#week-2)
- [WEEK 3](#week-3)
- [WEEK 4](#week-4)
- [WEEK 5](#week-5)
- [WEEK 6](#week-6)
- [PROJECT CONCLUSION](#project-conclusion)
- [RECOMMENDATIONS](#recommendations)
  
# Project Overview
AdventureWorks is a product/sales data highlighting the various products, subcategories, order etc.

## Project Aim
This project seeks to conduct a comprehensive analysis of sales data, customer demographics, and product details to uncover complex relationships and provide sophisticated insights. These insights will facilitate the development of more effective sales strategies, refined product offerings, and targeted marketing campaigns, ultimately driving sustained growth and customer satisfaction.

## Project Objectives
The objectives of this project is to;
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
![Sales by mth](https://github.com/user-attachments/assets/b89e3416-e1cd-4571-b359-12abe4fdbd26)
![Pdts by year](https://github.com/user-attachments/assets/0743e2dc-672e-4dfe-b8a9-a9e47659942b)

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
``` python
#Gender by Sales
df.groupby('Gender')['ProductName'].count().sort_values(ascending=False).plot(kind='bar')
plt.title("Customers' Gender by Sales")
plt.show(
```
![Sales by gender](https://github.com/user-attachments/assets/06652e77-8b43-41b7-9938-78c824f04196)
```python
#EducationLevel by Sale
df.groupby('EducationLevel')['ProductName'].count()
```
![Sales by edu level](https://github.com/user-attachments/assets/3e7b32e2-6c28-4501-b49d-ef7f0f2ec811)


```python
#Marital Statusby Sales
df.groupby('MaritalStatus')['ProductName'].count().sort_values(ascending=False).plot(kind='bar')
plt.title("Customers' MaritalStatus by Sales")
plt.show()
```
 ![Sales by marital](https://github.com/user-attachments/assets/87e5fe0c-1a08-4928-800c-5fa68f629de5)

```python
#Occupation by Sale
df.groupby('Occupation')['ProductName'].count()
```
![Sales by occupation](https://github.com/user-attachments/assets/c8810852-3d3f-4cf8-9831-c7ce4fa44ca5)
```python
#Customers Homeownership status by Sale
df.groupby('HomeOwner')['ProductName'].count()
```
![Sales by homeownership](https://github.com/user-attachments/assets/6b573aa1-22c9-4c65-a15c-1ed857386de2)

- Customer Segmentation
```SQL
------SEGMENTATION BASED ON GENDER------
SELECT Gender,count(ProductName) as Product_Sales
FROM Sales
GROUP BY Gender

------SEGMENTATION BASED ON EDUCATION LEVEL------
SELECT EducationLevel,count(ProductName) as Product_Sales
FROM Sales
GROUP BY 1
ORDER BY 2 DESC

------SEGMENTATION BASED ON MARITAL STATUS------
SELECT MaritalStatus,count(ProductName) as Product_Sales
FROM Sales
GROUP BY 1
ORDER BY 2 DESC

----SEGMENTATION BASED ON OCCUPATION------
SELECT Occupation,count(ProductName) as Product_Sales
FROM Sales
GROUP BY 1
ORDER BY 2 DESC

----SEGMENTATION BASED ON HOME OWNWERSHIP------
SELECT HomeOwner,count(ProductName) as Product_Sales
FROM Sales
GROUP BY 1
ORDER BY 2 DESC

----SEGMENTATION BASED ON NUMBER OF CHILDREN------
SELECT TotalChildren,count(ProductName) as Product_Sales
FROM Sales
group by 1
ORDER BY 2 DESC
```

# WEEK 4
- Product performance and Trends Analysis
``` python
#TOP 10 PRODUCTS BY REVENUE
g= df.groupby('ProductName', as_index=False)['Revenue'].sum().sort_values(by= 'Revenue', ascending=False).head(10)
sns.barplot(data=g, x='ProductName', y='Revenue', hue='ProductName', dodge=False).set(xticklabels=[]);
```
![pdts by revenue](https://github.com/user-attachments/assets/c745edcc-21c1-40be-a62d-96b40cb3f607)
``` python
#TOP 5 Products by Number Sold
plt.figure(figsize=(10,6))
plt.plot(df.groupby('ProductName')['ProductName'].count().sort_values(ascending=False).head(5))
plt.title("TOP 5 Products by Number Sold")
plt.show()
```
![Least 5 pdts quantity](https://github.com/user-attachments/assets/ec737e38-021d-4cb7-946b-44e67b5a965e)
``` python
#Product sold by Color
plt.figure(figsize=(10,6))
plt.plot(df.groupby('ProductColor')['ProductColor'].count().sort_values(ascending=False))
plt.title("Product Color Number Sold")
plt.show()
```
![Color by sales](https://github.com/user-attachments/assets/8d1483d1-e206-43c0-9673-e19697a83d8e)
- Sales performance analysis across territories
``` python
#REVENUE BY CONTINENT
R= df.groupby('Continent', as_index=False)['Revenue'].sum().sort_values(by= 'Revenue', ascending=False)
sns.barplot(data=R, x='Continent', y='Revenue', hue='Continent', dodge=False).set(xticklabels=[]);
```
![Revenue by  continent](https://github.com/user-attachments/assets/31d6a9a7-d179-4163-901e-bcc452baefe0)
``` python
#PRODUCT SALES BY CONTINENT
df.groupby('Continent')['ProductName'].count().sort_values(ascending=False).plot(kind='bar')
plt.title("Sales by Continent")
plt.show()
```
![sale by  continents](https://github.com/user-attachments/assets/6b3f92a7-0d87-4c94-8acc-1c4d287ac527)

``` python
#SALES BY COUNTRY
plt.figure(figsize=(10,5))
plt.plot(df.groupby('Country')['ModelName'].count().sort_values(ascending=False).head(10))
plt.title("Sales by Country")
plt.show()
```
![sale by  country](https://github.com/user-attachments/assets/9a0e9ef9-cd69-41ea-9046-b42631345c56)

# WEEK 5
- Hypothesis testing

  Hypothesis 1: Higher product prices will lower sales quantity
```python
 # Scatter plot of price vs sales_quantity
plt.figure(figsize=(14, 6))
sns.scatterplot(x='ProductPrice', y='OrderQuantity', data=data)
plt.title('Price vs Sales Quantity')
plt.xlabel('Price')
plt.ylabel('Sales Quantity')
plt.show()
```
![Scatterplot sales and qty](https://github.com/user-attachments/assets/1ef2c907-ffcc-4007-bd90-2d7b382c24e4)
```python
# Calculate Pearson's correlation coefficient for Hypothesis 1
correlation, p_value = stats.pearsonr(data['ProductPrice'], data['OrderQuantity'])
print(f"Pearson's correlation: {correlation}")
print(f"P-value: {p_value}")
```
  Hypothesis 2: Sales performancce varies significantly across territories
  ```python
# Scatter plot of price vs sales_quantity
plt.figure(figsize=(8, 6))
sns.scatterplot(x='Country', y='OrderQuantity', data=data)
plt.title('Price vs Sales Quantity')
plt.xlabel('Price')
plt.ylabel('Sales Quantity')
plt.show()
```
![Sales x territory scatter](https://github.com/user-attachments/assets/8bfec454-c3bd-4c83-97ec-29ca0e27ab3f)

``` python
# Calculate Pearson's correlation coefficient for Hypothesis 2
correlation, p_value = stats.pearsonr(data['OrderQuantity'], data['SalesTerritoryKey'])
print(f"Pearson's correlation: {correlation}")
print(f"P-value: {p_value}")
```
  Hypothesis 3; Sales are higher for certain product categories due to seasonal trends
```python
# Boxplot to visualize the distribution of sales across seasons and product categories
plt.figure(figsize=(16, 6))
sns.boxplot(x='Date', y='OrderQuantity', hue='ProductCategory', data=data)
plt.title('Sales Distribution Across Seasons and Product Categories')
plt.show()
```
![sales x season](https://github.com/user-attachments/assets/79476433-28b3-401b-8986-a4ef60cca2f5)
```python
# Hypothesis 3
# Perform ANOVA using statsmodels
model = ols('OrderQuantity ~ C(ProductCategory) + C(Season) + C(ProductCategory):C(Season)', data=data).fit()
anova_table = sm.stats.anova_lm(model, typ=2)

# Print the ANOVA table
print(anova_table)
```
```python
# Perform Tukey's HSD test
tukey = pairwise_tukeyhsd(endog=data['OrderQuantity'], groups=data['ProductCategory'] + "_" + data['Season'], alpha=0.05)
# Print Tukey's test results
print(tukey)
# Plot the Tukey's HSD results
tukey.plot_simultaneous()
plt.title('Tukey HSD - Sales across Product Categories and Seasons')
plt.show()
```
![Tukey HSD](https://github.com/user-attachments/assets/e878da2a-285c-4fa9-81af-adcf23f62456)
[Read Report Here]((https://github.com/user-attachments/files/17598318/HYPO.pdf)

- Advanced Statistical Analysis
```python
# Specific correlation
correlation = stats.pearsonr(data['ProductPrice'], data['Revenue'])
print(f'Correlation between ProductPrice and Revenue: {correlation[0]}, p-value: {correlation[1]}')
# Simple Linear Regression
model = ols('Revenue ~ ProductPrice + OrderQuantity', data=data).fit()
print(model.summary())
# Multiple Regression
multiple_model = ols('Revenue ~ ProductPrice + OrderQuantity + C(ProductCategory)', data=data).fit()
print(multiple_model.summary())
# One-way ANOVA
anova_result = sm.stats.anova_lm(model, typ=2)
print(anova_result)
```
[Read Report Here]((https://github.com/user-attachments/files/17598321/Statistical.Analysis.Racheal.Ilelaboye.pdf))

# WEEK 6
- Interactive Dashboards
![B1](https://github.com/user-attachments/assets/434f6dae-a25f-40b5-8f9a-febc86db5908)
![B2](https://github.com/user-attachments/assets/4fe9bfd2-7dfc-4549-add1-27749ebbbd33)
![B3](https://github.com/user-attachments/assets/0ff5f4c4-4faa-4ef4-a416-c3a11565021f)
![B4](https://github.com/user-attachments/assets/df107ed8-5768-4a9f-9d19-537824a1fc66)
![B5](https://github.com/user-attachments/assets/749719b0-70f5-46eb-834f-ac562d11423a)

- Presentation
[View slide here](https://github.com/user-attachments/files/17590770/ADW.Racheal.Ilelaboye.pptx)

- Live Demonstration
  [View here](https://github.com/user-attachments/assets/ac643031-2c0e-4d32-a6a8-4d324e19160c)



# PROJECT CONCLUSION
This project provided valuable insights into the sales dataset, uncovering patterns and trends to support data-driven decision-making. The analyses covered key areas such as customer segmentation, product performance, and regional sales trends. The findings included identifying top-performing products, understanding customer behavior, and evaluating regional contributions to revenue. These insights enable targeted strategies, such as optimizing inventory, enhancing marketing efforts, and prioritizing customer segments to increase overall sales efficiency and revenue.
Using SQL for database management, Power BI for dashboard creation, and Python for statistical analysis, we established a comprehensive approach for data analysis and visualization. This project serves as a foundational model for future data analytics initiatives and can be scaled to incorporate new data or additional analytical layers, thus providing a dynamic tool for ongoing performance monitoring.

# RECOMMENDATIONS
1. Price Strategy: Given the strong positive relationship between ProductPrice and Revenue, increasing the product price (without reducing demand too much) may lead to 
higher revenues.
2. Investigate Order Quantity: The negative relationship between OrderQuantity and Revenue suggests that higher sales volume may not be increasing revenue, likely due to discounts or lower-margin products being sold in higher quantities. Further analysis should be conducted on pricing and discount policies.
3. Product Price Optimization: Since ProductPrice has a very strong impact on  Revenue, consider carefully optimizing pricing strategies. Small changes in price could 
have a large effect on revenue.
4. Sales Strategy for Quantity: Even though OrderQuantity is statistically significant, its effect is comparatively smaller. It might be worth exploring how increasing order 
quantity could be optimized to enhance revenue without discounting too heavily.
5. Product Strategy: Since "Bikes" and "Clothing" are associated with lower revenues, consider evaluating product strategies for these categories. Explore ways to improve their 
profitability through better pricing, bundling, or cost management.
6. Order Quantity: The negative impact of order quantity suggests that large orders might be leading to revenue loss due to discounts. Consider revising the discount structure to 
balance between encouraging larger orders and maintaining profitabilit

