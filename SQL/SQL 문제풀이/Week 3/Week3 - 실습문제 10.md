# Week3 - 실습문제 10

## 직원이  월별  대여를 진행 한  대여 갯수가 어떻게 되는 지 알려주세요.
대여 수량이   아래에 해당 하는 경우에 대해서, 각 flag 를 알려주세요 .

0~ 500개 의 경우  under_500
501~ 3000 개의 경우  under_3000
3001 ~ 99999 개의 경우  over_3001

```sql
-- 월별  대여를 진행 한  대여 갯수

SELECT To_char(rental_date, 'YYYY/mm') as Mon,
               staff_id,
               Count(rental_id)   as  cnt
        FROM   rental r
        GROUP  BY To_char(rental_date, 'YYYY/mm'),
                  staff_id;
---
                 
 WITH tbl
     AS (SELECT 0           as lft,
                500         AS rgt,
                'under_500' AS flag
         UNION ALL
         SELECT 501          AS lft,
                3000         AS rgt,
                'under_3000' AS flag
         UNION ALL
         SELECT 3001        AS lft,
                99999       AS rgt,
                'over_3001' AS flag)
select  sb.mon,
		sb.staff_id,
		tbl.flag
from (SELECT To_char(rental_date, 'YYYY/mm') as Mon,
               staff_id,
               Count(rental_id)   as  cnt
        FROM   rental r
        GROUP  BY To_char(rental_date, 'YYYY/mm'),
                  staff_id) sb
left join tbl
		on sb.cnt between tbl.lft and tbl.rgt
order by mon
		;
```

## 직원의 현재 패스워드에 대해서, 새로이  패스워드를 지정하려고 합니다.
직원1의 새로운 패스워드는 12345  ,  직원2의 새로운 패스워드는 54321입니다.
해당의 경우, 직원별로 과거 패스워드와 현재 새로이 업데이트할 패스워드를
함께 보여주세요.

- with 절을 활용하여  새로운 패스워드 정보를 저장 후 , 알려주세요.

```sql
WITH tbl
     AS (SELECT 1       AS staff_id,
                '12345' AS new_pwd
         UNION ALL
         SELECT 2       AS staff_id,
                '54321' AS new_pwd)
SELECT s.staff_id,
       s.password,
       tbl.new_pwd
FROM   staff s
       JOIN tbl 
         ON s.staff_id = tbl.staff_id;

1	8cb2237d0679ca88db6464eac60da96345513964	12345
2	8cb2237d0679ca88db6464eac60da96345513964	54321
```