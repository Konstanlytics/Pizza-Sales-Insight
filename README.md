# 🍕 Pizza Sales Dashboard


## 📍 Pizza Sales Dashboard

This dashboard gives a simple and visual overview of how a fictional pizza company is doing in terms of sales.

It was built using **SQL** (to get the data ready) and **Power BI** (to create the charts and visuals).

The goal? To help anyone — whether you're a **business owner**, **data analyst**, or just curious — quickly find answers like:

- 🍕 **Which pizzas sell the most?**
- 📏 **Which pizza size makes the most money?**
- 📈 **How do sales change over time or across different pizza categories?**

This dashboard turns numbers into insights, helping you make smarter decisions at a glance.



## 🌟 Key Features

- 🧾 **KPI Cards**  
  Instantly see total revenue, total orders, total pizzas sold, and average pizzas per order.

- 🎛️ **Interactive Slicers**  
  Easily filter the dashboard by date, pizza size, or category.

- 📊 **Visual Variety**  
  Pie charts, bar graphs, and clustered column charts make it easy to view data across different sales metrics and categories.

- 🔍 **Drillable Charts**  
  Explore the best and worst sellers dynamically, whether you're looking at revenue, quantity sold, or number of orders.



## 🧰 Tools

Tools Used:

SQL – Used for querying and transforming the raw sales data

Power BI – Used for data modeling, DAX measures, and dashboard creation



## 🧮 SQL Data Preparation & Metric Calculation


Before building the visual dashboard in Power BI, we used SQL to perform data aggregation and business metric derivation from the raw transactional data. This script was responsible for generating key insights and summarized KPIs that were later visualized in the dashboard.

-- Calculate for Revenue by Summing the Total_Sales
Select SUM(total_price) 
		AS Revenue
		FROM pizza_sales;

-- Calculate for the Average
Select SUM(total_price) / COUNT(distinct order_id) 
		AS Avg_Order_Value
		From pizza_sales; 
	
-- Calculate For Total Pizza Sold
Select SUM(quantity)
		AS Total_Pizza_Sold
		From pizza_sales;

-- Total Orders Placed
Select COUNT(Distinct order_id)
		AS Total_Orders
		From pizza_sales;

-- Average Pizzas Per Order
Select CAST(CAST(SUM(quantity) AS decimal(10,2)) / CAST(COUNT(Distinct order_id) AS decimal(10,2)) AS decimal(10,2))
		AS Avg_Pizza_Per_Order
		FROM pizza_sales;

-- Daily Trend of Total Orders Arranged by Day of the Week (Sunday to Saturday)
Set Datefirst 7;
Select 
	DATENAME(WEEKDAY, order_date) AS order_day,
	COUNT(distinct order_id)  AS Total_Orders
From pizza_sales
Group by	
	DATENAME(WEEKDAY, order_date),
	DATEPART(weekday,order_date)
Order by	
	DATEPART(weekday,order_date)


-- Daily Trend of Total Orders Sorted from Highest to Lowest
Select		
	DATENAME(WEEKDAY, order_date) AS order_day,
	COUNT(distinct order_id) AS Total_Orders
From pizza_sales
Group by	
	DATENAME(WEEKDAY, order_date),
	DATEPART(weekday,order_date)
order by	
	Total_Orders Desc;

-- Monthly Trend of Total Orders Sorted in Desc Orders
Select 
	DATENAME(MONTH, order_date) AS Month_Name,
	COUNT(DISTINCT Order_Id) As Total_Orders
	From pizza_sales
GROUP BY 
	DATENAME(MONTH, Order_date)
Order by 
	Total_Orders Desc;

-- Percentage of Sales by Pizza Category
SELECT 
    pizza_category, 
    ROUND(SUM(total_price), 2) AS total_sales, 
    ROUND(
        SUM(total_price) * 100.0 / 
        (SELECT SUM(total_price) FROM pizza_sales), 
    2) AS pct
FROM pizza_sales
GROUP BY pizza_category;

