# Restaurant-Order-Analysis
Note: The information and dataset is taken from Maven Analytic's Data Playground. The data is taken from here:https://app.mavenanalytics.io/datasets

**INTRODUCTION**

This project conducts a detailed exploratory data analysis of The Taste of the World Cafe's operations. Using the restaraunt's menu and order details data, we explore various facets such as menu diversity, pricing strategy and ordering patterns.

**TABLE OF CONTENTS**
  1. Data Description
  2. Installation and usage
  3. Situation
  4. Analysis Overview
  5. Questions and Solutions
  6. Results and Discussions
  7. Conclusions

**DATA DESCRIPTION**

The dataset includes two main tables: 'menu_items' and 'order_details'. 'menu_items' contains details about the items on the menu, including their names, categories, and prices. 'order_details' records every order made, with timestamps and the ordered items.

**INSTALLATION AND USAGE**

To run this project, ensure you have PostgreSQL or any other tool capable of running SQL. Clone this repositary and import the SQL script into your PostgreSQL. 

**SITUATION**

The Taste of the World Cafe Introduced a new menu at the start of the year. I have been asked to dig into customer data to see which menu items are doing well/not well and what the top customers seem to like best. 

**ANALYSIS OVERVIEW**

The Analysis covers several key questions

1. Menu composition and category analysis.
2. Pricing analysis for items and categories.
3. Order patterns and date range analysis.
4. Analysis of ordering trends and top spenders.

**QUESTIONS AND SOLUTIONS**

--How many items are on the menu 
```SELECT
	COUNT( DISTINCT(menu_item_id))
FROM
	menu_items;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/c12da027-8277-4b7e-b331-9fcffdf9fb65)

--There are 32 items on the menu

--What are the least and most expensive item on the menu?

```
SELECT
	 price
	,item_name
FROM
	menu_items
ORDER BY 
	price ASC;
SELECT
	 price
	,item_name
FROM
	menu_items
ORDER BY 
	price DESC;
 ```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/8299852b-4845-486c-9d77-312d6c3f9e5c)

--Most expensive $19.95- Shrimp Scampi, least expensive-$5.00 Edamame

--How many italian dishes are on the menu? What are the least and 
most expensive italian dishes on the menu?

```
SELECT
	 COUNT(category)
FROM
	menu_items
WHERE
	category = 'Italian';
	
SELECT
	 price
	,item_name
	,category
FROM
	menu_items
WHERE
	category = 'Italian'
ORDER BY 
	price ASC;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/c3bbad58-74e7-4fe0-b329-c18e0fe47e8f)

```
SELECT
	 price
	,item_name
	,category
FROM
	menu_items
WHERE
	category = 'Italian'
ORDER BY 
	price DESC;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/95b1ba85-ce9e-46df-aa08-bb499eb7800d)

--There are 9 italian dishes on the menu, the least expensive italian item is fettuccine alfredo at $14.5,the most expensive italian item is Shrimp Scampi at $19.95

--How many dishes are in each category? What is the average dish price within each category?
```
SELECT
	 category
	,COUNT(menu_item_id) as no_of_dishes
FROM
	menu_items
GROUP BY
	category;
	
SELECT
	 category
	,ROUND(AVG(price),2) as Avg_price
FROM
	menu_items
GROUP BY
	category;
 ```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/adf26f9b-5890-4645-b99b-def67f0462ef)

--What is the date range of the order table?
```
SELECT
	 MIN(order_date)
	,MAX(order_date)
FROM
	order_details;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/fd000db7-3ea7-4ea5-ab23-eb7f4c34ab8e)

--The date range is 2023-01-01 to 2023-03-31

--How many orders were made within this date range?
```
SELECT
	 COUNT(DISTINCT (order_id))
FROM
	order_details;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/605029fc-a11b-4b50-9d48-72acab7b4592)

--There were 5370 orders made during this date range

--How many items were ordered within this date range?
```
SELECT
	 COUNT(DISTINCT(order_details_id))
FROM
	order_details;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/07f69606-7543-47d0-8aaf-1f6b0f34cf7d)

--There were 12234 items ordered during this date range

--what order had the most number of items?
```
SELECT
	 order_id
	,COUNT(item_id) as no_of_items
FROM
	order_details
GROUP BY
	order_id
ORDER BY
	no_of_items DESC;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/2762f946-b41b-4aa8-81ed-00b8d19b96de)

--there are seven orders that had the most number of items, the most no. of items ordered were 14

--How many orders had more than 12 items?
```
SELECT
	COUNT(order_id)
FROM
		(SELECT
			 order_id
			,COUNT(item_id) as no_of_items
		FROM
			order_details
		GROUP BY
			order_id
		HAVING COUNT(item_id) > 12) as no_of_orders;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/a8146d6a-2d60-46ff-9758-bc272b086cd6)

