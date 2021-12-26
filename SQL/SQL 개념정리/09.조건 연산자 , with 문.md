# 9. 조건 연산자 , with 문

### 📌 CASE

- 파이썬의 IF ELSE 문 같은 조건문을 구현 할 수 있다.
    - CASE 표현은 IF - THEN - ELSE 논리와 유사항 방식으로 사용할 수 있는 함수이다.
    - 오라클에서는 DECODE  함수를 사용할 수도 있다.
    
    ```sql
    SELECT
           CASE
    			WHEN 조건식1 THEN 결과1  -- WHEN은 IF와 유사
    			WHEN 조건식2 THEN 결과2  -- WHEN은 ELSE IF와 유사 ELSE는 ELSE와 유사
    						      ELSE 결과3
                END;
    ```
    
    ```sql
    SELECT   ENAME,                     -- 직원이름
             CASE   WHEN  SAL > 2000    -- 급여가 2,000 초과면
                    THEN  SAL           -- 급여를 표시하고
                    ELSE  2000          -- 해당하지 않으면 2,000을 표시하라
             END    REVISED_SALARY      -- CASE 행의 이름은  REVISED_SALARY 다
    FROM     EMP ;
    ```
    

### 📌 **COALESCE**

- 입력한 컬럼값 중에서 NULL 이 아닌 첫번째 값을 나타냄
    
    ```sql
    SELECT product,
           ( price - COALESCE(discount, 0) ) AS NET_PRICE 
    FROM   tb_item_coalesce_test;
    ```
    
    - 순이익 NET_PRICE 를 계산하려면 가격 컬럼에 있는 값에서 할인 컬럼에 있는 값을 빼면 되는데
    - 이때 Null 이 있으면 0을 리턴한다.
    
    ```sql
    SELECT product,
           ( price - CASE
                       WHEN discount IS NULL THEN 0 -- if discount == Null
    																							  -- return 0
                       ELSE discount                -- else return discount(x) :) 
                     END ) AS NET_PRICE
    FROM   tb_item_coalesce_test;
    ```
    
    - 위의 코드랑 결과는 똑같다

### 📌 **NULLIF**

- 입력한 두개의 인자값이 동일하면 NULL 리턴
- 아니면 첫번째 인자값 리턴
- **NULLIF(표현식1, 표현식2)**
    - 표현식1이 표현식2와 같으면 NULL을, 같지 않으면 표현식1을 리턴한다.
    - 특정 값을 NULL로 변경해야 할 때 유용하게 사용할 수 있다.

```sql
-- NULLIF (표현식1, 표현식2) : 표현식1 과 2과 같으면 NULL, 다르면 표현식 1 리턴
-- MGR 7698 이면 NULL로 표시한다. 
SELECT ename,
       empno,
       mgr,
       NULLIF(mgr, 7698) -- NUIF
FROM   emp;
```

### 📌 CAST

- CAST 함수를 사용하면 지정한 값을 다른 테이터 타입으로 변환할 수 있습니다.
- 형변환 함수

```sql
SELECT
 CAST ('100' AS INTEGER);

== 

SELECT
'100'::INTEGER;

-- 위 두개는 결과가 같다 
-- '100'이라는 문자열을 정수형으로 형변환
```

```sql
SELECT
 CAST ('2015-01-01' AS DATE);

-- '2015-01-01' 이라는 문자열을 DATE 타입으로 형변환
```

- CONVERT 함수도 지정한 값을 다른 테이터 타입으로 변환하고 싶을 때 사용합니다.
    - expr에는 값을 지정
    - type에는 변환하고 싶은 데이터 타입을 지정

```sql
CONVERT(expr, type)

```

- 지정 가능한 데이터 타입
    
    
    ![Untitled](9%20%E1%84%8C%E1%85%A9%E1%84%80%E1%85%A5%E1%86%AB%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%89%E1%85%A1%E1%86%AB%E1%84%8C%E1%85%A1%20,%20with%20%E1%84%86%E1%85%AE%E1%86%AB%20efae27346572415e8a581b1ab6333f2f/Untitled.png)
    

