# Week2 - 실습문제 1

### 고객의 기본 정보인, 고객 id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone 번호를 함께 보여주세요.

- 축약이 더 보기 좋다. ⇒ 근데 겹치면 ?

```sql
SELECT customer_id,
       first_name,
       last_name,
       email,
       customer.address_id,
       address.district,
       address.postal_code,
       address.phone

FROM   customer 
       JOIN address
         ON customer.address_id = address.address_id

---나의 답변

SELECT c.customer_id,
       c.first_name,
       c.last_name,
       c.email,
       c.address_id,
       a.district,
       a.postal_code,
       a.phone 
FROM   customer c
       INNER JOIN address a
         ON c.address_id = a.address_id

```

### 고객의  기본 정보인, 고객id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone , city 를 함께 알려주세요.

```sql
SELECT customer.customer_id,
       customer.first_name,
       customer.last_name,
       customer.email,
       customer.address_id,
       address.district,
       address.postal_code,
       address.phone,
       city.city
FROM   customer
       LEFT JOIN address
                ON customer.address_id = address.address_id
       LEFT JOIN city
                ON address.city_id = city.city_id
```

### Lima City에 사는 고객의 이름과, 성, 이메일, phonenumber에 대해서 알려주세요.

- INNER JOIN의 결과값이랑 Left JOIN의 결과값이랑 같다?
    - 아래처럼 결과값이 나오므로 결과가 같다
    
    ![Untitled](Week2%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%201%202a1d9100ae6e4b56b07c07cc8a7007ef/Untitled.png)
    

```sql
SELECT first_name,
       last_name,
       email,
       address.phone
FROM   customer
       LEFT JOIN address
         ON customer.address_id = address.address_id
       LEFT JOIN city
         ON address.city_id = city.city_id
WHERE  city.city = 'Lima'

---

SELECT first_name,
       last_name,
       email,
       address.phone,
       city.city
FROM   customer
       INNER JOIN address
         ON customer.address_id = address.address_id
       INNER JOIN city
         ON address.city_id = city.city_id
WHERE  city.city = 'Lima'
```

### Rental 정보에 추가로, 고객의 이름과, 직원의 이름을 함께 보여주세요.

- 고객의 이름, 직원 이름은 이름과 성을 fullname 컬럼으로만들어서 
직원이름/고객이름 2개의 컬럼으로 확인해주세요.

```sql
SELECT r.*,
       c.first_name,
       c.last_name
--       s.first_name,
--       s.last_name
FROM   rental AS r
       LEFT OUTER JOIN customer AS c
                    ON r.customer_id = c.customer_id
--       LEFT OUTER JOIN staff s
--                    ON r.staff_id = s.staff_id
                   limit 14;
```

위 쿼리의 결과

![Untitled](Week2%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%201%202a1d9100ae6e4b56b07c07cc8a7007ef/Untitled%201.png)

```sql
SELECT r.*,
       c.first_name,
       c.last_name,
       s.first_name,
       s.last_name
FROM   rental AS r
       LEFT OUTER JOIN customer AS c
                    ON r.customer_id = c.customer_id
       LEFT OUTER JOIN staff s
                    ON r.staff_id = s.staff_id
```

다른 쿼리의 결과

![Untitled](Week2%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%201%202a1d9100ae6e4b56b07c07cc8a7007ef/Untitled%202.png)

- 비교해보면 차이는 없음  ⇒ 그럼 INNER JOIN 써도 괜찮나 ?
    - 해보니까 써도 결과 똑같음
    - 그럼 섞어서 쓰면 ⇒ 똑같음

### ✅  [seth.hannon@sakilacustomer.org](mailto:seth.hannon@sakilacustomer.org) 이메일 주소를 가진 고객의  주소 address, address2, postal_code, phone, city 주소를 알려주세요.

```sql
SELECT address.address,
       address.address2,
       address.postal_code,
       address.phone,
       city.city
FROM   customer
       JOIN address
         ON customer.address_id = address.address_id
       JOIN city
         ON address.city_id = city.city_id
WHERE  customer.email = 'seth.hannon@sakilacustomer.org'
```

