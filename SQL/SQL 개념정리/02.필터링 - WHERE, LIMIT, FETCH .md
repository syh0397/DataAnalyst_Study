# 2. 필터링 - WHERE, LIMIT, FETCH

# 4. 필터링 - WHERE

- 어떤 집합을 가져올지 정하는 부분 = 조건을 걸어서

```sql
SELECT
COLUMN_1 , COLUMN_2 , 중략...
FROM
TABLE_NAME
WHERE
<조건> ;

	-- 어떤 집합을 가져올지에 대한 조건을 준다.

```

> =	같음
> 

> ~보다 큰
<	~보다 작은
=	~보다 크거나 같은
<=	~보다 작거나 같은
<> , !=	~가 아닌 
AND	그리고
OR	혹은
> 

- 먼저 sql을 짤때 alignment 를 하면서 짜야한다.
    - Dbeaver 에서는 **`ctrl + shift + f`** 를 누르면 자동으로 정렬이 된다.
    - Tableplus 에서는 ??[http://www.dpriver.com/pp/sqlformat.htm](http://www.dpriver.com/pp/sqlformat.htm) 에서 바꾸자

---

## 실습

- 조건 한 개

```sql
SELECT last_name,
       first_name
FROM   customer
WHERE  first_name = 'Jamie';
-- FIRST_NAME이 ‘Jamie’ 인 행을 출력함
```

- 조건 두 개

```sql
SELECT last_name,
       first_name
FROM   customer
WHERE  first_name = 'Jamie'
       AND last_name = 'Rice';
-- FIRST_NAME이 ‘Jamie’ 이면서 LAST_NAME이 ‘Rice’ 인 행을 출력함
```

WHERE 절 실습

- 조건 두 개

```sql
SELECT customer_id ,
       amount ,
       payment_date
FROM   payment
WHERE  amount <= 1
OR     amount >= 8;

-- AMOUNT가 1이하 이거나 amount가 8이상인 행을 출력
```

# 5. LIMIT

- 한정하다 제한하다라는 뜻
    
    ⇒ 출력하는 행의 수를 한정하는 역할
    

```sql
SELECT *
FROM   table_name
LIMIT  n;
-- 출력갯수 제한
```

```sql
SELECT *
FROM  TABLE_NAME

LIMIT N OFFSET M ;

-- 출력하는 행의 수를 지정하면서 시작위치를 지정한다. 
-- OFFSET M값의 시작위치는 0이다
.
```

# 6.FETCH

얼마나 중요하면  행의 수를 한정하는가?!

- 네트워크 한테 올때까지 많은 부하
- 클라이언트 한테 올때까지 많은 부하

때문에 행의 수를 한정한다.

출력하는 행의 수를 지정한다. N을 입력하지 않고  ROW ONLY 만 입력하면 단 한건만 출력한다.

```sql
SELECT *
FROM   table_name
FETCH first [N] row only ;
```

- n대신 1을 쓰면 1건만 가져온다는 뜻

---

- 출력하는 행의 수를 지정하면서 시작위치를 지정한다.
- OFFSET N값의 시작위치는 0이다.

```sql
SELECT*
FROM TABLE_NAME
OFFSET N ROWS
       FETCH FIRST [N] ROW ONLY
 ;
```

예시 

```sql
select * --이건 주석다는 법입니다. :
from   
	film
order by title
offset 5 rows 
fetch first 10 row only 
;
```