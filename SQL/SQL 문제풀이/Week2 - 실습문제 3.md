# Week2 - 실습문제 3

### 영화 배우(actor)들이 출연한 영화는 각각 몇 편인가요?

- 영화 배우의 이름 , 성 과 함께 출연 영화 수를 알려주세요.

```sql
SELECT a.first_name,
       a.last_name,
       fa.cnt
FROM   (SELECT fa.actor_id,
               Count(*)
        FROM   film_actor fa
        GROUP  BY fa.actor_id) fa
       INNER JOIN actor a
               ON fa.actor_id = a.actor_id;

---

-- 먼저 아래와 같이 만들고 
SELECT film_actor.actor_id,
       Count(*)
FROM   film_actor
GROUP  BY fa.actor_id

-- 이걸 서브쿼리로 써서
SELECT a.first_name,
			 a.last_name,
			 count_act.cnt
FROM (SELECT fa.actor_id as actor_id,
               Count(*) as cnt
        FROM   film_actor fa
        GROUP  BY fa.actor_id) as count_act
			JOIN actor a
			ON a.actor_id = count_act.actor_id	

-- 그냥 조인 써도 됨
-- Mysql 이나 postgresql이나 같음 
```

### 국가(country)별 고객(customer) 는 몇명인가요?

- country 까지 가려면 address 와 city를 지나가야 하는데 ,,, 국가가 없는것은 카운팅하지 않는다는 개념에서 inner join을 사용한다 ⇒

```sql
SELECT country.country,
       Count(*) cnt
FROM   customer c
       INNER JOIN address a
               ON c.address_id = a.address_id
       INNER JOIN city
               ON city.city_id = a.city_id
       INNER JOIN country
               ON city.country_id = city.country_id
GROUP  BY country.country
ORDER  BY cnt DESC

-- order by 해도 안해도 결과값이 같다
```

### 영화 재고 (inventory) 수량이 3개 이상인 영화(film) 는?

- store는 상관 없이 확인해주세요.

```sql
select  f.title , i.cnt
from
( select  film_id ,  count(*) cnt
from inventory i
group by  film_id
having count(*) > 3
) i
inner join film f on f.film_id = i.film_id;

-- 3개 이상인것 탐색

SELECT film_id,
       Count(inventory_id) as cnt
FROM   inventory
GROUP  BY film_id
HAVING Count(inventory_id) >= 3

-- 나의 정답

SELECT film.title,
       cnt_filmid.cnt
FROM   (SELECT film_id,
               Count(inventory_id) AS cnt
        FROM   inventory
        GROUP  BY film_id
        HAVING Count(inventory_id) >= 3) AS cnt_filmid
       INNER JOIN film
               ON film.film_id = cnt_filmid.film_id
```