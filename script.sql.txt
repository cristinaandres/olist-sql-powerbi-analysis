/* ====================================================================
   PROJECT: Olist E-Commerce - Delivery Performance & Customer Satisfaction
   AUTHOR: Cristina Andrés
   TOOL: SQLite (DBeaver)
   OBJECTIVE: Analyze how delivery delays impact the customer review score.
   ==================================================================== */

-- ====================================================================
-- QUERY 1: Initial exploration and delay calculation
-- Objective: Join the orders and reviews tables and calculate the 
-- difference in days between the actual and estimated delivery dates.
-- ====================================================================

SELECT 
    o.order_id,
    r.review_score,
    -- Use julianday() to convert text dates into numbers for subtraction.
    -- A positive result means a delayed delivery. Negative or 0 means on time.
    CAST(julianday(o.order_delivered_customer_date) - julianday(o.order_estimated_delivery_date) AS INTEGER) AS delay_days
FROM olist_orders_dataset o
-- INNER JOIN using 'order_id' as the primary key to connect orders and reviews.
JOIN olist_order_reviews_dataset r 
    ON o.order_id = r.order_id
-- Filter to exclude undelivered orders (null dates).
WHERE o.order_delivered_customer_date IS NOT NULL
ORDER BY delay_days DESC;


-- ====================================================================
-- QUERY 2: Grouping and Impact Analysis (Main Finding)
-- Objective: Group orders into delay categories to calculate the average 
-- review score for each category.
-- ====================================================================

SELECT 
    -- Use CASE WHEN to create delay categories ("buckets"). 
    -- This allows us to compare the impact of mild vs. severe delays.
    CASE 
        WHEN CAST(julianday(o.order_delivered_customer_date) - julianday(o.order_estimated_delivery_date) AS INTEGER) <= 0 THEN '1. On time or early'
        WHEN CAST(julianday(o.order_delivered_customer_date) - julianday(o.order_estimated_delivery_date) AS INTEGER) BETWEEN 1 AND 5 THEN '2. 1-5 days late'
        WHEN CAST(julianday(o.order_delivered_customer_date) - julianday(o.order_estimated_delivery_date) AS INTEGER) BETWEEN 6 AND 15 THEN '3. 6-15 days late'
        ELSE '4. More than 15 days late'
    END AS delivery_status,
    
    -- Count the total number of orders in each category.
    COUNT(o.order_id) AS total_orders,
    
    -- Calculate the average review score and round to 2 decimal places.
    ROUND(AVG(r.review_score), 2) AS avg_review_score
FROM olist_orders_dataset o
JOIN olist_order_reviews_dataset r 
    ON o.order_id = r.order_id
WHERE o.order_delivered_customer_date IS NOT NULL
-- Group results by the category created in the CASE WHEN statement.
GROUP BY delivery_status
-- Order alphabetically so categories appear in numerical order (1, 2, 3, 4).
ORDER BY delivery_status;