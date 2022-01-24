# Week3 - 실습문제 6

## 나라별로 가장 대여를 많이한 고객 TOP 5를 구하세요.

```sql
SELECT *
FROM   (SELECT cty.country,
               c.first_name,
               c.last_name,
               d.customer_id,
               Count(d.rental_id)  as  cnt_rent,
							row_number()
                 OVER (
                   partition BY cty.country
                   ORDER BY Count(d.rental_id) DESC ) rnum,
               Rank()
                 OVER (
                   partition BY cty.country
                   ORDER BY Count(d.rental_id) DESC ) rank
        FROM   rental d
               INNER JOIN customer c
                       ON d.customer_id = c.customer_id
               INNER JOIN address a
                       ON a.address_id = c.address_id
               INNER JOIN city ct
                       ON ct.city_id = a.city_id
               INNER JOIN country cty
                       ON cty.country_id = ct.country_id
        GROUP  BY cty.country,
                  c.first_name,
                  c.last_name,
                  d.customer_id) p
WHERE  rnum <= 5;
```

```sql
-- 나라별로 가장 대여를 많이한 고객 TOP 5를 구하세요.
select *
from (
SELECT c.customer_id,
	  cty.country,
	  count(r.rental_id) as cnt_rent,
		  rank () over(partition by cty.country 
						order by count(r.rental_id)) as rank,
		  dense_rank () over(partition by cty.country 
					order by count(r.rental_id)) as d_rank
FROM   rental r
       JOIN customer c
         ON r.customer_id = c.customer_id
       JOIN address a
         ON a.address_id = c.address_id
       JOIN city ct
         ON ct.city_id = a.city_id
       JOIN country cty
         ON cty.country_id = ct.country_id 
group by cty.country, c.customer_id ) as tbl
where tbl.rank <= 5
```

## 영화 카테고리 (Category) 별로 대여가 가장 많이 된 영화 TOP 5를 구하세요

```sql
SELECT *
FROM   (SELECT c.NAME,
               f.title,
               Count(d.rental_id)     as  rental_cnt,
               Row_number()
                 OVER (
                   partition BY c.NAME
                   ORDER BY Count(d.rental_id) DESC ) rnum
        FROM   rental d
                JOIN inventory i
                       ON d.inventory_id = i.inventory_id
                JOIN film f
                       ON f.film_id = i.film_id
                JOIN film_category fc
                       ON fc.film_id = f.film_id
                JOIN category c
                       ON c.category_id = fc.category_id
        GROUP  BY c.NAME,
                  f.title) p
WHERE  rnum <= 5;
```

## 매출이 가장 많은 영화 카테고리와 매출이 가장 작은 영화 카테고리를 구하세요. (first_value, last_value)

```sql
SELECT *
FROM   (SELECT c.name                       AS category_name,
               SUM(p.amount)                rental_aoumnt,
               First_value(c.name)
                 over (
                   ORDER BY SUM(p.amount) ) min_rental_amount_catagory,
               Last_value(c.name)
                 over (
                   ORDER BY SUM(p.amount) RANGE BETWEEN unbounded preceding AND
                 unbounded
                 following )                max_rental_amount_catagory
        FROM   rental r
               inner join payment p
                       ON r.rental_id = p.rental_id
               inner join inventory i
                       ON i.inventory_id = r.inventory_id
               inner join film_category fc
                       ON fc.film_id = i.film_id
               inner join category c
                       ON c.category_id = fc.category_id
        GROUP  BY c.name) c
WHERE  c.category_name IN ( min_rental_amount_catagory,
                            max_rental_amount_catagory );
```

```sql
select c."name" ,
		sum(p.amount) as amt,
		first_value (c.name) over ( 
								order by sum(p.amount) ) as f_val,
		last_value (c.name) over ( 
								order by sum(p.amount) ) as l_val
from rental  r
       inner join payment p on r.rental_id = p.rental_id
       inner join inventory i on i.inventory_id = r.inventory_id
       inner join film_category fc on fc.film_id = i.film_id
       inner join category      c  on c.category_id = fc.category_id
group by  c."name" ;

-- 왜 이건 안될까 ? RANGE BETWEEN unbounded preceding ANd unbounded following 이게 뭘까
-- 일단 

select c."name" ,
		sum(p.amount) as amt,
		first_value (sum(p.amount)) over () as f_val,
		last_value (sum(p.amount)) over ( ) as l_val
from rental  r
       inner join payment p on r.rental_id = p.rental_id
       inner join inventory i on i.inventory_id = r.inventory_id
       inner join film_category fc on fc.film_id = i.film_id
       inner join category      c  on c.category_id = fc.category_id
group by  c."name" ;      

-- 위와 같이 윈도우 함수의 인자값에 합계를 넣어주면 큰 값 작은값 두개가 나온다 3830.15	4336.01  

-- 정답은

select c."name" ,
		sum(p.amount) as amt,
		first_value (c.name) over ( 
								order by sum(p.amount) ) as f_val,
		last_value (c.name) over ( 
								order by sum(p.amount) ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING ) as l_val
from rental  r
       inner join payment p on r.rental_id = p.rental_id
       inner join inventory i on i.inventory_id = r.inventory_id
       inner join film_category fc on fc.film_id = i.film_id
       inner join category      c  on c.category_id = fc.category_id
group by  c."name" ;

-- 여기다가 

select tbl.f_val,
		tbl.l_val
from
(select c."name" ,
		sum(p.amount) as amt,
		first_value (c.name) over ( 
								order by sum(p.amount) ) as f_val,
		last_value (c.name) over ( 
								order by sum(p.amount) ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING ) as l_val
from rental  r
       inner join payment p on r.rental_id = p.rental_id
       inner join inventory i on i.inventory_id = r.inventory_id
       inner join film_category fc on fc.film_id = i.film_id
       inner join category      c  on c.category_id = fc.category_id
group by  c."name" ) as tbl
limit  1;

-- 이렇게 하면 깔끔
```

- O**VER(PARTITION BY 컬럼명)**을 사용할 경우 해당 컬럼의 그룹별로 첫번째 행의 값을 표시 한다.
- LAST_VALUE 함수에서 ORDER BY를 사용할 경우 WINDOWING 절에 해당 조건을 추가해야 한다
    - **"ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING"**
        - 무슨 뜻?
            
            **`UNBOUNDED PRECEDING`**
            
            : PARTITION의 첫 번째 로우에서 윈도우가 시작한다
            
            **`UNBOUNDED FOLLOWING`**
            
            : PARTITION의 마지막 로우에서 윈도우가 시작한다
            
        - 무슨 뜻?
            
            **`FIRST_VALUE(컬럼명 [RESPECT|IGNORE NULLS]) OVER()`**
            
            NULL 값을 제외한 가장 첫번째 행의 값을 표시한다.