```sql
SELECT a.address,
       a.address2,
       a.postal_code,
       a.phone,
       c.city
FROM   address a 
       JOIN city c
         ON a.city_id = c.city_id
WHERE  customer.email = 'seth.hannon@sakilacustomer.org'
-- 실행 안됨 

-- 내가 작성한 정답 
SELECT a.address,
       a.address2,
       a.postal_code,
       a.phone,
       c.city
FROM  customer
			JOIN address a 
			ON customer.address_id = a.address_id
       JOIN city c
      ON a.city_id = c.city_id
WHERE  customer.email = 'seth.hannon@sakilacustomer.org'
```

- Where 절에 customer 정보가 필요하므로 customer 정보까지 join

### Jon Stephens 직원을 통해 dvd대여를 한 payment 기록 정보를  확인하려고 합니다.
- payment_id,  고객 이름 과 성,  rental_id, amount, staff 이름과 성을 알려주세요.

```sql
SELECT payment_id,
       customer.first_name,
       customer.last_name,
       payment.rental_id,
       payment.amount,
       staff.first_name,
       staff.last_name
FROM   payment
       JOIN customer
         ON payment.customer_id = customer.customer_id
       JOIN staff
         ON payment.staff_id = staff.staff_id
WHERE  staff.first_name = 'Jon'
       AND staff.last_name = 'Stephens'
```

### 배우가 출연하지 않는 영화의 film_id, title, release_year, rental_rate, length 를 알려주세요.

- film ⇒ film_id , title, release_year, rental_rate, length
- 배우가 출연하지 않는 영화 ⇒ actor 테이블 이용 ⇒ film_actor 와 actor

```sql
SELECT film.film_id,
       film.title,
       release_year,
       rental_rate,
       length
FROM   film
       LEFT JOIN film_actor
                    ON film.film_id = film_actor.film_id
       LEFT JOIN actor
                    ON film_actor.actor_id = actor.actor_id
WHERE  actor.actor_id IS NULL
```

### store 상점 id별 주소 (address, address2, distict) 와 해당 상점이 위치한 city 주소를 알려주세요.

```sql
SELECT store.store_id,
       address.address,
       address.address2,
       address.district,
       city.city
FROM   store
       LEFT OUTER JOIN address
                    ON store.address_id = address.address_id
       LEFT OUTER JOIN city
                    ON address.city_id = city.city_id
```

### 고객의 id 별로 고객의 이름 (first_name, last_name), 이메일, 고객의 주소 (address, district), phone번호, city, country 를 알려주세요.

- 고객의 이름 (first_name, last_name), 이메일
- 고객의 주소 (address, district), phone번호
- city
- country
    - 각 하나씩 테이블 다써야 하므로 4개 조인해야함
    - 고객의 id 별로 ⇒ customer 기준으로 고고

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       c.email,
       a.address,
       a.district,
       a.phone,
       ci.city,
       co.country
FROM   customer c
       JOIN address a
         ON c.address_id = a.address_id
       JOIN city ci
         ON a.city_id = ci.city_id
       JOIN country co
         ON ci.country_id = co.country_id;
```

![Untitled](Week2%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%201%202a1d9100ae6e4b56b07c07cc8a7007ef/Untitled%203.png)

### country 가 china 가 아닌 지역에 사는, 고객의 이름(first_name, last_name)과 , email, phonenumber, country, city 를 알려주세요

```sql
SELECT customer.first_name,
       customer.last_name,
       customer.email,
       address.phone,
       country.country,
       city.city
FROM   customer
       JOIN address
         ON customer.address_id = address.address_id
       JOIN city
         ON address.city_id = city.city_id
       JOIN country
         ON city.country_id = country.country_id
WHERE  country.country NOT IN ( 'China' )
```

### Horror 카테고리 장르에 해당하는 영화의 이름과 description 에 대해서 알려주세요

```sql
SELECT title,
       description
