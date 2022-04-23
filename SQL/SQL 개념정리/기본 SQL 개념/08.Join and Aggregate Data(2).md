# 8.  Join and Aggregate Data(2)

- 조인과 집계데이터 = 분석 함수란
    - 특정 집합 내에서 합계 및 카운트 계산
    - 결과 건수의 변화 X
    
    ```sql
    SELECT Count(*)
             OVER(),
           p.*
    FROM   product p
    ```
    
    - 사용하고자 하는 분석함수를 쓰고 대상 컬럼을 기재 후
    - PARTITION BY에서 값을 구하는 기준 컬럼을 쓰고
    - ORDER BY에서 정렬 컬럼을 기재한다.
    
    ```sql
    SELECT   c1,
             분석함수(c2, c3, ...) 
    					OVER (partition BY c4 ORDER BY c5)
    FROM     table_name ;
    ```
    
- 조인과 집계데이터 = AVG함수
    - 분석함수 중 1개
        - 분석함수를 사용하여 결과집합을 그대로 출력하면서 GROUP_NAME 기준의 평균을 출력하였다.
        
        ```sql
        SELECT A.product_name,
               A.price,
               B.group_name,
               Avg (A.price)
                 OVER (
                   partition BY B.group_name)
        FROM   product A
               INNER JOIN product_group B
                       ON ( A.group_id = B.group_id );
        ```
        

- 조인과 집계데이터 = Row Number , Rank , Dense_Rank 함수
    - **Row Number**
        - 해당 집합내에서 순위를 구한다.
        순위를 구할 때 GROUP_NAME 컬럼 기준으로
        구하고 GROUP_NAME 기준의 각 순위는
        PRICE 컬럼 기준으로 정렬한다.
        - ROW_NUMBER 는 같은 순위가 있어도 무조건 순차적으로 순위를 매긴다.(1,2,3,4 순서)
        
        ```sql
        SELECT A.product_name,
               B.group_name,
               A.price,
               Row_number ()
                 OVER (
                   partition BY B.group_name
                   ORDER BY A.price)
        FROM   product A
               INNER JOIN product_group B
                       ON ( A.group_id = B.group_id );
        ```
        
    
    - **Rank**
        - RANK는 같은 순위가 있으면 동일 순위로 매기고 그 다음순위로건너뛴다.(1,1,3,4...순)
            - 2가 없어질수도 있음
        
    - **DENSE_RANK**
        - DENSE_RANK는 같은 순위가 있으면 동일 순위로 매기고그다음순위를건너뛰지않는다.(1,1,2,3 ... 순으로 나간다.)
            - 1,2,3은 있지만 중복 가능
- 조인과 집계데이터 = First_Value , Last_Value함수
    - First_Value
        
        ```sql
        SELECT A.product_name,
               B.group_name,
               A.price,
               First_value (A.price)
                 OVER (
                   partition BY B.group_name
                   ORDER BY A.price ) AS LOWEST_PRICE_PER_GROUP
        FROM   product A
               INNER JOIN product_group B
                       ON ( A.group_id = B.group_id );
        ```
        
        - GROUP_NAME 컬럼 기준으로 PRICE 컬럼으로 정렬한 값 중에서 가장 첫번째 나오는 PRICE 값을 출력한다.
    - Last_Value
        - LAST_VALUE함수에는
        - “RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING (안묶인 그 다음) ” 를 추가
        - DEFAULT가
            - ”RANGE BETWEEN UNBOUNDED PRECEDING (안묶인 이전) AND CURRENT ROW (현재 열)” 이기 때문이다.
        
        ```sql
        SELECT A.product_name,
               B.group_name,
               A.price,
               Last_value (A.price)
                 over (
                   PARTITION BY B.group_name
                   ORDER BY A.price RANGE BETWEEN unbounded preceding AND unbounded
                 following)
               AS HIGHEST_PRICE_PER_GROUP
        FROM   product A
               inner join product_group B
                       ON ( A.group_id = B.group_id );
        ```
        
- 조인과 집계데이터 = Lag, Lead 함수
    - Lag
        - 이전 행의 값 찾기
        - 없으면 NULL
            
            ```sql
            SELECT A.product_name,
                   B.group_name,
                   A.price,
                   **Lag** (A.price, 1)
                     OVER (
                       partition BY B.group_name
                       ORDER BY A.price )           AS PREV_PRICE,
            -- 현재행의 PRICE에서 이전행의 PRICE를 뺀다.
                   A.price - **Lag** (price, 1)
                               OVER (
                                 partition BY group_name
                                 ORDER BY A.price ) AS CUR_PREV_DIFF
            FROM   product A
                   INNER JOIN product_group B
                           ON ( A.group_id = B.group_id );
            ```
            
    - Lead
        - 다음 행의 값 찾기
        - 없으면 NULL
            
            ```sql
            SELECT A.product_name,
                   B.group_name,
                   A.price,
                   **Lead** (A.price, 1)
                     OVER (
                       partition BY B.group_name
                       ORDER BY A.price )           AS NEXT_PRICE,
            -- 현재행의 PRICE에서 다음행의 PRICE를 뺀다.
                   A.price - **Lead** (price, 1)
                               OVER (
                                 partition BY group_name
                                 ORDER BY A.price ) AS CUR_NEXT_DIFF
            FROM   product A
                   INNER JOIN product_group B
                           ON ( A.group_id = B.group_id );
            ```