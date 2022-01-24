# Week3 - 실습문제 4

## dvd 대여를 제일 많이한 고객 이름은? (analytic funtion 활용)

- analytic funtion  = window function

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       Count(r.rental_id) AS rental
FROM   customer c
       JOIN rental r
         ON r.customer_id = c.customer_id
GROUP  BY c.customer_id
ORDER  BY rental DESC;

-- 148번 엘리나헌트씨
```

```sql
-- 이게 정답이라는데  <148번 엘리나헌트씨>

SELECT c.first_name,
       c.last_name
FROM   customer c
       INNER JOIN (SELECT p.customer_id,
                          Count(p.rental_id)as rental_cnt,
                          Row_number()
                            OVER (
                              ORDER BY Count(p.rental_id) DESC) as row_num
                   FROM   payment p
                   GROUP  BY p.customer_id) p
               ON c.customer_id = p.customer_id
WHERE  p.row_num = 1;
```

```sql
-- 근데 내가 짠건 이거 

SELECT c.first_name,
       c.last_name,
	   Count(r.rental_id) as rental_cnt,
       Row_number() OVER (ORDER BY Count(r.rental_id) DESC) as row_num
FROM   customer c
       JOIN rental r
         ON r.customer_id = c.customer_id
GROUP  BY c.customer_id
ORDER  BY rental_cnt DESC;
```

## 매출을 가장 많이 올린 dvd 고객 이름은? (analytic funtion 활용)

```sql
SELECT c.first_name,
       c.last_name
FROM   customer c
       INNER JOIN (SELECT p.customer_id,
                          Sum(p.amount) as rental_amount,
                          Row_number()
                            OVER (
                              ORDER BY Sum(p.amount) DESC) rnum
                   FROM   payment p
                   GROUP  BY p.customer_id) p
               ON c.customer_id = p.customer_id
WHERE  p.rnum = 1;

-- Eleanor	Hunt
```

```sql
-- 근데 내가 짠건 이거  Eleanor	Hunt 211.55

SELECT c.first_name,
       c.last_name,
	    sum(p.amount) as rental_cnt,
       Row_number() OVER (ORDER BY sum(p.amount) DESC) as row_num
FROM   customer c
       JOIN payment p
         ON p.customer_id = c.customer_id
GROUP  BY c.customer_id
ORDER  BY rental_cnt DESC;
```