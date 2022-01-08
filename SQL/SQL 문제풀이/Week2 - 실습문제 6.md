# Week2 - 실습문제 6

### 반납이 되지 않은 대여점(store)별 영화 재고 (inventory)와 전체 영화 재고를 같이 구하세요. (union all)

```sql
SELECT null as null,
       Count(*) cnt
FROM   rental r
       INNER JOIN inventory i
               ON r.inventory_id = i.inventory_id
WHERE  return_date IS NULL
UNION ALL
SELECT i.store_id,
       Count(*) cnt
FROM   rental r
       INNER JOIN inventory i
               ON r.inventory_id = i.inventory_id
WHERE  return_date IS NULL
GROUP  BY i.store_id
```

- 하나씩 나누어 구함

```sql
-- 반납이 되지 않은 대여점(store)별 영화 재고

SELECT Count(i.inventory_id) AS cnt,
       i.store_id
FROM   inventory i
       JOIN rental r
         ON r.inventory_id = i.inventory_id
WHERE  r.return_date IS NULL
GROUP  BY i.store_id

| cnt | store_id |
|-----|----------|
| 92  | 1        |
| 91  | 2        |
|     |          |
|     |          |

```

```sql
-- 전체영화 재고

SELECT NULL                  AS store_id,
       Count(i.inventory_id) AS cnt
FROM   rental r
       JOIN inventory i
         ON i.inventory_id = r.inventory_id
WHERE  return_date IS NULL

| Store_id | cnt |
|----------|-----|
| NULL     | 183 |
|          |     |
```

```sql
SELECT Count(i.inventory_id) AS cnt,
       i.store_id
FROM   inventory i
       JOIN rental r
         ON r.inventory_id = i.inventory_id
WHERE  r.return_date IS NULL
GROUP  BY i.store_id
union all
SELECT NULL                  AS store_id,
       Count(i.inventory_id) AS cnt
FROM   rental r
       JOIN inventory i
         ON i.inventory_id = r.inventory_id
WHERE  return_date IS NULL

| cnt | id  |
|-----|-----|
| 92  | 1   |
| 91  | 2   |
|     | 183 |
|     |     |
|     |     |
|     |     |
```

### 국가(country)별 도시(city)별 매출액, 국가(country)매출액 소계 그리고 전체 매출액을 구하세요. (union all)

```sql
SELECT cty.country,
       ct.city,
       Sum(p.amount) rental_amount
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER JOIN country cty
               ON cty.country_id = ct.country_id
GROUP  BY cty.country,
          ct.city
UNION ALL
SELECT cty.country,
       NULL,
       Sum(p.amount) rental_amount
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER JOIN country cty
               ON cty.country_id = ct.country_id
GROUP  BY cty.country
UNION ALL
SELECT NULL,
       NULL,
       Sum(p.amount) rental_amount
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER JOIN country cty
               ON cty.country_id = ct.country_id
ORDER  BY 1,
          2;
```

```sql
-- 국가(country)별 도시(city)별 매출액

SELECT ctr.country,
       ct.city,
       Sum(p.amount) sum_amount
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER join country ctr
               ON ctr.country_id = ct.country_id
GROUP  BY ctr.country,
          ct.city
---

| country       | city                       | sum_amount |
|---------------|----------------------------|------------|
| Mexico        | San Juan Bautista Tuxtepec | 98.75      |
| Netherlands   | Ede                        | 103.78     |
| Germany       | Halle/Saale                | 100.75     |
| Malaysia      | Ipoh                       | 104.75     |
| Philippines   | Bislig                     | 71.81      |
| India         | Bhimavaram                 | 115.72     |
| United States | Bellevue                   | 91.80      |
|     ...       |       ...                  | ...        |
```

```sql
-- 국가(country)매출액 소계

SELECT ctr.country,
	     null as city, -- 위에 컬럼이랑 컬럼명을 맞춰주려면 이렇게 구한다
       Sum(p.amount) sum_amount
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER join country ctr
               ON ctr.country_id = ct.country_id
GROUP  BY ctr.country

---

| country              | city | sum_amount |
|----------------------|------|------------|
| Thailand             |      | 401.08     |
| Virgin Islands, U.S. |      | 121.69     |
| Faroe Islands        |      | 96.76      |
| Bangladesh           |      | 353.19     |
| Indonesia            |      | 1352.69    |
| Italy                |      | 753.26     |
| Venezuela            |      | 632.43     |
| Oman                 |      | 161.56     |
```

```sql
-- 전체 매출액
select sum(p.amount) sum_amount
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER join country ctr
               ON ctr.country_id = ct.country_id

-- 61312.04
```

- 하지만 이대로 다 합치면 ⇒ each UNION query must have the same number of columns
- 컬럼의 수가 다 같지 않아서 ㅎㅎㅎ 안됨
- 고로 마지막 테이블에 컬럼 추가  ⇒ 근데 안됨 왜 ?
    - 컬럼 순서가 달라서 ,,,,,
    - null 을 뒤에 적었더니 안되고, 앞에 적으니까 됨

```sql
-- 국가(country)별 도시(city)별 매출액

SELECT ctr.country,
       ct.city,
       Sum(p.amount) as sum_amount
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER join country ctr
               ON ctr.country_id = ct.country_id
GROUP  BY ctr.country,
          ct.city
union all
SELECT ctr.country,
	     null as city, -- 위에 컬럼이랑 컬럼명을 맞춰주려면 이렇게 구한다
       Sum(p.amount) as sum_amount
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER join country ctr
               ON ctr.country_id = ct.country_id
GROUP  BY ctr.country
union all
SELECT null as country,
		null as city,
		sum(p.amount) as sum_amount
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON a.address_id = c.address_id
       INNER JOIN city ct
               ON ct.city_id = a.city_id
       INNER join country ctr
               ON ctr.country_id = ct.country_id
order by sum_amount Desc
```