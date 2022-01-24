# Week 1 - 실습문제 2

문제1번) film 테이블을 활용하여,  film 테이블의  100개의 row 만 확인해보세요.

- 100개의 Row 만 ⇒ limit = 파이썬에서 df.head(100)과 비슷함

```sql
SELECT *
FROM   film
LIMIT  100
```

문제2번) actor 의 성(last_name) 이  Jo 로 시작하는 사람의 id 값이 가장 낮은 사람 한사람에 대하여, 
사람의  id 값과  이름, 성 을 알려주세요.

- Jo 로 시작하는 ⇒

```sql
SELECT actor_id,
       first_name,
       last_name
FROM   actor
WHERE  last_name LIKE 'Jo%'

ORDER  BY actor_id
LIMIT  1
```

문제3번)
film 테이블을 이용하여, film 테이블의 아이디값이 1~10 사이에 있는 모든 컬럼을 확인해주세요.

```sql
SELECT *
FROM   film
WHERE  film_id BETWEEN 1 AND 10
```

문제4번) country 테이블을 이용하여, country 이름이 A 로 시작하는 country 를 확인해주세요.

```sql
SELECT *
FROM   country
WHERE  country LIKE 'A%'
```

### 📌  문제5번) country 테이블을 이용하여, country 이름이 s 로 끝나는 country 를 확인해주세요.

- **Lower ⇒ 소문자로 변경**

```sql
SELECT *
FROM   country
WHERE  **Lower**(country) LIKE '%s'
```

문제6번) address 테이블을 이용하여, 우편번호(postal_code) 값이 77로 시작하는  주소에 대하여,    address_id, address, district ,postal_code  컬럼을 확인해주세요.

```sql
select address_id, address, district ,postal_code
from address
where postal_code like '77%'
```

### 📌 문제7번) address 테이블을 이용하여, 우편번호(postal_code) 값이  두번째글자가 1인 우편번호의      address_id, address, district ,postal_code  컬럼을 확인해주세요.

- SUBSTRING(*string*, *start*, *length*)
    - SUBSTRING(mysqldastudy,-5,5) ⇒ study

```sql
select address_id, address, district ,postal_code
from address
where substring(postal_code,2,1) ='1'

--- 내가 작성한 답변 
select address_id, address, district ,postal_code
from address
where postal_code like '*1%'
```

문제8번) payment 테이블을 이용하여,  고객번호가 341에 해당 하는 사람이 결제를 2007년 2월 15~16일 사이에 한 모든 결제내역을 확인해주세요.

- 시간도 잘 넣어야함

```sql
SELECT *
FROM   payment
WHERE  customer_id = 341
       AND payment_date BETWEEN '2007-02-15 00:00:00' AND '2007-02-16 23:59:59'
```

문제9번) payment 테이블을 이용하여, 고객번호가 355에 해당 하는 사람의 결제 금액이 1~3원 사이에 해당하는 모든 결제 내역을 확인해주세요.

```sql
SELECT *
FROM   payment
WHERE  customer_id = 355
       AND amount BETWEEN 1 AND 3
```

문제10번) customer 테이블을 이용하여, 고객의 이름이 Maria, Lisa, Mike 에 해당하는 사람의 id, 이름, 성을 확인해주세요.

```sql
SELECT customer_id,
       first_name,
       last_name
FROM   customer
WHERE  first_name IN ( 'Maria', 'Lisa', 'Mike' )
```

문제11번) film 테이블을 이용하여,  film의 길이가  100~120 에 해당하거나 또는 rental 대여기간이 3~5일에 해당하는 film 의 모든 정보를 확인해주세요.

```sql
SELECT*
FROM   film
WHERE  ( length BETWEEN 100 AND 120 )
        OR ( rental_duration BETWEEN 3 AND 5 )
```