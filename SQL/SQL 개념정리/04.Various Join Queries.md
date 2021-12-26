# 4. Various Join Queries

# SQL Joins

### 2개 이상의 테이블을 1개의 테이블로 생각하고 join하여 쿼리를 날리는 것

⇒ 두개이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법

1. 보통 Primary key혹은 Foreign key로 두 테이블을 연결
2. 테이블을 연결하려면 **적어도 하나의 칼럼은 서로 공유**되어야 한다.
3. 

## Inner join

= intersect

- 교집합의 느낌이다.
- A 테이블과 B 테이블이 모두 가지고있는 데이터만 검색

![https://user-images.githubusercontent.com/74339882/129435437-9f24c455-a47a-4040-a8a6-42dacdf4c08d.png](https://user-images.githubusercontent.com/74339882/129435437-9f24c455-a47a-4040-a8a6-42dacdf4c08d.png)

```sql
SELECT Column_name(s)
FROM   table1
       INNER JOIN table2
               ON table1.column_name = table2.column_name;
```

```sql
SELECT 테이블별칭.조회할칼럼,
       테이블별칭.조회할칼럼
FROM   기준테이블 별칭
       INNER JOIN 조인테이블 별칭
               ON 기준테이블별칭.기준키 =
                  조인테이블별칭.기준키
```

## Outer join

### Left Outer join

- 기준테이블의 값(table1) + 테이블과 기준테이블의 중복된 값(table1과 2의 중복된 값)
- 테이블1의 모든 데이터 + 테이블1과 테이블2의 중복되는 값이 검색

![https://user-images.githubusercontent.com/74339882/129435541-b3120d72-bdaf-46c3-94e7-075ee8fdd03f.png](https://user-images.githubusercontent.com/74339882/129435541-b3120d72-bdaf-46c3-94e7-075ee8fdd03f.png)

```sql
SELECT Column_name(s)
FROM   table1
       LEFT JOIN table2
              ON table1.column_name = table2.column_name;
```

### Right Outer join

- 결과값은 테이블2의 모든 데이터와 + 테이블1과 테이블2의 중복되는 값

![https://user-images.githubusercontent.com/74339882/129435546-1f9c67e1-72a4-4771-a09a-1bf558197e47.png](https://user-images.githubusercontent.com/74339882/129435546-1f9c67e1-72a4-4771-a09a-1bf558197e47.png)

```sql
SELECT Column_name(s)
FROM   table1
       RIGHT JOIN table2
               ON table1.column_name = table2.column_name;
```

```sql
SELECT 테이블별칭.조회할칼럼,
       테이블별칭.조회할칼럼
FROM   기준테이블 별칭
       RIGHT OUTER JOIN 조인테이블 별칭
                     ON 기준테이블별칭.기준키 =
                        조인테이블별칭.기준키
```

### Full Outer join (== Full join)

- 그냥 합집합이라고 생각하면 됨
- 모든 데이터

![https://user-images.githubusercontent.com/74339882/129435610-82f8037b-c6f0-4b4c-b3d9-3b3473faafeb.png](https://user-images.githubusercontent.com/74339882/129435610-82f8037b-c6f0-4b4c-b3d9-3b3473faafeb.png)

```sql
SELECT Column_name(s)
FROM   table1
       full OUTER JOIN table2
                    ON table1.column_name = table2.column_name
WHERE  CONDITION;
```

```sql
SELECT 테이블별칭.조회할칼럼,
       테이블별칭.조회할칼럼
FROM   기준테이블 별칭
       FULL OUTER JOIN 조인테이블 별칭
                    ON 기준테이블별칭.기준키 =
                       조인테이블별칭.기준키
```

## Self join

- 하나의 테이블을 여러번 복사해서 join
    - 자기 자신 테이블과 join
- FROM 뒤에 테이블이 두 개가 온다는 것이 self join 의 특징 / 하나의 테이블 이름을 다르게 지정해줘야 함

```sql
SELECT Column_name(s)
FROM   table1 T1,
       table1 T2
WHERE  CONDITION;
```

## Cross join

![https://user-images.githubusercontent.com/74339882/129435952-92ec14b0-0ea0-4ed0-a2a1-855c7af2c12d.png](https://user-images.githubusercontent.com/74339882/129435952-92ec14b0-0ea0-4ed0-a2a1-855c7af2c12d.png)

- 모든 경우의 수를 표현할 때 사용
- A, B 테이블이 있을 때 A 테이블의 한 행을 B테이블의 모든 행과 비교할 때
- 결과값이 엄청 많아짐 (n * m)

```sql
--방법1--
SELECT A.NAME,
       B.age
FROM   ex_table A
       CROSS JOIN join_table
```

```sql
--방법2--
SELECT A.NAME,--A테이블의 NAME조회
       B.age --B테이블의 AGE조회
FROM   ex_table A,
       join_table B
```

## Natural join

- NATURAL JOIN은 두 테이블의 동일한 이름을 가지는 칼럼이 모두 조인된다.
    - 반드시 두 테이블 간의 동일한 이름, 타입을 가진 컬럼이 필요하다.
        - 동일한 이름을 갖는 컬럼이 있지만 데이터 타입이 다르면 에러가 발생한다.
- USING 절을 사용해서 특정 칼럼을 설정해서 사용 가능
- 동일한 칼럼을 내부적으로 찾게 되므로 테이블 별칭(Alias)을 주면 오류가 발생한다.
    - 조인하는 테이블 간의 동일 컬럼이 SELECT절에 기술되도 테이블 이름을 생략해야 한다.
    - 조인에 이용되는 컬럼은 명시하지 않아도 자동으로 조인해 사용된다.

```sql
SELECT 컬럼,
       컬럼,
       …
FROM   테이블1 
Natural JOIN   테이블2 [NATURAL JOIN 테이블3] …
WHERE  검색 조건;
```