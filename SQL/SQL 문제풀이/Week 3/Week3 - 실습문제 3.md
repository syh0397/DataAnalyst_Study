# Week 3 - 실습문제 3

### 국가(country)별 도시(city)별 매출액, 국가(country)매출액 소계 그리고 전체 매출액을 구하세요. (grouping set)

```sql
SELECT cty.country,
       ct.city,
       Sum(p.amount) as AMONT
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER JOIN country cty
               ON cty.country_id = ct.country_id
GROUP  BY grouping sets ( ( cty.country, ct.city ), 
									( cty.country ), 
											   ( ) )
ORDER  BY cty.country,
          ct.city;
```

### 국가(country)별 도시(city)별 매출액, 국가(country)매출액 소계 그리고 전체 매출액을 구하세요. (rollup)

```sql
SELECT cty.country,
       ct.city,
       Sum(p.amount) as AMONT
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER JOIN country cty
               ON cty.country_id = ct.country_id
GROUP  by rollup ( cty.country, ct.city )
ORDER  BY cty.country,
          ct.city;

--- 이거랑은 다름

SELECT cty.country,
       ct.city,
       Sum(p.amount) as AMONT
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER JOIN country cty
               ON cty.country_id = ct.country_id
GROUP  by rollup **(( cty.country, ct.city ))**
ORDER  BY cty.country,
          ct.city;

-- 뭐가 다르냐면 위에는 국가별 도시별 , 국가별 , 총계지만 
-- 아래는 국가별 도시별 총계 + 전체 총계만 나온다
```

### 영화배우별로  출연한 영화 count 수 와,   모든 배우의 전체 출연 영화 수를 합산 해서 함께 보여주세요.

```sql
SELECT fa.actor_id ,
       Count(fa.film_id)
FROM   film_actor fa
GROUP  BY Rollup(actor_id)
order by fa.actor_id desc
```

### 국가 (Country)별, 도시(City)별  고객의 수와 ,  
전체 국가별 고객의 수를 함께 보여주세요. (grouping sets)

```sql
SELECT cty.country,
       ct.city,
       Count(DISTINCT customer_id) AS cnt
FROM   customer c
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER JOIN country cty
               ON cty.country_id = ct.country_id
GROUP  BY grouping sets ( ( cty.country, ct.city ))
ORDER  BY cty.country,
          ct.city;
```

### 영화에서 사용한 언어와  영화 개봉 연도 에 대한 영화  갯수와  , 영화 개봉 연도에 대한 영화 갯수를 함께 보여주세요.

```sql
SELECT language_id,
       release_year,
       Count(film_id)
FROM   film
GROUP  BY Rollup (language_id, release_year)
;

-- language_id 가 1개 뿐 
```

### 연도별, 일별 결제  수량과, 연도별 결제 수량을 함께 보여주세요.

- 결제수량은  결제 의 id 갯수 를 의미합니다.

```sql
SELECT Substring(Cast(payment_date AS VARCHAR), 1, 4)  AS year,
       Substring(Cast(payment_date AS VARCHAR), 1, 10) AS date,
       Count(payment_id)                               AS cnt
FROM   payment p
GROUP  BY grouping sets( ( Substring(Cast(payment_date AS VARCHAR), 1, 4),
                           Substring(Cast(payment_date AS VARCHAR), 1, 10) ), 
														Substring(Cast(payment_date AS VARCHAR), 1, 4) );

-- To_char 를 활용한 정답 

SELECT To_char(p.payment_date, 'YYYY')       AS year,
       To_char(p.payment_date, 'yyyy"년" mm"월" dd"일"') AS day,
       Count(p.payment_id)
FROM   payment p
GROUP  BY Rollup (( year ), ( day ))
ORDER  BY day;
```

```sql
Substring(Cast(payment_date AS VARCHAR),1,10 -- 
To_char(p.payment_date, 'YYYY-mm-dd')
```

Substring(Cast(payment_date AS VARCHAR),1,10 에서 10은 뭔가? 문자 ‘ - ‘ 까지 해서 10 

### 지점 별,  active 고객의 수와 ,   active 고객 수 를  함께 보여주세요.

지점과, active 여부에 대해서는 customer 테이블을 이용하여 보여주세요.

-  grouping sets 를 이용해서 풀이해주세요.

```sql
-- 지점 별,  active 고객의 수와 ,   active 고객 수 를  함께 보여주세요.
-- grouping sets 를 이용해서 풀이해주세요.

select c.store_id,
		count(c.active) as cnt
from customer c 
group by grouping sets (c.store_id,()) 

Null 599
	1	326
	2	273

-- 이거 아닌가 
-- 답은 이거라는데 문제가 active '별'까지 추가 되어야 할듯

SELECT store_id,
       active,
       Count(customer_id) AS cnt
FROM   customer c
GROUP  BY grouping sets( ( store_id, active ), ( active ) );
```

### 지점 별,  active 고객의 수와 ,   active 고객 수 를  함께 보여주세요.지점과, active 여부에 대해서는 customer 테이블을 이용하여 보여주세요. roll up으로 풀이해보면서,  grouping sets 과의 차이를 확인해보세요.

```sql
-- rollup으로 풀이 

SELECT store_id,
       active,
       Count(customer_id) AS cnt
FROM   customer c
GROUP  by rollup (  store_id, active); 
 
-- 차이는 ( active ) 굳이 명시하지 않아도 알아서 active 까지 넣은 칼럼이랑 똑같은 결과를 보여준다 
```

- 또한 `rollup (  store_id, active);`  여기에서 `rollup (( store_id, active ));` 
 이렇게 괄호를 두번치면  active 컬럼만 가지고 count 진행한 부분이 나오지 않는다.