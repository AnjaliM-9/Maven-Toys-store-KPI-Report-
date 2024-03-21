# Maven Toy store KPI Report
## Problem Statement:
Sales & inventory dataset for a fictitious chain of toy stores in Mexico called Maven Toys, including information about products, stores, daily transactions, and current inventory levels at each location.
Some questions that prepared report can answer:
1. Which product categories drive the biggest profits? Is this the same across store locations?
2. Can you find any seasonal trends or patterns in the sales data?
3. Are sales being lost with out-of-stock products at certain locations?
4. How much money is tied up in inventory at the toy stores? How long will it last?
   
## Datasource and Preparation
Data set for this task was provided by Maven Analytics that contained 4 CSV Files name as:-
- calendar
- inventory
- products
- sales

Steps performed for Data transformation in Power Query after the dataset loaded into Microsoft Power BI Desktop for modelling:
- Each of the columns in the table were validated to have the correct data type for all 5 query tables
- Created date hierarchy in model view in date table.
- Filtered out missing values row from data set.
- Changed the currency type by changing the currency locale.
- created date field “bins” approx. according to the seasons.

  ![Picture - bins](https://github.com/AnjaliM-9/Maven-Toys-store-KPI-Report-/assets/155083462/74a39f52-37ef-41a0-b562-42ca0bcc6c39)

## Data Modeling

![Picture 1](https://github.com/AnjaliM-9/Maven-Toys-store-KPI-Report-/assets/155083462/573705a5-b693-46c5-b728-383ee15e0249)

## Data Analysis (DAX)
1.	 Created following calculated columns:-

In Inventory table: 
- ``` Prod cost = RELATED(products[Product_Cost])```
- ``` Total = inventory[Stock_On_Hand]*RELATED(products[Product_Cost])```
  
In Sales table:

- ``` Cost = RELATED(products[Product_Cost])```
- ``` Price = RELATED(products[Product_Price])```
- ``` Profit = sales[Revenue] - (sales[Cost]*sales[Units])```
- ``` Revenue = sales[Units]*sales[Price]```
  
2.	Created following measures:-
	
In Inventory table:
-	```running total in Stock_On_Hand = CALCULATE( SUM('inventory'[Stock_On_Hand]),FILTER(ALLSELECTED('inventory'[Stock_On_Hand]), ISONORAFTER('inventory'[Stock_On_Hand], MAX('inventory'[Stock_On_Hand]), DESC)
    )```
- ```Total value of current stock = SUMX(inventory,inventory[Stock_On_Hand]*RELATED(products[Product_Cost]))```
- ``` Stock ratio = DIVIDE([Total revenue 90 days],[Total value of current stock],0)```
- ``` Total value of current stock = SUMX(inventory,inventory[Stock_On_Hand]*RELATED(products[Product_Cost]))```
  
In Sales table:
-	```Revenue(M) = SUMX(sales,sales[Units]*RELATED(products[Product_Price]))```
-	```Total Orders = DISTINCTCOUNT(sales[Sale_ID])```
-	``` Total profit = SUM(sales[Profit])```
-	``` Total Revenue = SUM(sales[Revenue])```
-	``` Total revenue 90 days = CALCULATE([Total Revenue],DATESBETWEEN('calendar'[Date],MAX('calendar'[Date])-90,MAX('calendar'[Date])))```
  
## Dashboard (Data Visualization)
### Toy store KPI’S
![Toy_store_KPI_Report_page-0001](https://github.com/AnjaliM-9/Maven-Toys-store-KPI-Report-/assets/155083462/95b3389c-ca4c-41bf-9656-a3c6c6ceb004)

### Trend Analysis
![Toy_store_KPI_Report_page-0002](https://github.com/AnjaliM-9/Maven-Toys-store-KPI-Report-/assets/155083462/6ac7ca85-46cb-4b77-b3a1-049660d781a5)

### Stock Valuation
![Toy_store_KPI_Report_page-0003](https://github.com/AnjaliM-9/Maven-Toys-store-KPI-Report-/assets/155083462/a8455127-5ed4-4371-8378-630248fd491a)

## Insights!
Following observations can be inferred by the created dashboard:-
1. Toys only category highest selling product category among all the store locations.
2. The Month of March has a peek in revenue collection of the company.
3. Downtown location stands to be more profitable location as compared to other locations.
4. Winter season seems have a significant increase in total no of orders.
5. Product items likes Uno and Jenga have less stocks in various stores.


