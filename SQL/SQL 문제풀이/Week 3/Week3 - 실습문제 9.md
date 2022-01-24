# Week3 - 실습문제 9

## payment 테이블을 기준으로,  
2007년 1월~ 3월 까지의 결제일에 해당하며,  staff2번 인원에게 결제를 진행한  결제건에 대해서는, Y 로
그 외에 대해서는 N 으로 표기해주세요.

with 절을 이용해주세요.

```sql
WITH tbl
     AS (SELECT 2                                        AS staff_id,
                Cast('2007-01-01 00:00:00' AS TIMESTAMP) AS chk1,
                Cast('2007-03-31 23:59:59' AS TIMESTAMP) AS chk2)
SELECT p.*,
       CASE
         WHEN tbl.staff_id IS NOT NULL THEN 'Y'
         ELSE 'N'
       END flag
FROM   payment p
       LEFT OUTER JOIN tbl
                    ON p.payment_date BETWEEN tbl.chk1 AND tbl.chk2
                       AND p.staff_id = tbl.staff_id;
```

```sql
SELECT 2 as staff_id,
 		to_date('2007-01-01','YYYY/MM/DD' ) as lft,
		to_date('2007-01-03','YYYY/MM/DD') as rgt

-- to_date 혹은 to_char 쓸때 들어가는 양식 주의 '-' 들어가야함

-- payment 테이블을 기준으로,  
-- 2007년 1월~ 3월 까지의 결제일에 해당하며,  
-- staff 2번 인원에게 결제를 진행한  결제건에 대해서는, Y 로
-- 그 외에 대해서는 N 으로 표기해주세요. 
with tbl
	as (
		SELECT 2 as staff_id,
 		to_date('20070101000000','YYYYMMDDHH24' ) as lft,
		to_date('20070103000000','YYYYMMDDHH24') as rgt
		)
select p.*,
		CASE
         WHEN tbl.staff_id IS NOT NULL THEN 'Y'
         ELSE 'N'
       END flag
from payment p 
	left join tbl
		ON p.payment_date BETWEEN tbl.lft AND tbl.rgt
                       	AND p.staff_id = tbl.staff_id;

;

--- 이건 왜 안될까 ? 

SELECT 2 as staff_id,
 		to_date('20070101000000','YYYYMMDDHH24' ) as lft,
		to_date('20070103000000','YYYYMMDDHH24') as rgt;
```

<aside>
⛔ **SELECT TO_DATE(문자열, FORMAT) FROM DUAL;**

**SELECT TO_TIMETAMP(문자열, FORMAT) FROM DUAL;**

찾아보니까 **TO_DATE, TO_TIMETAMP** 이거 두개가 다르네 ,,, 

</aside>

## Payement 테이블을 기준으로,  결제에 대한 Quarter 분기를 함께 표기해주세요.
with 절을 활용해서 풀이해주세요.

1~월의 경우 Q1
4~6월 의 경우 Q2
7~9월의 경우 Q3
10~12월의 경우 Q4

```sql
WITH tbl
     AS (SELECT Cast('2007-01-01 00:00:00' AS TIMESTAMP) AS chk1,
                Cast('2007-03-31 23:59:59' AS TIMESTAMP) AS chk2,
                'Q1'                                     AS quarter
         UNION ALL
         SELECT Cast('2007-04-01 00:00:00' AS TIMESTAMP) AS chk1,
                Cast('2007-06-30 23:59:59' AS TIMESTAMP) AS chk2,
                'Q2'                                     AS quarter
         UNION ALL
         SELECT Cast('2007-07-01 00:00:00' AS TIMESTAMP) AS chk1,
                Cast('2007-09-30 23:59:59' AS TIMESTAMP) AS chk2,
                'Q3'                                     AS quarter
         UNION ALL
         SELECT Cast('2007-10-01 00:00:00' AS TIMESTAMP) AS chk1,
                Cast('2007-12-31 23:59:59' AS TIMESTAMP) AS chk2,
                'Q4'                                     AS quarter)
SELECT p.*,
       tbl.quarter
FROM   payment p
       LEFT OUTER JOIN tbl
                    ON p.payment_date BETWEEN tbl.chk1 AND tbl.chk2;
```

## Rental 테이블을 기준으로,  회수일자에 대한 Quater 분기를 함께 표기해주세요. with 절을 활용해서 풀이해주세요.

1~월의 경우 Q1
4~6월 의 경우 Q2
7~9월의 경우 Q3
10~12월의 경우 Q4 로 함께 보여주세요.

```sql
WITH tbl
     AS (SELECT Cast('2007-01-01 00:00:00' AS TIMESTAMP) AS chk1,
                Cast('2007-03-31 23:59:59' AS TIMESTAMP) AS chk2,
                'Q1'                                     AS quarter
         UNION ALL
         SELECT Cast('2007-04-01 00:00:00' AS TIMESTAMP) AS chk1,
                Cast('2007-06-30 23:59:59' AS TIMESTAMP) AS chk2,
                'Q2'                                     AS quarter
         UNION ALL
         SELECT Cast('2007-07-01 00:00:00' AS TIMESTAMP) AS chk1,
                Cast('2007-09-30 23:59:59' AS TIMESTAMP) AS chk2,
                'Q3'                                     AS quarter
         UNION ALL
         SELECT Cast('2007-10-01 00:00:00' AS TIMESTAMP) AS chk1,
                Cast('2007-12-31 23:59:59' AS TIMESTAMP) AS chk2,
                'Q4'                                     AS quarter)
SELECT r.*,
       tbl.quarter
FROM   rental r
       LEFT OUTER JOIN tbl
                    ON r.return_date BETWEEN tbl.chk1 AND tbl.chk2;
```