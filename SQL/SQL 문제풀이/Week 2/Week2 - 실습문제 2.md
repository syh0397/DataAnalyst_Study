# Week2 - 실습문제 2

### store 별로 staff는 몇명이 있는지 확인해주세요.

```sql
SELECT store_id,
       Count(*)
FROM   staff
GROUP  BY store_id;
```

### 영화등급(rating) 별로 몇개 영화 film을 가지고 있는지 확인해주세요.

```sql
select rating , count(*) cnt
from film
group by rating;

------------- 내가 작성한 답

SELECT rating,
       Count(title)
FROM   film
GROUP  BY rating;
```

### 출현한 영화배우(actor)가 10명 초과한 영화명은 무엇인가요?

```sql
SELECT f.title,
       fc.cnt
FROM   (SELECT fa.film_id,
               Count(*) cnt
        FROM   film_actor fa
        GROUP  BY fa.film_id
        HAVING Count(*) > 10) fc
       INNER JOIN film f
               ON f.film_id = fc.film_id;

---------------------------------------------------

select film.title
from (SELECT film_id,
       Count(*)
			FROM   film_actor
			GROUP  BY film_id
			HAVING Count(*) > 10)
Join film
	on film.film_id = film_actor.film_id -- film actor로 합치면 안됨 
-- 서브쿼리 사용했으면 사용해서 만든 테이블의 칼럼명이랑 같게 만들어 줘야함

----------------------------------------------------

SELECT film.title,
       film_count.cnt
FROM   (SELECT film_id,
               Count(*) AS cnt
        FROM   film_actor
        GROUP  BY film_id
        HAVING Count(*) > 10) AS film_count
       JOIN film
         ON film.film_id = film_count.film_id;
```