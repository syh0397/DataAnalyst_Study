# Week3 - μ€μ΅λ¬Έμ  8

## πΒ κ³ κ° λ±κΈλ³ κ³ κ° μλ₯Ό κ΅¬νμΈμ. 
(λμ¬ νμμ λ°λΌ κ³ κ° λ±κΈμ λλκ³  μ‘°κ±΄μ μλμ κ°μ΅λλ€.)

A λ±κΈμ 31ν μ΄μ
B λ±κΈμ 21ν μ΄μ 30ν μ΄ν
C λ±κΈμ 11ν μ΄μ 20ν μ΄ν
D λ±κΈμ 10ν μ΄ν

- μ΄κ±΄ κ³μ λ€μ λ΄μΌκ² λ€.

```sql
-- κ³ κ° λ±κΈλ³ κ³ κ° μλ₯Ό κ΅¬νμΈμ. 
-- (λμ¬ νμμ λ°λΌ κ³ κ° λ±κΈμ λλκ³  μ‘°κ±΄μ μλμ κ°μ΅λλ€.)
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

## κ³ κ° μ΄λ¦ λ³λ‘ , flag  λ₯Ό λΆμ¬μ λ³΄μ¬μ£ΌμΈμ.

- κ³ κ°μ first_name μ΄λ¦μ μ²«λ²μ§Έ κΈμκ°, A, B,C μ ν΄λΉ νλ μ¬λμ  κ° A,B,C λ‘ flag λ₯Ό λΆμ¬μ£Όμκ³ 
   A,B,C μ ν΄λΉνμ§ μλ μΈμμ λν΄μλ Others λΌλ flag λ‘  λΆμ¬μ£ΌμΈμ.

- μ¬μ©λ² :Β `COALESCE(param1, param2)`
    - μ€λͺ : param1 μ κ°μ΄ nullμ΄λ©΄ β param2μ κ°μΌλ‘ λ°ν νλ€.
    
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
-- κ³ κ° μ΄λ¦ λ³λ‘ , flag  λ₯Ό λΆμ¬μ λ³΄μ¬μ£ΌμΈμ.
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