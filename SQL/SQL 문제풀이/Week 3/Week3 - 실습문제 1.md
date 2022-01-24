# Week3 - 실습문제 1

### 매출을 가장 많이 올린 dvd 고객 이름은? (subquery 활용)

```sql
select  first_name , last_name
from customer c
where  customer_id in (
select customer_id
from payment p
group by customer_id
order by sum(amount) desc
limit 1
)
```

```sql
-- 매출자 id in payment table

select payment_id , 
		sum(amount) as amount 
from payment p 
group by payment_id 
order by amount desc ;

-- where sub query로 풀기 

SELECT c.customer_id,
       c.first_name,
       c.last_name
FROM   customer c
WHERE  customer_id IN (SELECT customer_id
                       FROM   payment p
                       GROUP  BY payment_id
                       ORDER  BY amount DESC
                       LIMIT  1)
-- 13	Karen	Jackson
-- left join으로도 풀 수 있지 않을까? 

select *
from customer c
right join (SELECT customer_id
                       FROM   payment p
                       GROUP  BY payment_id
                       ORDER  BY amount DESC
                       LIMIT  1) as db
        on db.customer_id = c.customer_id

-- 이렇게 풀 수 있다. 
```

### 대여가 한번도이라도 된 영화 카테 고리 이름을 알려주세요. 
(쿼리는, Exists조건을 이용하여 풀어봅시다)

```sql
SELECT *
FROM   rental r
WHERE  rental_date IS NULL;

-- 렌탈 데이트가 없는 것들을 조회해보니 없음

SELECT c.category_name 
FROM   categories c
WHERE  EXISTS (SELECT *
               FROM   rental r
                      JOIN inventory i
                        ON r.inventory_id = i.inventory_id
                      JOIN film f
                        ON i.film_id = f.film_id
                      JOIN film_category fc
                        ON fc.film_id = f.film_id);

-- 첨에 이렇게 쿼리를 짰는데 알고보니 이건 플랫폼기기 카테고리 였음 ,,,, 

SELECT c."name"
FROM   category c
WHERE  EXISTS (SELECT *
               FROM   rental r
                      JOIN inventory i
                        ON r.inventory_id = i.inventory_id
                      JOIN film f
                        ON i.film_id = f.film_id
                      JOIN film_category fc
                        ON fc.film_id = f.film_id)

-- 16 값이 나온다. 
```

### 대여가 한번도이라도 된 영화 카테 고리 이름을 알려주세요. 
(쿼리는, Any 조건을 이용하여 풀어봅시다)

```sql
-- any
SELECT c."name" 
FROM   category AS c
WHERE  category_id = any (SELECT fc.category_id 
                          FROM   rental AS r
                                 JOIN inventory AS i
                                   ON r.inventory_id = i.inventory_id
                                 JOIN film_category AS fc
                                   ON i.film_id = fc.film_id);
                                   
-- 다른 답
SELECT c."name" 
FROM   category AS c
WHERE  category_id >= any (select r.inventory_id 
                           FROM   rental AS r
                                 JOIN inventory AS i
                                   ON r.inventory_id = i.inventory_id
                                 JOIN film_category AS fc
                                   ON i.film_id = fc.film_id);
                                  
                                  
-- 아래 쿼리 실행하면 4580개가 나오는데 어차피 카테고리 안에 속해서 상관 없음
 
SELECT Count(DISTINCT r.inventory_id)
FROM   rental AS r
       JOIN inventory AS i
         ON r.inventory_id = i.inventory_id
       JOIN film_category AS fc
         ON i.film_id = fc.film_id    ;
```

### 대여가 가장 많이 진행된 카테고리는 무엇인가요? (Any, All 조건 중 하나를 사용하여 풀어봅시다)

```sql
-- 대여가 가장 많이 진행된 카테고리는 무엇인가요? 
-- (Any, All 조건 중 하나를 사용하여 풀어봅시다)

SELECT fc.category_id
FROM   rental AS r
       JOIN inventory AS i
         ON r.inventory_id = i.inventory_id
       JOIN film_category AS fc
         ON i.film_id = fc.film_id; 
        
-- 대여가 진행된 카테고리id
        
SELECT Count(DISTINCT r.rental_id) AS cnt,
       fc.category_id
FROM   rental AS r
       JOIN inventory AS i
         ON r.inventory_id = i.inventory_id
       JOIN film_category AS fc
         ON i.film_id = fc.film_id
GROUP  BY fc.category_id
ORDER  BY cnt desc;

-- rental_id count 해서 카테고리 아이디 별로 나온 값

SELECT c."name"
FROM   category c
WHERE  category_id = any (SELECT fc.category_id
                          FROM   rental AS r
                                 JOIN inventory AS i
                                   ON r.inventory_id = i.inventory_id
                                 JOIN film_category AS fc
                                   ON i.film_id = fc.film_id
                          GROUP  BY fc.category_id
                          ORDER  BY Count(DISTINCT r.rental_id) DESC)
LIMIT  1
-- Action

```

### dvd 대여를 제일 많이한 고객 이름은? (subquery 활용)

```sql
SELECT p.customer_id
                        FROM   payment p
                        GROUP  BY p.customer_id
                        ORDER  BY Count(p.rental_id) desc;

-- 아래가 정답 
                       
SELECT c.first_name,
       c.last_name
FROM   customer c
WHERE  c.customer_id = any (SELECT p.customer_id
                            FROM   payment p
                            GROUP  BY p.customer_id
                            ORDER  BY Count(p.rental_id) DESC)
LIMIT  1 ;

-- Eleanor	Hunt
```

### 영화 카테고리값이 존재하지 않는 영화가 있나요?

```sql
SELECT *
FROM   film AS f
WHERE  NOT EXISTS (SELECT *
                   FROM   film_category AS fc
                   WHERE  f.film_id = fc.film_id);

-- 없음 

SELECT *
FROM   film AS f
       JOIN film_category fc
         ON f.film_id = fc.film_id
WHERE  fc.category_id IS NULL

-- join 으로 풀기 
```

## 📌 참고사항

- IN, EXISTS 는 함수는 다르나 동일한 결과물을 내는 것이라고 생각해도 된다.
- 하지만 NOT IN, NOT EXISTS 는 NULL 에 의한 차이가 존재한다.

```sql
SELECT *
FROM   address a
WHERE  a.address2 IN (SELECT NULL);

SELECT *
FROM   address a
WHERE  address2 NOT IN (SELECT '');
```

- address2 != ''

```sql
SELECT *
FROM   address a
WHERE  NOT EXISTS (SELECT 1
                   FROM   (SELECT '' AS a) AS db
                   WHERE  db.a = a.address2);
```

- IN = EXISTS
- NOT IN != NOT EXISTS   
(전제조건은 null,  null 이 데이터셋에 들어있을때는 같지않음.
⛔️  단 NULL이 없으면 같다.)
- not in  + null   = not exists

### 무슨말이냐면 IN = EXISTS 는 맞고 NOT IN은 NULL값이 있으면 NOT EXISTS 랑 동일한 결과를 return하지 않는다 !