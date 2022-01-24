# Week3 - 실습문제 5

## dvd 대여가 가장 적은 도시는? (anlytic funtion)

```sql
SELECT ct.city
FROM   city ct
       INNER JOIN (SELECT a.city_id,
                          Count(p.rental_id)                rental_cnt,
                          Row_number()
                            OVER (
                              ORDER BY Count(p.rental_id) ) rnum
                   FROM   payment p
                          INNER JOIN customer c
                                  ON p.customer_id = c.customer_id
                          INNER JOIN address a
                                  ON a.address_id = c.address_id
                   GROUP  BY a.city_id) p
               ON ct.city_id = p.city_id
WHERE  p.rnum = 1;
```

```sql
-- 가장 많은도시 
SELECT   ct.city_id,
         ct.city,
         Count(r.rental_id),
         Row_number () over (ORDER BY count(r.rental_id) DESC) AS rnum
FROM     city ct
JOIN     address a
ON       a.city_id = ct.city_id
JOIN     customer c
ON       c.address_id = a.address_id
JOIN     rental r
ON       r.customer_id = c.customer_id
GROUP BY ct.city,
         ct.city_id
LIMIT    1;

-- 42 Aurora 50 1
```

```sql
-- 가장 적은 도시
SELECT   ct.city_id,
         ct.city,
         Count(r.rental_id),
         Row_number () over (ORDER BY count(r.rental_id)) AS rnum
FROM     city ct
	JOIN     address a
		ON       a.city_id = ct.city_id
	JOIN     customer c
		ON       c.address_id = a.address_id
	JOIN     rental r
		ON       r.customer_id = c.customer_id
GROUP BY ct.city,
         ct.city_id
LIMIT    1;

-- 97 Bydgoszcz 12 1
```

## 매출이 가장 안나오는 도시는? (anlytic funtion)

```sql
-- 매출이 가장 안나오는 도시는? (anlytic funtion)

SELECT   ct.city_id,
         ct.city,
         Count(p.amount),
         Row_number () over (ORDER BY count(p.amount)) AS amt
FROM     city ct
	JOIN     address a
		ON       a.city_id = ct.city_id
	JOIN     customer c
		ON       c.address_id = a.address_id
	JOIN     payment p 
		ON       p.customer_id = c.customer_id
GROUP BY ct.city,
         ct.city_id
LIMIT    1;

-- 97 Bydgoszcz 12 1
```

## 월별 매출액을 구하고 이전 월보다 매출액이 줄어든 월을 구하세요. 
(일자는 payment_date 기준)

```sql
-- 월별 매출액을 구하고 이전 월보다 매출액이 줄어든 월을 구하세요. 
-- (일자는 payment_date 기준)

select  *
from (
select to_char(p.payment_date, 'YYYY') as year,
		to_char(p.payment_date, 'mm') as mon,
		sum(p.amount) as amt,
		lag(sum(p.amount)) over(order by to_char(p.payment_date, 'mm')) as lag,
		((sum(p.amount)) - (lag(sum(p.amount)) over(order by to_char(p.payment_date, 'mm')))) as mns
from payment p
group by year, mon) as tab
where tab.mns < 0
;

-- 그냥  서브쿼리만 쓰면 where 이 안된다.
```

```sql
-- 이게 정답 
-- Extract(year FROM Date(p.payment_date)) 를 기억하자 
-- 날짜와 시간 형태 다루는 방법도 한번 정리 해야겠다.
SELECT *
FROM   (SELECT Extract(year FROM Date(p.payment_date)) as yr,
               Extract(month FROM Date(p.payment_date)) as mon,
               Sum(amount) as amt,
               Lag(Sum(amount))
                 OVER (ORDER BY Extract(month FROM Date(p.payment_date))) as pre_amt,
               Sum(amount) - Lag(Sum(amount))
                 OVER (ORDER BY Extract(month FROM Date(p.payment_date))) as gap
        FROM   payment p
        GROUP  BY Extract(year FROM Date(p.payment_date)),
                  Extract(month FROM Date(p.payment_date)))
WHERE  gap < 0;
```

## 도시별 dvd 대여 매출 순위를 구하세요.

```sql
--  도시별 dvd 대여 매출 순위를 구하세요.
SELECT ct.city,
       Sum(p.amount) as amt,
       Rank()
         OVER (
           ORDER BY Sum(p.amount) DESC ) rnk
FROM   payment p
       JOIN customer c
          ON p.customer_id = c.customer_id
       JOIN address a
          ON a.address_id = c.address_id
       JOIN city ct
          ON ct.city_id = a.city_id
GROUP  BY ct.city
;
```

## 대여점별 매출 순위를 구하세요.

```sql
SELECT i.store_id,
       Sum(p.amount) AS amt,
       Rank()
         OVER (
           ORDER BY Sum(p.amount) DESC ) rnum
FROM   payment p
       JOIN rental r
         ON r.rental_id = p.rental_id
       JOIN inventory i
         ON i.inventory_id = r.inventory_id
GROUP  BY i.store_id;
```