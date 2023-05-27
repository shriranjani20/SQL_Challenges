# SQL_Challenges
SHOW DATABASES;
USE PIZZA_RUNNER;
SHOW TABLES;
/*1*/
/*How many pizzas were ordered?*/
SELECT * FROM CUSTOMER_ORDERS;
SELECT COUNT(*) FROM CUSTOMER_ORDERS;

/*2.How many unique customer orders were made?*/

SELECT COUNT(DISTINCT(CUSTOMER_ID)) FROM CUSTOMER_ORDERS;

/*3.we can count how many orders each customer made*/
SELECT CUSTOMER_ID,COUNT(CUSTOMER_ID) FROM CUSTOMER_ORDERS
GROUP BY CUSTOMER_ID ;
/*How many successful orders were delivered by each runner*/
SELECT * FROM RUNNER_ORDERS;

SELECT RUNNER_ID,COUNT(RUNNER_ID) AS SUCCESSFUL_DELIVERY FROM RUNNER_ORDERS
WHERE CANCELLATION IS NULL
GROUP BY RUNNER_ID;

/*How many of each type of pizza was delivered?*/
select * from PIZZA_NAMES;

SELECT * FROM CUSTOMER_ORDERS;

SELECT PIZZA_ID,COUNT(PIZZA_ID) FROM CUSTOMER_ORDERS C
JOIN RUNNER_ORDERS R ON C.ï»¿order_id = R.ï»¿order_id
WHERE CANCELLATION IS NULL
GROUP BY PIZZA_ID;

/*How many Vegetarian and Meatlovers were ordered by each customer?*/

SELECT C.CUSTOMER_ID,C.PIZZA_ID,COUNT(PIZZA_ID)   FROM CUSTOMER_ORDERS C
JOIN RUNNER_ORDERS R ON C.ï»¿order_id = R.ï»¿order_id
WHERE CANCELLATION IS NULL
GROUP BY  PIZZA_ID,CUSTOMER_ID;

/*What was the maximum number of pizzas delivered in a single order*/ 
SELECT * FROM RUNNER_ORDERS;

WITH CTE AS (SELECT ï»¿order_id,COUNT(ï»¿order_id) AS CT FROM CUSTOMER_ORDERS
GROUP BY ï»¿order_id)
SELECT MAX(CT) AS MAX_ORDERS FROM CTE;

/*For each customer, how many delivered pizzas had at least 1 change and how many had no changes?*/
WITH CTE AS (SELECT CUSTOMER_ID, COUNT(EXCLUSIONS) FROM CUSTOMER_ORDERS
WHERE EXCLUSIONS IS NOT NULL 
GROUP BY EXCLUSIONS,CUSTOMER_ID )
SELECT * FROM CTE ;

SELECT CUSTOMER_ID , (COUNT(EXCLUSIONS)+COUNT(EXTRAS)) AS CHANGES FROM CUSTOMER_ORDERS C
JOIN RUNNER_ORDERS R ON C.ï»¿order_id = R.ï»¿order_id
WHERE R.CANCELLATION IS NULL 
GROUP BY CUSTOMER_ID;

/*How many pizzas were delivered that had both exclusions and extras*/

SELECT CUSTOMER_ID,COUNT(CUSTOMER_id) FROM CUSTOMER_ORDERS
WHERE EXCLUSIONS IS NOT NULL AND EXTRAS IS NOT NULL AND CANCELLATION 
GROUP BY CUSTOMER_ID;

/*What was the total volume of pizzas ordered for each hour of the day?
*/
SELECT * FROM CUSTOMER_ORDERS;
SELECT ORDER_TIME FROM CUSTOMER_ORDERS;

describe CUSTOMER_ORDERS;
ALTER TABLE CUSTOMER_ORDERS
MODIFY ORDER_TIME DATETIME;

DESCRIBE CUSTOMER_ORDERS;
/*What was the total volume of pizzas ordered for each hour of the day?
*/

WITH CTE AS (SELECT *,hour(ORDER_TIME) as hr FROM CUSTOMER_ORDERS)
SELECT cte.hr,count(customer_id) from cte
group by cte.hr
order by cte.hr;

/*What was the volume of orders for each day of the week?*/

WITH CTE AS (SELECT *,dayname(order_time) as day FROM CUSTOMER_ORDERS)
SELECT cte.day,count(customer_id) as count_orders from cte
group by cte.day
order by count(customer_id);






 