-- Percentage of Sales by Pizza Category for the Month of January
SELECT 
    pizza_category, 
    ROUND(SUM(total_price), 2) AS Total_Sales, 
    ROUND(
        SUM(total_price) * 100.0 / 
        (SELECT SUM(total_price) FROM pizza_sales WHERE MONTH(order_date) = 1), 2) AS PCT
FROM pizza_sales
WHERE MONTH(order_date) = 1
GROUP BY pizza_category;

-- Percentage of Sales by Pizza Size
SELECT 
    pizza_size, 
    ROUND(SUM(total_price), 2) AS total_sales, 
    ROUND(SUM(total_price) * 100.0 / 
        (SELECT SUM(total_price) FROM pizza_sales), 2) AS pct
FROM pizza_sales
GROUP BY pizza_size;

-- Percentage of Sales by Pizza Size by Quarter 1
SELECT 
    pizza_size, 
    ROUND(SUM(total_price), 2) AS total_sales, 
    ROUND(SUM(total_price) * 100.0 / 
        (SELECT SUM(total_price) FROM pizza_sales Where DATEPART(quarter, order_date) = 1), 2) AS pct
FROM pizza_sales
Where DATEPART(quarter, order_date) = 1 
GROUP BY pizza_size;

-- Top 5 Best Sellers by Revenue
Select top 5 pizza_name,
		SUM(total_price) As Total_Revenue
From	pizza_sales
Group by pizza_name
order by Total_Revenue Desc

-- Bottom 5 Sellers by Revenue
select top 5 pizza_name, 
		SUM(total_price) AS Total_Revenue 
From pizza_sales
Group by pizza_name
order by Total_Revenue 

-- Top 5 Sellers by Total Quantity
select top 5 pizza_name, 
		SUM(quantity) AS Total_Quantity 
From pizza_sales
Group by pizza_name
order by Total_Quantity Desc

-- Bottom 5 Sellers by Total Quantity
select top 5 pizza_name, 
		SUM(quantity) AS Total_Quantity 
From pizza_sales
Group by pizza_name
order by Total_Quantity Asc

-- Top 5 Sellers by Total Orders
select top 5 pizza_name, 
		Count(Distinct order_id) AS Total_orders 
From pizza_sales
Group by pizza_name
order by Total_orders Desc

-- Bottom 5 Sellers by Total Orders
select top 5 pizza_name, 
		Count(Distinct order_id) AS Total_orders 
From pizza_sales
Group by pizza_name
order by Total_orders

## Data Analysis

### Overview