### 📌 WITH

```sql
WITH tmp1  --WITH문을 이용해서 해당 집합을 TMP1으로 지정
     AS (SELECT film_id,
                title,
                ( CASE  --영화의 상영시간별로 SHORT, MEDIUM, LONG
                    WHEN length < 30 THEN 'SHORT'
                    WHEN length >= 30
                         AND length < 90 THEN 'MEDIUM'
                    WHEN length > 90 THEN 'LONG'
                  END ) LENGTH
         FROM   film)
SELECT *  -- SELECT문에서 TMP1을 조회
FROM   tmp1
WHERE  length = 'LONG'; --TMP1 집합에서 상영시간 구분이 LONG인 집합을 출력
```

- with 문을 활용하면 select 문에서 리턴한 결과를 또 하나의 조건식으로 사용 가능합니다

### 📌 재귀쿼리

- RECURSIVE WITH 재귀 쿼리

```sql
-- 나라별 급여 평균의 평균보다 많이 급여를 주고 있는 나라와 나라의 평균 급여를 구하시오. 
-- (나라명과 나라의 평균 급여만 출력)
```

- ‘평균의 평균값’을 구하기 위해서는 **쿼리A를 SUBQUERY로 사용**해서 평균을 구한다
- 근데 이렇게 반보갷서 사용하면 **퍼포먼스 저하** 및 **가독성 저하** 문제 발생
- 이러한 문제를 해결하기 위해서 **WITH절을 사용해서 쿼리문을 재사용**할 수 있습니다.

```sql
WITH recursive tmp1 AS

(
       **SELECT employee_id ,
              manager_id ,
              full_name ,
              0 lvl
       FROM
       WHERE  manager_id IS NULL -- 관리자가 없는 사람은 최상위 관리자**

       UNION

       **SELECT e.employee_id ,
              e.manager_id ,
              e.full_name ,
              s.lvl + 1
       FROM   tb_emp_recursive_test e ,
              tmp1 s tb_emp_recursive_test
       WHERE  s.employee_id = e.manager_id**  -- 사원ID와 관리자 ID를 조인함
)

SELECT employee_id,
       manager_id,
       lpad(' ', 4 * (lvl))
              || full_name AS full_name
FROM   tmp1;

-- 이전의 결과를 계속해서 인자로 값으로 넣어주고 
-- 더 이상 넣어줄 인자가 없을 때 나머지 나머지 쿼리가 진행됩니다.
```

- 제일 위의 관리자부터 시작해서 `EMPLOYEE_ID` 1부터 계속 나열

###


### 이해가 잘 안가서 찾아본 재귀 쿼리

- **재귀** - **원래의 자리로 되돌아가거나 되돌아옴**
    - 같은걸 반복한다?
    - 한 쿼리가 반복되어 실행된다.
        
        a,b의 부모는 A, A,B의 부모는 AA
        부모가 없는 최상위 AA 구조에서 아래와 같이 코드 작성
        
        ```sql
        WITH RECURSIVE cte. -- MYsql에서는 RECURSIVE 추가
             AS 
        				(SELECT code,
                        parent_code
                 FROM   code_table
                 WHERE  code = 'AA'  -- AA 인 부분부터 시작
        
                 UNION ALL
        
                 SELECT a.code, 
                        a.parent_code
                 FROM   code_table a
                        INNER JOIN cte b
                                ON a.parent_code = b.code)
        -- AA 를 부모로 가지는 A 랑 B
        -- A 또는 B를 부모로 가지는 a 와 b 까지 모두 select
        SELECT code,
               parent_code
        FROM   cte
        ```
        

참고 [https://allmana.tistory.com/134](https://allmana.tistory.com/134)
