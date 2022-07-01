# aikeen_industries_data_exploration
Data Exploration of Aikeen industries with MySql - Test Mode
/*Aikeen Industries, a leading manufacturer and seller of gourmet crips & nachos
products and has requested our services.
Aikeen Industries is looking at its operations wanting to sync all aspects of its business
from production to forecasting. Aikeen Industries’ priority is to ensure a strong
production, selling and forecasting business model to take the company to the phase 1
of their expansion plan.*/
/* CREATE DATABASE aikeen;
USE aikeen;
 SHOW tables;
SELECT * FROM sales_orders;
SELECT * FROM products;
SELECT * FROM production_sales_forecast;
SELECT * FROM machines;
SELECT * FROM actual_production;
SELECT * FROM finished_goods_inventory;
*/
/* 1. Sales team performance by comparing Sales Forecast to Production Forecast. */
/* CREATE temporary TABLE sfvspf*/
/* SELECT 
DISTINCT(forecast_type),
SUM(forecast_units)
FROM
production_sales_forecast
GROUP BY forecast_type
ORDER BY 2 DESC;
*/

/*2. Inefficiencies in forecast by comparing Actual Production to Production Forecast. */
/*SELECT * FROM production_sales_forecast;
SELECT * FROM actual_production;
SELECT 
production_sales_forecast.forecast_units-
actual_production.quantity,
forecast_type
AS 
forecast_ineffciencies
FROM 
actual_production, production_sales_forecast; */

/* 3. Identify products with high inaccuracies in forecasts based on historical data (2019 till date)*/
/*SELECT * FROM actual_production;
SELECT * FROM production_sales_forecast;
SELECT * FROM finished_goods_inventory;*/
/*SELECT
day(production_sales_forecast.date) AS day,
month(production_sales_forecast.date) AS month,
year(production_sales_forecast.date) AS year,
COUNT(DISTINCT production_sales_forecast.product_no) AS products,
COUNT(DISTINCT CASE WHEN forecast_type = 'Production Forecast' THEN production_sales_forecast.forecast_type ELSE NULL END) AS production_forecast,
COUNT(DISTINCT CASE WHEN forecast_type = 'Sales Forecast' THEN production_sales_forecast.forecast_type ELSE NULL END) AS sales_forecast
FROM 
production_sales_forecast
WHERE date > 1/1/2020
GROUP BY 1,2,3;
/*GROUP BY forecast_type, product_no, forecast_units
ORDER BY forecast_type, product_no, forecast_units;*/
/*GROUP BY forecast_type
ORDER BY product_no;*/

/* 4. How many shortages of orders (compare Sales Orders vs Finished Goods Inventory?
SELECT * FROM finished_goods_inventory;
SELECT * FROM sales_orders;
SELECT 
SUM(quantity),
SUM(total_orders)
FROM 
sales_orders
LEFT JOIN finished_goods_inventory
ON sales_orders.quantity = finished_goods_inventory.closing;
-- Qty=170181 total_orders=125051-- */

/* 5. Calculate Sell-through rate: Sell-through rate = (Units Consumed/Units Produced)
SELECT 
SUM(consumed)/SUM(produced)
FROM 
finished_goods_inventory;
-- Sell-through rate = -1.1333 -- */

/* 6. Accuracy of Forecast = [(Actual Production/Production Forecast) *100] 
SELECT * FROM actual_production;
SELECT * FROM production_sales_forecast;
SELECT 
SUM(actual_production.quantity)/SUM(production_sales_forecast.forecast_units)*100
FROM 
actual_production, production_sales_forecast;
-- Accuracy of forecast = 21.33%-- 
*/
/*7. Identify items in Finished Goods Inventory for which we have no or low sales
(consumed means sold). */
/* DESC finished_goods_inventory;
DESC sales_orders;
SELECT * FROM finished_goods_inventory;
SELECT * FROM sales_orders;
SELECT 
total_orders,
product_no AS item_no,
consumed AS sold_items
FROM 
finished_goods_inventory
LEFT JOIN sales_orders
ON 
finished_goods_inventory.sold_items = sales_orders.total_orders
GROUP BY total_orders
ORDER BY total_orders DESC;
*/


/*8. Fill rate = [(**Total Units in inventory-Consumed Units)/Total Units in inventory]*100*/
/*
SELECT  
SUM(closing),
+ SUM(opening) 
- 
SUM(consumed)/
SUM(opening) + SUM(closing) * 100
AS fill_rate
FROM finished_goods_inventory;
-- 163630.6870 --
*/


/* 9. Predict the remaining 2022 forecast using Actual Production vs Production Forecast 
-- Test Query--*/
/*SELECT * FROM actual_production;
SELECT * FROM production_sales_forecast;
/*SELECT STR_TO_DATE("3/1/2020", "%e/%m/%Y");*/
/*SELECT 
DATE_FORMAT(STR_TO_DATE("1/1/2022", "%e/%m/%Y"),"%m-%d-%Y") as my_new_date,
forecast_type,
forecast_units
FROM 
production_sales_forecast
LEFT JOIN 
actual_production 
ON production_sales_forecast.forecast_units = actual_production.quantity
WHERE forecast_type = 'Production Forecast'
AND my_new_date BETWEEN '2/1/2022' AND '9/1/2022'
GROUP BY 1,2;
*/


DESC production_sales_forecast;
/*SELECT 
date,
CONVERT(VARCHAR(255), date, 103) AS "new_date",
FROM production_sales_forecast; */

/*SELECT CAST(date as DATE)
FROM 
production_sales_forecast;
*/


/* 10. Predict the 2022 sales team performance using Sales Forecast vs Production
Forecast */
-- Test Query WIP -- 
/* SELECT * FROM production_sales_forecast;
SELECT STR_TO_DATE("3/1/2020", "%e/%m/%Y");
SELECT DATE_FORMAT(STR_TO_DATE("3/1/2020", "%e/%m/%Y"),"%m-%d-%Y") as my_new_date 
FROM production_sales_forecast;
*/
/*SELECT * FROM sales_orders;
SELECT * FROM production_sales_forecast;
SELECT DATE_FORMAT(STR_TO_DATE("1/1/2022", "%e/%m/%Y"),"%m-%d-%Y") as my_new_date,
forecast_units,
forecast_type
FROM 
production_sales_forecast
LEFT JOIN sales_orders
ON 
sales_orders.total_orders = production_sales_forecast.forecast_units
WHERE date >= '1/1/2022'
GROUP BY 1,2;
*/