![pizza-overview](https://github.com/Konstanlytics/Pizza-Sales-Insight/blob/main/Pizza-Overview.JPG)

## Key Numbers
- **Total Revenue**: $818,000
- **Average Order Value**: $38.31
- **Total Pizzas Sold**: 49,574
- **Total Orders**: 21,350
- **Average Pizzas Per Order**: 2.32

---

## Monthly Trends

### Total Orders by Month
Looking at the number of orders throughout the year, we see that **January** and **October** have the most orders. The number of orders tends to go down toward the end of the year, which might be because of less demand after the busy summer months.

This information helps us figure out when we have the most customers and when we might need to do promotions or offer discounts to boost sales.

---

## Sales Breakdown

### Which Pizza Types Sell the Most?
- **Classic Pizzas**: 23.68% of total sales
- **Supreme Pizzas**: 26.91% of total sales
- **Chicken Pizzas**: 23.56% of total sales
- **Veggie Pizzas**: 23.96% of total sales

Classic and Supreme pizzas make up the biggest chunk of sales. This means we could focus more on promoting these types since people seem to prefer them.

---

### Which Pizza Sizes are the Most Popular?
- **Large**: 45.89% of sales
- **Medium**: 21.77% of sales
- **Regular**: 30.49% of sales
- **XXL**: 1.72% of sales

Large pizzas are by far the most popular, which makes sense because people tend to buy larger pizzas for groups. We could push the Large size more, especially when we expect a lot of orders.

---

## Where and When are Sales the Worst?

### By Day of the Week
- **Friday**: Most orders (3.5K orders)
- **Saturday**: Slightly fewer (3.2K orders)
- **Sunday**: Least orders (2.6K orders)

### By Month
- **January**: The month with the most orders (1,845 orders)
- **December**: The month with the fewest orders (1,680 orders), showing a dip toward the end of the year.

Weekends, especially Fridays and Saturdays, bring in the most orders. But January is the busiest month, while December tends to be slow. Knowing this can help us prepare for busy months and plan special deals during slower months to keep customers coming.


#### Key Numbers
- **Total Revenue**: $818,000
- **Average Order Value**: $38.31
- **Total Pizzas Sold**: 49,574
- **Total Orders**: 21,350
- **Average Pizzas Per Order**: 2.32

---

#### Monthly Trends

##### Total Orders by Month
Looking at the number of orders throughout the year, we see that **January** and **October** have the most orders. The number of orders tends to go down toward the end of the year, which might be because of less demand after the busy summer months.

This information helps us figure out when we have the most customers and when we might need to do promotions or offer discounts to boost sales.

---

#### Sales Breakdown

##### Which Pizza Types Sell the Most?
- **Classic Pizzas**: 23.68% of total sales
- **Supreme Pizzas**: 26.91% of total sales
- **Chicken Pizzas**: 23.56% of total sales
- **Veggie Pizzas**: 23.96% of total sales

Classic and Supreme pizzas make up the biggest chunk of sales. This means we could focus more on promoting these types since people seem to prefer them.

---

#### Which Pizza Sizes are the Most Popular?
- **Large**: 45.89% of sales
- **Medium**: 21.77% of sales
- **Regular**: 30.49% of sales
- **XXL**: 1.72% of sales

Large pizzas are by far the most popular, which makes sense because people tend to buy larger pizzas for groups. We could push the Large size more, especially when we expect a lot of orders.

---

#### Where and When are Sales the Worst?

##### By Day of the Week
- **Friday**: Most orders (3.5K orders)
- **Saturday**: Slightly fewer (3.2K orders)
- **Sunday**: Least orders (2.6K orders)

##### By Month
- **January**: The month with the most orders (1,845 orders)
- **December**: The month with the fewest orders (1,680 orders), showing a dip toward the end of the year.

Weekends, especially Fridays and Saturdays, bring in the most orders. But January is the busiest month, while December tends to be slow. Knowing this can help us prepare for busy months and plan special deals during slower months to keep customers coming.


## Best/Worst Selling Pizza

![Best-worst-selling-pizza](https://github.com/Konstanlytics/Pizza-Sales-Insight/blob/main/Best-Worst-Selling-Pizza.JPG)


### Key Numbers
- **Total Revenue**: $818,000
- **Average Order Value**: $38.31
- **Total Pizzas Sold**: 49,574
- **Total Orders**: 21,350
- **Average Pizzas Per Order**: 2.32

---

### Best-Selling Products

##### Revenue:
- The **Thai Chicken Pizza** is the top performer, bringing in **$43,434** in total sales.
- The **Barbecue Chicken Pizza** follows closely with **$42,768** in revenue.
- **California Chicken Pizza** made **$41,410** in revenue.
- **Classic Deluxe Pizza** generated **$31,814** in revenue.
- The **Spicy Italian Pizza** made **$34,831** in total revenue.

The **Thai Chicken Pizza** not only sells the most but also contributes the most to the business’s revenue.

---

### Total Quantities Sold:
- The **Classic Deluxe Pizza** is the top-seller in terms of quantity sold.
- The **Barbecue Chicken Pizza** and **California Chicken Pizza** also sell a lot of units.

---

### Worst-Selling Products

##### Revenue:
- The **Brie Carre Pizza** is the worst performer, bringing in the least amount of revenue, **$34,831**.
- The **Classic Deluxe Pizza** also performed poorly in revenue with **$31,814**.
- **The Spicy Italian Pizza** performed relatively poorly with **$34,831** in revenue.

### Total Orders:
- The **Brie Carre Pizza** had the fewest orders placed, indicating a need for improvement in sales or marketing for this product.

---

### Key Insights:
- The **Thai Chicken Pizza** is the clear winner in sales and revenue, and should continue to be promoted.
- The **Brie Carre Pizza** is the worst performer and may need reworking or a fresh marketing strategy to improve its sales.


