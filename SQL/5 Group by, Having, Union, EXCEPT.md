# 5. Group by, Having, Union, EXCEPT

# Group by

![Untitled](5%20Group%20by,%20Having,%20Union,%20EXCEPT%20b84fa3631dbc42d9a0108764ff11420d/Untitled.png)

- 테이블의 레코드를 `grouping` 하기 위해 사용
- 국가, 성별 같이 같은 값을 가진 카테고리별로 묶어서 조회
- aggregate functions <집계 함수> (COUNT(), MAX(), MIN(), SUM(), AVG())와 함께 자주 사용됩니다.

```sql
SELECT Column_name(s)
FROM   table_name
WHERE  CONDITION
GROUP  BY Column_name(s)
ORDER  BY Column_name(s);
```

```sql
SELECT Count(customerid),
       country
FROM   customers
GROUP  BY country
ORDER  BY Count(customerid) DESC; *-- count 기준으로 orderby 내림차순*
```

# Having

![SQL 순서](5%20Group%20by,%20Having,%20Union,%20EXCEPT%20b84fa3631dbc42d9a0108764ff11420d/Untitled%201.png)

SQL 순서

- WHERE 절이 aggregate function (sum같은) 과 같이 사용될 수 없어서 추가됨
- GROUP BY 하위에 조건을 걸 때 사용 , ORDER BY 위에
- Where 절 뒤에 Like 구문을 넣는것과 having 절에 Like 구문을 넣는것은 동일하다.

```sql
SELECT Count(customerid),
       country
FROM   customers
GROUP  BY country
HAVING Count(customerid) > 5
ORDER  BY Count(customerid) DESC;
```

# 집합연산자와 서브쿼리

![Untitled](5%20Group%20by,%20Having,%20Union,%20EXCEPT%20b84fa3631dbc42d9a0108764ff11420d/Untitled%202.png)

## UNION

- **중복을 제거**한 결과의 합
    - 중복데이터는 출력되지 않음
- 두 개 이상의 SELECT문과 결과값을 결합하는데 사용
- JOIN과 유사하지만 SELECT 문으로 만들어진 field가 **동일한 데이터 유형에 사용되어야 함**

![Untitled](5%20Group%20by,%20Having,%20Union,%20EXCEPT%20b84fa3631dbc42d9a0108764ff11420d/Untitled%203.png)

- UNION 내 SELECT문의 결과값은 같은 수의 열을 가지고 동일한 순서로 있어야 함

```sql
SELECT column_name(s) 
	FROM table1

UNION

SELECT column_name(s) 
	FROM table2;

```

```sql
SELECT city,
       country
FROM   customers
WHERE  country = 'Germany'

UNION

SELECT city,
       country
FROM   suppliers
WHERE  country = 'Germany'
ORDER  BY city;
```

## UNION ALL

- 중복을 포함한 결과의 합
    - UNION 은 중복값을 출력하지 않기 때문에 중복값까지 모두 출력할 때 사용

```sql
SELECT column_name(s) 
	FROM table1

UNION ALL

SELECT column_name(s) 
	FROM table2;
```

## INTERSECT

![Untitled](5%20Group%20by,%20Having,%20Union,%20EXCEPT%20b84fa3631dbc42d9a0108764ff11420d/Untitled%204.png)

- 양쪽 모두에서 포함된 행
- INTERSECT는 두 SELECT문을 결합하는 데 사용되지만 두 번째 SELECT문의 행과 동일한 첫 번째 SELECT 문의 행만 반환
    - 두 개의 SELECT 문에서 반환된 공통 행만 반환

```sql
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]

INTERSECT

SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]
```

```sql
SELECT id
FROM   a
INTERSECT
SELECT id
FROM   b;
```

## EXCEPT

- 굳이 안써도 됨
- = MINUS
    - 첫 번째 검색 결과에서 두 번째 검색 결과를 제외한 나머지

![Untitled](5%20Group%20by,%20Having,%20Union,%20EXCEPT%20b84fa3631dbc42d9a0108764ff11420d/Untitled%205.png)

- 보통 EXCEPT 집합연산자를 이용한 방법보다 두 개의 조건을 이용한 SELECT문이 훨씬 단순하고 가독성이 좋으며 성능 또한 우수
- 경우에 따라 EXCEPT 연산자를 이용하면 성능이 훨씬 좋을 때가 있음 (ex. 조건에 부정연산이 들어간 경우)

```sql
SELECT employee_id,
       last_name,
       job_id
FROM   employees
WHERE  last_name LIKE 'K%'
       AND job_id != 'SA_REP';
-- 이게 가독성이 더 좋고 성능이 더 우수함 
```

```sql
SELECT employee_id,
       last_name,
       job_id
FROM   employees
WHERE  last_name LIKE 'K%'

EXCEPT

SELECT employee_id,
       last_name,
       job_id
FROM   employees
WHERE  job_id = 'SA_REP';
```