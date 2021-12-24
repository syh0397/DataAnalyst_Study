# Week 1 - 실습문제 1

- 문제1번) dvd 렌탈 업체의  dvd 대여가 있었던 날짜를 확인해주세요.
    - 날짜만 확인 = 유니크한 값 = distinct 사용
    - rental 테이블 rental_date ⇒ date 로 변환
    
    ```sql
    SELECT DISTINCT Date(rental_date)
    FROM   rental
    ```
    
    문제2번) 영화길이가 120분 이상이면서, 대여기간이 4일 이상이 가능한, 영화제목을 알려주세요.
    
    ```sql
    SELECT title
    FROM   film
    WHERE  length >= 120
           AND rental_duration >= 4
    ```
    
    문제3번) 직원의 id 가 2 번인  직원의  id, 이름, 성을 알려주세요
    
    ```sql
    SELECT staff_id,
           first_name,
           last_name
    FROM   staff
    WHERE  staff_id = 2
    ```
    
    문제4번) 지불 내역 중에서,   지불 내역 번호가 17510 에 해당하는  ,  고객의 지출 내역 (amount ) 는 얼마인가요?
    
    ```sql
    SELECT amount
    FROM   payment
    WHERE  payment_id = 17510
    ```
    
    문제5번) 영화 카테고리 중에서 ,Sci-Fi  카테고리의  카테고리 번호는 몇번인가요?
    
    ```sql
    SELECT category_id
    FROM   category
    WHERE  name = 'Sci-Fi'
    ```
    
    문제6번) film 테이블을 활용하여, rating  등급(?) 에 대해서, 몇개의 등급이 있는지 확인해보세요.
    
    ```sql
    SELECT DISTINCT rating
    FROM   film
    
    -------------------
    
    SELECT Count(DISTINCT rating) -- 이게 더 맞지 않나 ? 
    FROM   film
    ```
    
    ### 📌 문제7번) 대여 기간이 (회수 - 대여일) 10일 이상이였던 rental 테이블에 대한 모든 정보를 알려주세요.
    단 , 대여기간은  대여일자부터 대여기간으로 포함하여 계산합니다.
    
    - 모든정보 = *
    - 대여 일자 부터 대여기간으로 포함 = + 1
    
    ```sql
    SELECT *,
           Date(return_date) - Date(rental_date) + 1 AS diff_date
    FROM   rental
    WHERE  Date(return_date) - Date(rental_date) + 1 >= 10
    
    ```
    
    ![Untitled](Week%201%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%201%200e85101caa2a42beaf1f0a238221e206/Untitled.png)
    
    ```sql
    
    ```
    
    ![Untitled](Week%201%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%201%200e85101caa2a42beaf1f0a238221e206/Untitled%201.png)
    
    ### 문제 8번) 고객의 id 가  50,100,150 ..등 **50번의 배수**에 해당하는 고객들에 대해서, 회원 가입 감사 이벤트를 진행하려고합니다. 고객 아이디가 50번 배수인 아이디와, 고객의 이름 (성, 이름)과 이메일에 대해서 확인해주세요.
    
    - 50의 배수 = mod ⇒ 0
    
    ```sql
    select customer_id, last_name, first_name, email
    from customer
    where mod(customer_id,50) = 0
    
    ```
    
    문제9번) 영화 제목의 길이가 8글자인, 영화 제목 리스트를 나열해주세요.
    
    ```sql
    SELECT **Length**(title),
           title
    FROM   film
    WHERE  **Length**(title) = 8
    ORDER  BY title
    ```
    
    문제10번) city 테이블의 city 갯수는 몇개인가요?
    
    ```sql
    SELECT Count(*)
    FROM   city
    ```
    
    ### 📌 문제11번) 영화배우의 이름 (이름+' '+성) 에 대해서,  대문자로 이름을 보여주세요.  단 고객의 이름이 동일한 사람이 있다면,  중복 제거하고, 알려주세요.
    
    `||` represents string concatenation 문자열 연속을 나타냄. 
    Unfortunately, string concatenation is not completely portable across all sql dialects:
    
    - ansi sql: `||` (infix operator) 삽입 조작어
    - mysql: `concat` ( var arg function ). **caution**: `||` means 'logical or' ([It's configurable](http://dev.mysql.com/doc/refman/5.7/en/sql-mode.html#sqlmode_pipes_as_concat), however; thanks to [@hvd](https://stackoverflow.com/users/743382/hvd) for pointing that out)
    - oracle: `||` (infix operator), `concat` ( **caution**: function of arity 2 only ! )
    - postgres: `||` (infix operator)
    - sql server: `+` (infix operator), `concat` ( vararg function )
    - sqlite: `||` (infix operator)
    
    hopefully the confusion is complete ...
    
    🔥 즉 쌍파이프 
    
    - 문자열이나 내용을 합쳐주는 역할을 한다
    - 아래에서는 이름과 성을 합쳐서 보여준다
    
    ```sql
    SELECT DISTINCT Upper(first_name
                          || ' '
                          || last_name)
    FROM   actor
    ```
    
    문제12번) 고객 중에서,  active 상태가 0 인 즉 현재 사용하지 않고 있는 고객의 수를 알려주세요.
    
    ```sql
    SELECT Count(DISTINCT customer_id)
    FROM   customer c
    WHERE  active = 0
    ```
    
    문제13번) Customer 테이블을 활용하여,  store_id = 1 에 매핑된  고객의 수는 몇명인지 확인해보세요.
    
    - store_id = 1 에 매핑된 ⇒ 이 조건을 만족하는 유니크한 수
    
    ```sql
    select count(distinct customer_id)
    from customer c
    where store_id =1
    ```
    
    문제14번) 
    
    rental 테이블을 활용하여,  고객이 return 했던 날짜가 2005년 6월 20일에 해당했던 rental 의 갯수가 몇개였는지 확인해보세요.
    
    - date() = ‘년-월-일’
    
    ```sql
    SELECT Count(DISTINCT rental_id)
    FROM   rental
    WHERE  Date(return_date) = '2005-06-20'
    ```
    
    문제15번) film 테이블을 활용하여, 2006년에 출시가 되고 rating 이 'G' 등급에 해당하며, 대여기간이 3일에 해당하는  것에 대한 film 테이블의 모든 컬럼을 알려주세요.
    
    ```sql
    SELECT *
    FROM   film
    WHERE  release_year = '2006'
           AND rental_duration = 3
           AND rating = 'G'
    ```
    
    문제16번) langugage 테이블에 있는 id, name 컬럼을 확인해보세요 .
    
    ```sql
    SELECT language_id,
           name
    FROM   language
    ```
    
    문제17번) film 테이블을 활용하여,  rental_duration 이  7일 이상 대여가 가능한  film 에 대해서  film_id,   title,  description 컬럼을 확인해보세요.
    
    ```sql
    SELECT film_id,
           title,
           description
    FROM   film
    WHERE  rental_duration >= 7
    ```
    
    문제18번) film 테이블을 활용하여,  rental_duration   대여가 가능한 일자가 
    3일 또는 5일에 해당하는  film_id,  title, desciption 을 확인해주세요.
    
    - 또는 - or 조건이 하나만 겹쳐도 리턴
    
    ```sql
    SELECT film_id,
           title,
           description
    FROM   film
    WHERE  rental_duration = 3
            OR rental_duration = 5
    ```
    
    문제19번) Actor 테이블을 이용하여,  
    이름이 Nick 이거나  성이 Hunt 인  배우의  id 와  이름, 성을 확인해주세요.
    
    ```sql
    SELECT actor_id,
           first_name,
           last_name
    FROM   actor
    WHERE  first_name = 'Nick'
            OR last_name = 'Hunt'
    ```
    
    문제20번) 
    Actor 테이블을 이용하여, Actor 테이블의  first_name 컬럼과 last_name 컬럼을 , 
    firstname, lastname 으로 컬럼명을 바꿔서 보여주세요
    
    ```sql
    SELECT first_name AS firstname,
           last_name  AS lastname
    From Actor
    ```