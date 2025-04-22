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

-- Calculate Revenue by Summing the Total Sales
SELECT SUM(total_price) AS Revenue
FROM pizza_sales;

-- Calculate the Average Order Value
SELECT SUM(total_price) / COUNT(DISTINCT order_id) AS Avg_Order_Value
FROM pizza_sales;

-- Calculate Total Pizzas Sold
SELECT SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales;

-- Total Orders Placed
SELECT COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales;

-- Average Pizzas Per Order
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2)) AS Avg_Pizza_Per_Order
FROM pizza_sales;

-- Daily Trend of Total Orders Arranged by Day of the Week (Sunday to Saturday)
SET DATEFIRST 7;
SELECT 
    DATENAME(WEEKDAY, order_date) AS Order_Day,
    COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY DATENAME(WEEKDAY, order_date), DATEPART(WEEKDAY, order_date)
ORDER BY DATEPART(WEEKDAY, order_date);

-- Daily Trend of Total Orders Sorted from Highest to Lowest
SELECT 
    DATENAME(WEEKDAY, order_date) AS Order_Day,
    COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY DATENAME(WEEKDAY, order_date), DATEPART(WEEKDAY, order_date)
ORDER BY Total_Orders DESC;

-- Monthly Trend of Total Orders Sorted in Descending Order
SELECT 
    DATENAME(MONTH, order_date) AS Month_Name,
    COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY DATENAME(MONTH, order_date)
ORDER BY Total_Orders DESC;

-- Percentage of Sales by Pizza Category
SELECT 
    pizza_category, 
    ROUND(SUM(total_price), 2) AS Total_Sales, 
    ROUND(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales), 2) AS Pct
FROM pizza_sales
GROUP BY pizza_category;

-- Percentage of Sales by Pizza Category for the Month of January
SELECT 
    pizza_category, 
    ROUND(SUM(total_price), 2) AS Total_Sales, 
    ROUND(SUM(total_price) * 100.0 / 
         (SELECT SUM(total_price) FROM pizza_sales WHERE MONTH(order_date) = 1), 2) AS Pct
FROM pizza_sales
WHERE MONTH(order_date) = 1
GROUP BY pizza_category;

-- Percentage of Sales by Pizza Size
SELECT 
    pizza_size, 
    ROUND(SUM(total_price), 2) AS Total_Sales, 
    ROUND(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales), 2) AS Pct
FROM pizza_sales
GROUP BY pizza_size;

-- Percentage of Sales by Pizza Size in Quarter 1
SELECT 
    pizza_size, 
    ROUND(SUM(total_price), 2) AS Total_Sales, 
    ROUND(SUM(total_price) * 100.0 / 
         (SELECT SUM(total_price) FROM pizza_sales WHERE DATEPART(QUARTER, order_date) = 1), 2) AS Pct
FROM pizza_sales
WHERE DATEPART(QUARTER, order_date) = 1
GROUP BY pizza_size;

-- Top 5 Best Sellers by Revenue
SELECT TOP 5 pizza_name,
    SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC;

-- Bottom 5 Sellers by Revenue
SELECT TOP 5 pizza_name, 
    SUM(total_price) AS Total_Revenue 
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue;

-- Top 5 Sellers by Total Quantity
SELECT TOP 5 pizza_name, 
    SUM(quantity) AS Total_Quantity 
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Quantity DESC;

-- Bottom 5 Sellers by Total Quantity
SELECT TOP 5 pizza_name, 
    SUM(quantity) AS Total_Quantity 
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Quantity ASC;

-- Top 5 Sellers by Total Orders
SELECT TOP 5 pizza_name, 
    COUNT(DISTINCT order_id) AS Total_Orders 
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders DESC;

-- Bottom 5 Sellers by Total Orders
SELECT TOP 5 pizza_name, 
    COUNT(DISTINCT order_id) AS Total_Orders 
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders;


## Data Analysis

### Insight

