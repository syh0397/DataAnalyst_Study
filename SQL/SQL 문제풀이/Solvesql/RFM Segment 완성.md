# RFM Segmentation 2 완성

```sql
/* 구분 기준별 나누기  */
SELECT customer_id,
			rfm_recency,
       rfm_frequency,
       rfm_monetary,
       rfm_cell,
       CASE
         WHEN rfm_cell IN ( '355', '255' ) THEN 
					'Cannot lose'
         WHEN rfm_cell IN ( '543', '542', '453', '452' ) THEN 
					'Active fans'
         WHEN rfm_cell IN ( '525', '524', '515', '514' ) THEN
         'Promising newbies'
         WHEN rfm_cell IN ( '335', '334', '325', '324' ) THEN
         'Potential churners'
         ELSE 'Other'
       END         AS rfm_segment
FROM   (
       /* 서브쿼리 - RFM cell 만들기  */
       SELECT customer_id,
              rfm_recency,
              rfm_frequency,
              rfm_monetary,
              Concat(rfm_recency, rfm_frequency, rfm_monetary) AS rfm_cell
        FROM   (
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
                        GROUP  BY customer_id) AS tbl1) tbl2) AS tbl3
```