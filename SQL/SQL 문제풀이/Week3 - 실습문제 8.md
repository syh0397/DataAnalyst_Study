# Week3 - 실습문제 8

## 📌 고객 등급별 고객 수를 구하세요. 
(대여 횟수에 따라 고객 등급을 나누고 조건은 아래와 같습니다.)

A 등급은 31회 이상
B 등급은 21회 이상 30회 이하
C 등급은 11회 이상 20회 이하
D 등급은 10회 이하

- 이건 계속 다시 봐야겠다.

```sql
-- 고객 등급별 고객 수를 구하세요. 
-- (대여 횟수에 따라 고객 등급을 나누고 조건은 아래와 같습니다.)
WITH tbl
     AS (SELECT 0   AS from_,
                10  AS until_,
                'D' AS grade
         UNION ALL
         SELECT 11  AS from_,
                20  AS until_,
                'C' AS grade
         UNION ALL
         SELECT 21  AS from_,
                30  AS until_,
                'B' AS grade
         UNION ALL
         SELECT 31  AS from_,
                99  AS until_,
                'A' AS grade)
SELECT tbl.grade,
       Count(rent.customer_id)
FROM   (SELECT r.customer_id,
               Count(r.rental_id) AS cnt
        FROM   rental r
        GROUP  BY r.customer_id
        ORDER  BY cnt) AS rent
        JOIN tbl
         ON rent.cnt BETWEEN tbl.from_ AND tbl.until_
GROUP  BY tbl.grade

A	134
B	409
C	56
```

```sql
WITH tbl
     AS (SELECT 0   AS chk1,
                10  AS chk2,
                'D' AS grade
         UNION ALL
         SELECT 11  AS chk1,
                20  AS chk2,
                'C' AS grade
         UNION ALL
         SELECT 21  AS chk1,
                30  AS chk2,
                'B' AS grade
         UNION ALL
         SELECT 31  AS chk1,
                99  AS chk2,
                'A' AS grade)
SELECT tbl.grade,
       Count(customer_id) AS cnt
FROM   (SELECT customer_id,
               Count(*) rental_cnt
        FROM   rental r
        GROUP  BY customer_id
        ORDER  BY rental_cnt DESC) r
       LEFT OUTER JOIN tbl
                    ON r.rental_cnt BETWEEN tbl.chk1 AND tbl.chk2
GROUP  BY tbl.grade;

A	134
C	56
B	409
```

## 고객 이름 별로 , flag  를 붙여서 보여주세요.

- 고객의 first_name 이름의 첫번째 글자가, A, B,C 에 해당 하는 사람은  각 A,B,C 로 flag 를 붙여주시고
   A,B,C 에 해당하지 않는 인원에 대해서는 Others 라는 flag 로  붙여주세요.

- 사용법 : `COALESCE(param1, param2)`
    - 설명 : param1 의 값이 null이면 → param2의 값으로 반환 한다.
    
    | PostgreSQL | ORACLE | MSSQL |
    | --- | --- | --- |
    | COALESCE(param1, param2) | NVL(param1, param2) | ISNULL(param1, param2) |

```sql
WITH tbl
     AS (SELECT 'A' AS first_name
         UNION ALL
         SELECT 'B' AS first_name
         UNION ALL
         SELECT 'C' AS first_name)
SELECT c.*,
       COALESCE(tbl.first_name, 'Others') AS flag
FROM   customer c
       LEFT OUTER JOIN tbl
                    ON Substring(c.first_name, 1, 1) = tbl.first_name;
```

```sql
-- 고객 이름 별로 , flag  를 붙여서 보여주세요.
with tbl as 
		(SELECT 'A' AS first_name
         UNION ALL
         SELECT 'B' AS first_name
         UNION ALL
         SELECT 'C' AS first_name
         UNION all
         select 'Others' as first_name)
select *
from customer c
join tbl
on substring( c.first_name,1,1) = tbl.first_name;
```