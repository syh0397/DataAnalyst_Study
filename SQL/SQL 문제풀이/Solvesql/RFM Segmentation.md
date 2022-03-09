# RFM Segmentation

1. RFM 분석의 3가지 지표 집계하기
    - Recency: 최근 구매일 - 최근에 구매하였는가?
    - Frequency: 구매 횟수 - 얼마나 자주 구매하였는가?
    - Monetary: 구매 금액 합계 -  얼마나 돈을 썼는가?

```sql
select customer_id,
      count(customer_id) as frequency,
      sum(sales) as monetary,
      max(order_date) AS recent_order
from records
group by customer_id
```

```sql
SELECT customer_id,
	ntile(5) OVER ( ORDER BY recent_order ) AS rfm_recency,
	ntile(5) OVER ( ORDER BY total_orders ) AS rfm_frequency,
	ntile(5) OVER ( ORDER BY total_sales ) AS rfm_monetary
from(select customer_id,
      count(customer_id) as total_orders,
      sum(sales) as total_sales,
      max(order_date) AS recent_order
from records
group by customer_id) as t
```

```sql
WITH preprocessing_tbl
     AS (
        /* 5등분한 테이블 */
        SELECT customer_id,
               Ntile(5)
                 OVER (
                   ORDER BY recent_order ) AS rfm_recency,
               Ntile(5)
                 OVER (
                   ORDER BY total_orders ) AS rfm_frequency,
               Ntile(5)
                 OVER (
                   ORDER BY total_sales )  AS rfm_monetary
         /* RFM 을 계산한 테이블 */
         FROM   (SELECT customer_id,
                        Count(customer_id) AS total_orders,
                        Sum(sales)         AS total_sales,
                        Max(order_date)    AS recent_order
                 FROM   records
                 GROUP  BY customer_id) AS tbl1)
/* RFM CELL 만들기 */
SELECT customer_id,
       rfm_recency,
       rfm_frequency,
       rfm_monetary,
       Concat(rfm_recency, rfm_frequency, rfm_monetary) AS rfm_cell
FROM   preprocessing_tbl
```