# Lita_Project_Salesdata
This is where the project given at the end of the data analysis training is documented.

### Project Overview
This project is to demonstrate the workflow for analyzing a dataset that contains sales data of a retail store using Microsoft Excel, SQL Server and PowerBi. The goal of the project is to showcase how the data can be transformed, analyzed, and visualized to generate actionable insight and to produce an interactive PowerBi dashboard for reporting and decision making. The sales performance was analyzed to know the top-selling products, regional performance, monthly sales and many more.

### Data Source
The sales data of the retail store was provided by the facilitators for the final project of the training. The dataset contains the orderid, customerid, product, region, quantity, order date and unit price.

### Tools and Techniques
- Microsoft Excel: This was used for the initial data cleaning, calculation of metrics such as average sales and total revenue and creation of pivot tables to summarise the data.
- SQL Server: This was used for querying and manipulating the dataset. It was used to extract insights such as top-selling product, revenue per region and many more.
- Power BI: This wass used to create interactive visualizations and dashboards that provide a summary of the data analysis results.

### Steps Taken
1. Data Cleaning
- Removing Duplicates: The data was cleaned using excel to remove duplicates. There were 50,000 rows before the duplicates were removed resulting in 9,921 rows left.
- Pivot Tables: The data was summarized using pivot tables to create total sales by product, region, and month tables.
  
  i. This image shows the pivot table containing the total sales by product, region, and month.

  ![image](https://github.com/user-attachments/assets/8c298b73-0911-4142-8e74-df27065e912f)

  ii. This image shows the total sales by product. The store has 6 products; shoes, gloves, jackets, shirts, hat, and socks.

  ![image](https://github.com/user-attachments/assets/0db515db-1af6-40fb-b5b6-f5561e565f5c)

  iii. This image shows the total sales by region. The store has branches in the north, south, east and west.

  ![image](https://github.com/user-attachments/assets/54970629-34b3-4f53-96fc-f870390be347)

  iv. A pivot table was also created to show the total sales per month. There are two years with the first year running from January till December and the second year running from January to August.

  ![image](https://github.com/user-attachments/assets/9da98cd3-0e04-4491-9f09-06014e6354ef)

2. Data Analysis
- Microsoft Excel: Excel formulas were to calculate metrics such as average sales per product and total revenue by region.

  -Average sales per product
``` Excel
=AVERAGEIF(C2:C9922, "shirt", H2:H9922)

=AVERAGEIF(C2:C9922, "shoes", H2:H9922)

=AVERAGEIF(C2:C9922, "gloves", H2:H9922)

=AVERAGEIF(C2:C9922, "socks", H2:H9922)

=AVERAGEIF(C2:C9922, "hat", H2:H9922)

=AVERAGEIF(C2:C9922, "jacket", H2:H9922)
```
   -Total revenue by region
``` Excel
=SUMIF(D2:D9922,"North",H2:H9922)

=SUMIF(D2:D9922,"South",H2:H9922)

=SUMIF(D2:D9922,"West",H2:H9922)

=SUMIF(D2:D9922,"East",H2:H9922)
```

- SQL: The data was imported from excel into sql by converting the excel file into csv file. After the data was imported, there were several null values values so that was removed first using the query below.

```SQL
delete from salesdata
where customer_id is null
```
After the data was cleaned, different queries were executed to extract key information from the dataset.

   -Total sales for each product
   ``` SQL
select sum(Quantity) as Salesperproduct, Product from SalesData
group by Product
```
   -Number of sales transaction in each region
``` SQL
select region, count(*) as TransactionperRegion
from SalesData
group by region
```
   -Highest selling product by total sales
``` SQL
select top 1 Product, sum(quantity) as SalesperProduct
from SalesData
group by Product
order by SalesperProduct desc
```
  -Total revenue per product
``` SQL
select sum(totalsales) as TotalRevenueperProduct, product from SalesData
group by product
```
  -Monthly sales for current year
``` SQL
select datepart(month, orderdate) as month, sum(quantity) as MonthlySales
from SalesData
where year(orderdate) = year(GETDATE())
group by datepart(month, orderdate)
order by datepart(month, orderdate)
```
  -Top 5 customers
``` SQL
select top 5 customer_id, sum(totalsales) as Totalpurchaseamount
from SalesData
group by Customer_Id
order by Totalpurchaseamount desc
```
  -Percentage of total sales contributed by each region
``` SQL
select region,
sum(quantity) as RegionalSales,
(sum(quantity) * 100 / sum(sum(quantity)) over()) as Percentagesales
from SalesData
group by Region;
```









