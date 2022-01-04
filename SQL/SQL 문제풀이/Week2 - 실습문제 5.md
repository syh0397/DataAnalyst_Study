# Week2 - 실습문제 5

### 영화 배우가,  영화 180분 이상의 길이 의 영화에 출연하거나, 영화의 rating 이 R 인 등급에 해당하는 영화에 출연한  영화 배우에 대해서,  영화 배우 ID 와 (180분이상 / R등급영화)에 대한 Flag 컬럼을 알려주세요.

![Untitled](Week2%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%205%203a2e30c491c34807b4c4232e878eabed/Untitled.png)

- film_actor 테이블과 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
- actor_id 가 동일한 flag 값 이 여러개 나오지 않도록 해주세요
    - 중복 X ⇒ Union

```sql
SELECT actor_id,
       'length_over_180' as flag
FROM   film_actor fa
WHERE  film_id IN (select film_id 
                   FROM   film
                   WHERE  length >= 180)
UNION -- 여기 밑에 select 문을 띄우게 되면 rating_R 컬럼만 나온다.
SELECT actor_id,
       'rating_R' AS flag
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  rating = 'R')
```

```sql

-- 나의 정답 => 실행이 안됨

SELECT fa.actor_id,
       length_over_180.length
FROM   (SELECT film_id,
               length
        FROM   film
        WHERE  length >= 180) AS length_over_180
       JOIN film_actor fa
         ON fa.film_id = length_over_180.film_id

UNION 

SELECT fa.actor_id,
       rating_R.rating
FROM   (SELECT film_id,
               rating
        FROM   film
        WHERE  rating = 'R') AS rating_R
       JOIN film_actor fa
         ON fa.film_id = rating_R.film_id
```

```sql
-- 이건 실행은 되는데 actor_id밖에 안뜸

(SELECT fa.actor_id
FROM   (SELECT film_id,
               length
        FROM   film
        WHERE  length >= 180) AS length_over_180
       JOIN film_actor fa
         ON fa.film_id = length_over_180.film_id)
union
(SELECT fa.actor_id
FROM   (SELECT film_id,
               rating
        FROM   film
        WHERE  rating = 'R') AS rating_R
       JOIN film_actor fa
         ON fa.film_id= rating_R.film_id)

```

📌 **Union을 사용할 때 주의할 점**.

- 대응하는 필드의 이름이 같아야 한다. 같지 않다면 AS 를 사용하여 같게 만든다.
- 대응되는 각 필드의 타입이 같아야 한다.

```sql
나의 경우 문제점은 대응하는 필드의 이름이 달랐다는 점 
```

### R등급의 영화에 출연했던 배우이면서, 동시에, Alone Trip의 영화에 출연한  영화배우의 ID 를 확인해주세요.

- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
    - intersect ⇒ 교집합

```sql
SELECT actor_id
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  rating = 'R')
INTERSECT
SELECT actor_id
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film f
                   WHERE  title = 'Alone Trip')
```

문제3번) G 등급에 해당하는 필름을 찍었으나,   영화를 20편이상 찍지 않은 영화배우의 ID 를 확인해주세요.

- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.

```sql
SELECT actor_id
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  rating = 'G')
EXCEPT

SELECT actor_id
FROM   film_actor fa
GROUP  BY actor_id
HAVING Count(DISTINCT film_id) >= 20
```

![g등급의 actor_id 및 film_id](Week2%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%205%203a2e30c491c34807b4c4232e878eabed/Untitled%201.png)

g등급의 actor_id 및 film_id

![count film_id 포함 ](Week2%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%205%203a2e30c491c34807b4c4232e878eabed/Untitled%202.png)

count film_id 포함 

```sql
SELECT actor_id, Count( film_id)
FROM film_actor
GROUP BY actor_id
HAVING Count( film_id) >= 20
order by actor_id
```

![Untitled](Week2%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%205%203a2e30c491c34807b4c4232e878eabed/Untitled%203.png)

```sql
SELECT actor_id, count(distinct film_id) as cnt
FROM film_actor fa
GROUP BY actor_id
order by cnt
```

<aside>
❣️ 굳이 DISTINCT를 쓰는이유 ⇒ 그냥 중복 방지를 위해서인듯 (습관화)

</aside>

### 필름 중에서,  필름 카테고리가 Action, Animation, Horror 에 해당하지 않는 필름 아이디를 알려주세요.

- category 테이블을 이용해서 알려주세요.

```sql
SELECT film_id
FROM   film
EXCEPT
SELECT film_id
FROM   film_category fc
WHERE  category_id IN (SELECT category_id
                       FROM   category c
                       WHERE  NAME IN ( 'Action', 'Animation', 'Horror' ))
```

```sql
SELECT film_id,
       category_id
FROM   film_category fc
WHERE  category_id IN (SELECT category_id
                       FROM   category c
                       WHERE  NAME IN ( 'Action', 'Animation', 'Horror' ));

---
| film_id  | category_id |
|----------|-------------|
| 2        | 11          |
| 4        | 11          |
| 8        | 11          |
| 9        | 11          |
| 13       | 11          |
| 18       | 2           |
| 19       | 1           |
```

```sql
서브쿼리에도 많은 컬럼이 들어가면 실행이 안된다. (오류에 너무 많은 행이 들어가 있다고 뜬다.)
```

### Staff의id , 이름, 성 에 대한 데이터와 , Customer 의 id, 이름 , 성에 대한 데이터를  하나의  데이터셋의 형태로 보여주세요.

- 컬럼 구성 : id, 이름 , 성, flag (직원/고객여부) 로 구성해주세요.

```sql
SELECT staff_id,
       first_name,
       last_name,
       'Staff' AS flag
FROM   staff
UNION ALL
SELECT customer_id,
       first_name,
       last_name,
       'Customer' AS flag
FROM   customer
```

### 직원과  고객의 이름이 동일한 사람이 혹시 있나요? 있다면, 해당 사람의 이름과 성을 알려주세요.

```sql
SELECT first_name,
       last_name
FROM   customer
INTERSECT
SELECT first_name,
       last_name
FROM   staff
```

```sql
SELECT first_name
FROM   staff
where first_name in (select first_name
					 from customer)

-- first_name 즉 이름이 겹치는 사람이 있는지 찾아보면 

| first_name |   |
|------------|---|
| Mike       |   |
| Jon        |   |
|            |   |
```

```sql
staff 테이블에는 (Mike	Hillyer)
customer 테이블에는 (Mike	Way)

-- 교훈 : 의심하지 말자\ 아니 의심해보자
```