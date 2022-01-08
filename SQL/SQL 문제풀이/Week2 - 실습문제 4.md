# Week2 - 실습문제 4

### dvd 대여를 제일 많이한 고객 이름은?

```sql
SELECT c.first_name,
       c.last_name
FROM   customer c
       INNER JOIN (SELECT p.customer_id,
                          Count(p.rental_id) rental_cnt
                   FROM   payment p
                   GROUP  BY p.customer_id
                   ORDER  BY rental_cnt DESC
                   LIMIT  1) p
               ON c.customer_id = p.customer_id;
```

### rental 테이블을  기준으로,   2005년 5월26일에 대여를 기록한 고객 중, 하루에 2번 이상 대여를 한 고객의 ID 값을 확인해주세요.

```sql
SELECT customer_id,
       Count(DISTINCT rental_id) as cnt
FROM   rental
WHERE  rental_date BETWEEN '2005-05-26 00:00:00' AND '2005-05-26 23:59:59'
GROUP  BY customer_id
HAVING Count(DISTINCT rental_id) > 1
order by cnt asc
```

### film_actor 테이블을 기준으로, 출현한 영화의 수가 많은  5명의 actor_id 와 , 출현한 영화 수 를 알려주세요.

```sql
SELECT actor_id,
       Count(DISTINCT film_id) AS cnt
FROM   film_actor
GROUP  BY actor_id
ORDER  BY cnt DESC
LIMIT  5
```

### payment 테이블을 기준으로,  결제일자가 2007년2월15일에 해당 하는 주문 중에서  ,  하루에 2건 이상 주문한 고객의  총 결제 금액이 10달러 이상인 고객에 대해서 알려주세요.
(고객의 id,  주문건수 , 총 결제 금액까지 알려주세요)

- 하루에 2건 이상 주문한 고객의  총 결제 금액이 10달러 이상인 고객 ⇒ 2건 + 10건 AND 교집합

```sql
SELECT customer_id,
       Count(DISTINCT rental_id) AS cnt,
       Sum(amount)               AS sum_amount
FROM   payment
WHERE  payment_date BETWEEN '2007-02-15 00:00:00' AND '2007-02-15 23:59:59'
GROUP  BY customer_id
HAVING Count(DISTINCT rental_id) >= 2
       AND Sum(amount) >= 10
```

### 사용되는 언어별 영화 수는?

- ~~별 → 그룹바이

```sql
SELECT l.NAME,
       Count(*) as cnt
FROM   language l
       INNER JOIN film f
               ON l.language_id = f.language_id
GROUP  BY l.NAME
```

### 40편 이상 출연한 영화 배우(actor) 는 누구인가요?

```sql
SELECT a.first_name,
       a.last_name,
       fc.cnt
FROM   (SELECT fc.actor_id,
               Count(*) AS cnt
        FROM   film_actor fc
        GROUP  BY fc.actor_id
        HAVING Count(*) >= 40) fc
       INNER JOIN actor AS a
               ON a.actor_id = fc.actor_id;
```

### 고객 등급별 고객 수를 구하세요. (대여 금액 혹은 매출액  에 따라 고객 등급을 나누고 조건은 아래와 같습니다.)

<aside>
❣️ A 등급은 151 이상
B 등급은 101 이상 150 이하
C 등급은   51 이상 100 이하
D 등급은   50 이하

- 대여 금액의 소수점은 반올림 하세요.
- HINT : 반올림 하는 함수는 ROUND 입니다.
</aside>

```sql
SELECT 
			CASE
				 WHEN rental_amount >= 151 THEN 'A'
				 WHEN rental_amount BETWEEN 101 AND 150 THEN 'B'
				 WHEN rental_amount BETWEEN 51 AND 100 THEN 'C'
        		 WHEN rental_amount <= 50 THEN 'D'
       END customer_class,
			 Count(*) as cnt
FROM   (SELECT r.customer_id,
        Round(Sum(p.amount), 0) as rental_amount
		FROM   rental r
               INNER JOIN payment p
                       ON p.rental_id = r.rental_id
        GROUP  BY r.customer_id) r
GROUP  BY CASE
            WHEN rental_amount >= 151 THEN 'A'
				 WHEN rental_amount BETWEEN 101 AND 150 THEN 'B'
				 WHEN rental_amount BETWEEN 51 AND 100 THEN 'C'
        		 WHEN rental_amount <= 50 THEN 'D'
          END
```