FROM   film f
       JOIN film_category fc
         ON f.film_id = fc.film_id
       JOIN category c
         ON fc.category_id = c.category_id
WHERE  c.name = 'Horror';
```

문제12번) Music 장르이면서, 영화길이가 60~180분 사이에 해당하는 영화의 title, description, length 를 알려주세요.

- 영화 길이가 짧은 순으로 정렬해서 알려주세요.

```sql
SELECT f.title,
       f.description,
       f.length
FROM   film f
       JOIN film_category fc
         ON f.film_id = fc.film_id
       JOIN category c
         ON fc.category_id = c.category_id
WHERE  c."name" = 'Music'
       AND f.length BETWEEN 60 AND 180
ORDER  BY f.length;
```

### 📌 actor 테이블을 이용하여,  배우의 ID, 이름, 성 컬럼에 추가로    'Angels Life' 영화에 나온 영화 배우 여부를 Y , N 으로 컬럼을 추가 표기해주세요.

- 해당 컬럼은 angelslife_flag로 만들어주세요.

```sql
SELECT a.actor_id,
       a.first_name,
       a.last_name,
       CASE
         WHEN a.actor_id IN (SELECT actor_id
                             FROM   film f
                                    INNER JOIN film_actor fa
                                            ON f.film_id = fa.film_id
                             WHERE  f.title = 'Angels Life') THEN 'Y'
         ELSE 'N'
       end AS angelslife_flag
FROM   actor a;
```

```sql
SELECT actor_id
FROM   film f
       INNER JOIN film_actor fa
               ON f.film_id = fa.film_id
WHERE  f.title = 'Angels Life'
```

- 우측과 같은 결과가 나오는데 'Angels Life'에 나온 ‘actor_id’를 나타낸다.

![Untitled](Week2%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%201%202a1d9100ae6e4b56b07c07cc8a7007ef/Untitled%204.png)

- 이 값을 만족시키는 부분을 찾으면 ⇒ 서브쿼리를 안에 넣어주면 됨 and 조건식 활용
- 다시 한번 손으로 적어볼것

### 대여일자가 2005-06-01~ 14일에 해당하는 주문 중에서 , 
직원의 이름(이름 성) = 'Mike Hillyer' 이거나  고객의 이름이 (이름 성) ='Gloria Cook'  에 해당 하는 rental 의 모든 정보를 알려주세요.

- 추가로 직원이름과, 고객이름에 대해서도 fullname 으로 구성해서 알려주세요.

```sql
SELECT r.*,
       s.first_name|| ' '|| s.last_name AS staff_fullname,
       c.first_name|| ' '|| c.last_name AS cust_fullname
FROM   rental r
       JOIN staff s
         ON r.staff_id = s.staff_id
       JOIN customer c
         ON r.customer_id = c.customer_id
WHERE  Date(rental_date) BETWEEN '2005-06-01' AND '2005-06-14'
       AND ( s.first_name|| ' '|| s.last_name = 'Mike Hillyer'
              OR c.first_name|| ' '|| c.last_name = 'Gloria Cook' );
```

### 대여일자가 2005-06-01~ 14일에 해당하는 주문 중에서 , 
직원의 이름(이름 성) = 'Mike Hillyer' 에 해당 하는 직원에게  구매하지 않은  
rental 의 모든 정보를 알려주세요.

- 추가로 직원이름과, 고객이름에 대해서도 fullname 으로 구성해서 알려주세요.

```sql
SELECT r.*,
       s.first_name|| ' '|| s.last_name AS staff_fullname,
       c.first_name|| ' '|| c.last_name AS cust_fullname
FROM   rental r
       JOIN staff s
         ON r.staff_id = s.staff_id
       JOIN customer c
         ON r.customer_id = c.customer_id
WHERE  Date(rental_date) BETWEEN '2005-06-01' AND '2005-06-14'
       AND s.first_name|| ' '|| s.last_name != 'Mike Hillyer';
```