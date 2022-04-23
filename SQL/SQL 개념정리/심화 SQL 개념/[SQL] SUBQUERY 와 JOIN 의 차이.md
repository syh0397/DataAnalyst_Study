# SUBQUERY 와 JOIN 의 차이

## 📌 JOIN 의 종류

1. **묵시적 조인**
    
    SELECT 속성명
    
    FROM 테이블1, 테이블2
    
    WHERE 테이블1.속성1 = 테이블2.속성1
    

1.  **명시적 조인**
    
    SELECT 속성명
    
    FROM 테이블1 JOIN 테이블2
    
    ON 테이블1.속성1 = 테이블2.속성1
    

## 📌 **SUBQUERY 와 JOIN 의 차이**

서브 쿼리는 복잡한 SQL 쿼리문에 많이 사용됩니다.

반면에, 조인은 여러 개의 쿼리를 필요로 하지 않습니다. 

조인은 쿼리문이 복잡해지더라도 서브 쿼리에 비해 읽어내기 수월합니다.

- 서브 쿼리를 조인으로 대체할 수 있는 경우
    - 내부 쿼리가 단일한 값을 반환한다면 이는 조인으로도 충분히 구현할 수 있다
        - 스칼라 서브쿼리
    - IN 연산자 안에 서브 쿼리가 있다면 해당 서브 쿼리를 조인으로 바꿔 쓸 수 있습니다. 이 경우는 내부 쿼리, 즉, 서브 쿼리가 여러 개의 값을 반환합니다.
        - **NOT IN 연산자 안에 있는 서브 쿼리포함**
    - EXISTS와 NOT EXISTS 연산자 안에 있는 서브 쿼리도 조인으로 대체

- **하위 쿼리를 JOIN으로 대체할 수 없는 경우**
    - **GROUP BY가 있는 FROM의 하위 쿼리**
        
        ```sql
        SELECT city,
               sum_price
        FROM   (SELECT city,
                       Sum(price) AS sum_price
                FROM   sale
                GROUP  BY city) AS s
        WHERE  sum_price < 2100;
        ```
        
    - **WHERE 절에서 집계 값을 반환하는 하위 쿼리**
        
        ```sql
        SELECT name FROM product
        WHERE cost<(SELECT AVG(price) from sale);
        ```
        
    - **ALL 절의 하위 쿼리**
    
    ## 📌 결론
    
    - 웬만하면 `JOIN` 사용하자