# Week 3 - 실습문제 2

### 대여점(store)별 영화 재고(inventory) 수량과 전체 영화 재고 수량은? (grouping set)

```sql
-- 대여점(store)별 영화 재고(inventory) 수량과 전체 영화 재고 수량은? (grouping set)

SELECT i.store_id,
       Count(i.inventory_id) as cnt
FROM   inventory i
GROUP  BY grouping sets( ( store_id ), 
							( ))
order by cnt;
```

### 대여점(store)별 영화 재고(inventory) 수량과 전체 영화 재고 수량은? (roll up)

```sql
SELECT i.store_id,
       Count(i.inventory_id) as cnt
FROM   inventory i
GROUP  BY rollup ( ( store_id ))
order by cnt;

-- 위의 코드에서 rollup만 바꾸면 실행이 안됨 

SELECT i.store_id,
       Count(i.inventory_id) as cnt
FROM   inventory i
GROUP  BY **rollup**( ( store_id ), 
							( ))
order by cnt;

-- 왜냐면 이렇게 스토어 아이디 2개로 롤업해보면 
-- 스토어 아이디별 총 계수가 두번 나온다.

SELECT i.store_id,
       Count(i.inventory_id) as cnt
FROM   inventory i
GROUP  BY rollup ( ( store_id ), 
							(i.store_id) )
order by cnt;

```

```sql
-- 다른 예시로 

SELECT i.store_id,i.film_id ,
       Count(i.inventory_id) as cnt
FROM   inventory i
GROUP  BY rollup ( 
					( store_id ),
					(i.film_id)
							)
order by film_id desc ;

-- 이렇게 짜면 store_id, film_id, 전체 다 나옴 

```