## Data analysis project: Restaurant orders 

A data analysis project to analyze a quarter's worth of orders from a fictitious restaurant serving international cuisine.

## Table of Contents
- [About the project](#about-the-project)
- [Data sources and tools](#data-sources-and-tools)
- [Data preparation](#data-preparation)
- [Checking and cleaning data](#checking-and-cleaning-data)
- [Analyzing data](#analyzing-data)
- [Results](#results)

## About the project
The project aims to explore data and analyze restaurant orders to identify most popular menu categories, top-selling meals, ordering preferences and peak hours. <br>
The main goal is to provide data-driven insights on further menu development.

## Data sources and tools
Dataset includes the date and time of each order, the items ordered, additional details on the type, name and price of the items. <br>
Source: Maven Analytics <br>
Tools: MySQL

## Data Preparation
Preparation includes: 
- Creating database, 
- Creating tables,
- Inserting data.

[Link to preparation SQL file](Restaurant_orders_preparation.sql)

## Checking and cleaning data
It includes:
- Evaluating data quality,
- Handling missing values,
- Understanding data.

[Link to Cleaning and analyzing file](Cleaning_and_analyzing.md)

## Analyzing data
Analysis includes finding: 
- Most popular categories by orders and revenue,
- Average, lowest and highest prices of menu items,
- Most and least popular meals by orders and revenue,
- Lowest, highest and average order value,
- Highest spend orders,
- Most and least popular hours to order.

[Link to Cleaning and analyzing file](Cleaning_and_analyzing.md#analyzing-data)

## Results
Summary:

Insights are based on available data as mentioned in Data Source section and mostly depend on popularity and revenue. <br>
For a more in-depth analysis, additional data could be included, such as cost of menu items, profit margins, customer feedback, etc.

Insights:
-	Asian cuisine is the most popular category by total order volume. It ranks second in categories by revenue with Italian being the first. 
-	Asian and Italian are not only liked by customers – due to higher pricing they generate more revenue.
-	Although American cuisine is least popular category by orders and revenue, menu items such as Hamburger and Cheeseburger are in the top-selling items by orders and generated revenue.
-	Korean Beef Bowl is one of the top-selling items, but it is the only bowl option in the menu.
-	The least popular menu item is Chicken tacos, followed by Potstickers.
-	Three of the five least popular items are from Mexican cuisine category.
-	Italian cuisine is mostly preferred in high-spend orders.
-	During peak hours, customers prefer Hamburger and Cheeseburger, followed by top meals from Asian cuisine.
  
Opportunities: 
-	As Italian cuisine generates the most revenue and is popular amongst high-spend orders, more Italian meals could be included in the menu.
-	Since Asian cuisine meals are highly demanded, more meals or variations (e.g., bowl options) could be introduced.
-	High-demand and high-revenue items should remain in the menu. They could be highlighted in the menu or featured in promotions. Additionally, lunch deals could be introduced. 
-	The popularity of sides shows demand for small additions to main dishes. Combo options with main meals could be offered.
- Low-revenue and low-popularity items could be considered for removal. Changes in recipes or ingredients could be explored for other items with lower popularity.
-	Reducing low performing Mexican items could create more space to introduce new options from other cuisines.

Other:
-	Understanding the reasons behind missing data (e.g., missing menu items to add or non-existing orders) would help to improve data quality.
-	Collecting additional data for longer time period could help to recognize seasonal trends.