--There are 20 orders with items more than 12 

--What are the least and most ordered items? What categories were they in?
```
SELECT
	 m.item_name
	,m.category
	,COUNT(o.order_details_id) as no_of_purchases
FROM
	order_details o
LEFT JOIN 
	menu_items m
		ON o.item_id = m.menu_item_id
GROUP BY
	 m.item_name
	,m.category
ORDER BY
	COUNT(o.order_details_id) DESC;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/9db89837-648b-4e9e-8ec7-2da52813b07d)
```
SELECT
	 m.item_name
	,m.category
	,COUNT(o.order_details_id) as no_of_purchases
FROM
	order_details o
LEFT JOIN 
	menu_items m
		ON o.item_id = m.menu_item_id
GROUP BY
	 m.item_name
	,m.category
ORDER BY
	COUNT(o.order_details_id) ASC;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/ffe5155e-a5d4-49d3-a167-66f722e57285)

--The most ordered item was hamburger with 622 purchases in American category and the least ordered item was chicken tacos with 123 purchases in Mexican category

--What were the top 5 orders that spent the most money? 

```
SELECT
	 o.order_id
	,SUM(m.price) as total_Spent
FROM
	order_details o
LEFT JOIN
	menu_items m
		ON o.item_id = m.menu_item_id
WHERE
	m.price IS NOT NULL -- This line excludes rows where m.price is null
GROUP BY
	o.order_id 
ORDER BY
	total_Spent DESC
LIMIT 5;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/98bef073-158f-4dc8-b36a-f82bd21d36fc)


--View the details of the highest spent order
```
SELECT
	 m.category
	,COUNT(o.item_id) as no_of_items
FROM
	order_details o
LEFT JOIN
	menu_items m
		ON o.item_id = m.menu_item_id
WHERE
	order_id = 440
GROUP BY
	m.category;
```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/a2e50d15-7b7c-4dea-9297-66d5f859101b)


--The highest spend order bought mostly italian even though italian items arent as popular on the menu

--viewing details of the top 5 spend orders 
```
SELECT
	 m.category
	,COUNT(o.item_id) as no_of_items
FROM
	order_details o
LEFT JOIN
	menu_items m
		ON o.item_id = m.menu_item_id
WHERE
	order_id = 2075
GROUP BY
	m.category;
 ```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/782ba594-4146-43b1-8885-db507ccfa642)

--The second top order also likes italain 
```
SELECT
	 m.category
	,COUNT(o.item_id) as no_of_items
FROM
	order_details o
LEFT JOIN
	menu_items m
		ON o.item_id = m.menu_item_id
WHERE
	order_id = 1957
GROUP BY
	m.category;
 ```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/f6c9259a-bf66-4f49-bc60-ac65263427df)

--the third top order also likes italian
```
SELECT
	 m.category
	,COUNT(o.item_id) as no_of_items
FROM
	order_details o
LEFT JOIN
	menu_items m
		ON o.item_id = m.menu_item_id
WHERE
	order_id = 330
GROUP BY
	m.category;
 ```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/599503f4-1176-425b-8182-c0a5bb1cc258)

--the fourth top order likes asian the most
```
SELECT
	 m.category
	,COUNT(o.item_id) as no_of_items
FROM
	order_details o
LEFT JOIN
	menu_items m
		ON o.item_id = m.menu_item_id
WHERE
	order_id = 2675
GROUP BY
	m.category;
 ```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/a72316e2-cb9c-4ffd-b75d-52bb314c4fee)

--The fifth top spender has an equal liking to mexican and italian

--ALTERNATE CODE
```
SELECT
	 o.order_id
	,m.category
	,COUNT(o.item_id) as no_of_items
FROM
	order_details o
LEFT JOIN
	menu_items m
		ON o.item_id = m.menu_item_id
WHERE
	order_id IN (440, 2075, 1957, 330, 2675)
GROUP BY
	 o.order_id
	,m.category;
 ```
![image](https://github.com/AbhirajDadiyan/SQL-projects/assets/136371147/1e8a358a-40d9-4514-9c45-d063908206d3)

--The top 5 highest spent orders are ordering italian food than any other food. (individual results same as mentioned before)
--we need to keep the expensive italian dishes on our menu becuase people seem to like it more than other dishes


**RESULTS AND DISCUSSION**

The analysis yielded insights such as:

1. The diversity of the menu across different food categories.
2. The pricing range of items and average prices per category.
3. The order frequency and the popularity of items.
4. The spending patterns of the top orders, with a particular focus on Italian dishes.

**CONCLUSION**

The analysis reveals that customers have a strong preference for italian dishes, despite their higher price point. This has implications for menu planning and inventory management. 
