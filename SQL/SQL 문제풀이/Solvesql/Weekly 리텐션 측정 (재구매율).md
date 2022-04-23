# 주별 리텐션 측정

- 데이터

[United States E-Commerce records 2020](https://www.kaggle.com/ammaraahmad/us-ecommerce-record-2020)

- 소스코드

```sql
WITH records_preprocessed
     AS (SELECT r.customer_id,
       Date_format(Date_sub(`first_order_date`,
                   INTERVAL (Dayofweek(`first_order_date`)
                   -1) day), '%Y-%m-%d') AS
       first_order_week,
       Date_format(Date_sub(`order_date`, INTERVAL (Dayofweek(`order_date`)-1)
                                          day),
       '%Y-%m-%d')                                                    AS
       order_week
FROM   records r
       INNER JOIN (SELECT customer_id,
                          Min(order_date)          AS first_order_date,
                          Max(order_date)          AS last_order_date,
                          Count(DISTINCT order_id) AS cnt_orders,
                          Sum(sales)               AS sum_sales
                   FROM   records
                   GROUP  BY customer_id
                   ORDER  BY first_order_date) AS c
               ON r.customer_id = c.customer_id 
)
-- CTE
SELECT first_order_week
     , COUNT(DISTINCT customer_id) week0
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 1 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 2 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 3 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 4 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 5 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 6 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 7 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 8 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 9 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 10 WEEK) = order_week THEN customer_id END) AS week
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 11 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 12 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 13 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 14 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 15 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 16 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 17 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 18 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 19 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 20 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 21 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 22 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 23 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 24 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 25 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 26 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 27 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 28 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 29 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 30 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 31 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 32 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 33 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 34 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 35 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 36 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 37 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 38 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 39 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 40 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 41 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 42 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 43 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 44 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 45 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 46 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 47 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 48 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 49 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 50 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 51 WEEK) = order_week THEN customer_id END) AS week
		, COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_week, INTERVAL 52 WEEK) = order_week THEN customer_id END) AS week
from records_preprocessed
GROUP BY first_order_week
ORDER BY first_order_week
```