# Week3 - 실습문제 7

## dvd 대여를 제일 많이한 고객 이름은?   (with 문 활용)

```sql
-- 정답은 이건데
 
WITH rental_customer_rank
     AS (SELECT r.customer_id,
                Count(*) rental_cnt
         FROM   rental r
         GROUP  BY r.customer_id) -- rental 테이블에서
SELECT Concat(c.first_name, ' ', c.last_name) customer_name,
       r.rental_cnt
FROM   customer c
       INNER JOIN (SELECT r.customer_id,
                          rental_cnt,
                          Row_number()
                            OVER (
                              ORDER BY rental_cnt DESC ) rnum
                   FROM   rental_customer_rank r) r
               ON c.customer_id = r.customer_id
WHERE  r.rnum = 1;
```

```sql
-- dvd 대여를 제일 많이한 고객 이름은?  

select c.first_name,
		c.last_name,
		c.customer_id,
		count(r.rental_id) as cnt
from customer c 
join rental r 
on r.customer_id = c.customer_id 
group by c.first_name,c.last_name ,c.customer_id 
order by cnt desc 
limit 1; 

--  (with 문 활용)

SELECT r.customer_id,
       Count(r.rental_id) AS cnt
FROM   rental r
GROUP  BY r.customer_id; 
        
-- 서브쿼리 처럼 작성하기 
-- 내가 작성한 정답

with rank_dvd_rent
	as (SELECT r.customer_id,
       			Count(r.rental_id) AS cnt
		FROM   rental r
		GROUP  BY r.customer_id)
select c.first_name,
		c.last_name,
		c.customer_id,
		rnk.cnt
from customer c 
	join rank_dvd_rent rnk 
		on rnk.customer_id = c.customer_id
order by cnt desc
```

## 📌 영화 시간 유형 (length_type)에 대한 영화 수를 구하세요.
영화 상영 시간 유형의 정의는 다음과 같습니다.

- 영화 길이 (length) 은 60분 이하 short , 61분 이상 120분 이하 middle , 121 분이상 long 으로 한다.

```sql
WITH category_flag
     AS (SELECT 0       AS chk1,
                60      AS chk2,
                'short' AS length_flag
         UNION ALL
         SELECT 61       AS chk1,
                120      AS chk2,
                'middle' AS length_flag
         UNION ALL
         SELECT 121    AS chk1,
                999    AS chk2,
                'long' AS length_flag)
select f.film_id,
		f.length,
		cf.length_flag,
		concat(cf.chk1, '부터 ' , cf.chk2) 
from film f 
join category_flag as cf 
	on f.length between cf.chk1 and cf.chk2;
	
-- 이건 좀 어렵다
-- 테이블 을 만드는 것 까지는 이해했는데
-- on 다음에 between 이 오는게 넘 어렵다
```

## 약어로 표현되어 있는 영화등급(rating) 을 영문명, 한글명과 같이 표현해 주세요. (with 문 활용)

G        ? General Audiences (모든 연령대 시청가능)
PG      ? Parental Guidance Suggested. (모든 연령대 시청가능하나, 부모의 지도가 필요)
PG-13 ? Parents Strongly Cautioned (13세 미만의 아동에게 부적절 할 수 있으며, 부모의 주의를 요함)
R         ? Restricted. (17세 또는 그이상의 성인)
NC-17 ? No One 17 and Under Admitted.  (17세 이하 시청 불가)

```sql
WITH tbl
     AS (SELECT 'G'                 AS rating,
                'General Audiences' AS text
         UNION ALL
         SELECT 'PG'                          AS rating,
                'Parental Guidance Suggested' AS text
         UNION ALL
         SELECT 'PG-13'                      AS rating,
                'Parents Strongly Cautioned' AS text
         UNION ALL
         SELECT 'R'          AS rating,
                'Restricted' AS text
         UNION ALL
         SELECT 'NC-17'                        AS rating,
                'No One 17 and Under Admitted' AS text)
SELECT f.film_id,
       f.title,
       f.rating,
       t.text
FROM   film AS f
       LEFT JOIN tbl AS t
                    ON Cast(f.rating AS TEXT) = t.rating; -- text로 형변환
```

- -테이블(MY_TALBE)에서 가격(PRICE)칼럼을 INT에서 VARCHAR로 형변환
    
    ```sql
    SELECT CAST(PRICEAS AS VARCHAR) AS 가격 FROM MY_TABLE
    ```