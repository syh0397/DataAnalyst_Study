# Week 1 - 실습문제 4

각 제품 가격을 5 % 줄이면 어떻게 될까요?

```sql
SELECT productname,
       retailprice,
       retailprice * 0.95 as discount
FROM   products
```

### employees 테이블을 이용하여, 705 아이디를 갖은  직원의 , 이름, 성과  해당 직원의  태어난 해를 확인해주세요.

- 나의 답처럼 작성하면 오류가 뜬다.
    - 왜지 ? ⇒ MYSQL에서는 괜찮다는데 ?

![Untitled](Week%201%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%204%207a612ca5695d48049b6d557a389537f3/Untitled.png)

```sql
SELECT empfirstname,
       emplastname,
       Extract (year FROM empbirthdate) AS birsthday_yyyy
FROM   employees
WHERE  employeeid = 705

--- 나의 답 
-- My SQL에서는 가능 , Postgre에서는 안되는듯

SELECT empfirstname,
       emplastname,
			 YEAR (empbirthdate) as Birth_year
FROM   employees
WHERE  employeeid = 705

-- 다른답 

SELECT empfirstname,
       emplastname,
       to_char(empbirthdate, 'yyyy') as birthday_yyyy
FROM   employees
WHERE  employeeid = 705

-- 다른답 

SELECT empfirstname,
       emplastname,
       **substring** (**CAST**(empbirthdate as varchar),1,4)
FROM   employees
WHERE  employeeid = 705
```

### 📌 EXTRACT(**A** FROM **B**)

- **A (Field 값)** : year, month, day 와 같은 추출할 날짜/시간

| A (Field 값) | 의미 |
| --- | --- |
| CENTURY | 세기(21세기, 20세기) |
| DAY | 1~31에 해당하는 해당 월의 일 |
| DOW | 일요일(0) ~ 토요일(6)까지 반환하는 값 |
| DOY | 1~366 까지 해당하는 연중일수 |
| EPOCH | 1970년 1월 1일 00:00:00 UTC 부터 현재까지의 초 (unixtime) |
| HOUR | 0 ~ 23 에 해당하는 시간정보 |
| MILLISECONDS | 1/1000에 해당하는 밀리초 |
| MINUTE | 0 ~ 59에 해당하는 분 정보 |
| MONTH | 1 ~ 12에 해당하는 월 정보 |
| QUARTER | 1(1~3월), 2(4~6월), 3(7~9월), 4(10~12월) 분기로 나뉘어지는 정보 |
| SECOND | 0 ~ 59에 해당하는 초 정보 |
| WEEK | 주 정보 (1월 1일 : 1, 12월 31일: 52~53) |
| YEAR | 연도 정보. |

### **CHAR, VARCHAR 중 어떤 문자열 유형을 사용해야 하는가?**

- 주민등록번호나 사번과 같이 고정적인 길이를 갖고있는 경우는 CHAR를 사용
- 고정적인 길이를 갖지 못한다면 VARCHAR를 사용
    
    ※ VARCHAR의 경우
    
    CHAR 유형보다 적은 공간에 저장을 할 수 있기 때문에 유용하다.
    

문제4번)  customers 테이블을 이용하여,  고객의 이름과 성을 하나의 컬럼으로 전체 이름을 보여주세요 (이름, 성 의 형태로  full_name 이라는 이름으로 보여주세요)

```sql
SELECT first_name
       || ', '
       || last_name AS full_name,
       *
FROM   customer
```

문제8번) products 테이블을 활용하여, productdescription에 상품 상세 설명 값이 없는  상품 데이터를 모두 알려주세요.

```sql
SELECT *,
       Coalesce(productdescription, 'Empty') AS new_productdescription
FROM   products
WHERE  productdescription IS NULL;

-- 

SELECT *
FROM   products
WHERE  productdescription IS NULL;
```

customers 테이블을 이용하여,

 고객의 id 별로,  custstate 지역 중 WA 지역에 사는 사람과  WA 가 아닌 지역에 사는 사람을 구분해서  보여주세요. 

- customerid 와, newstate_flag 컬럼으로 구성해주세요 .
- newstate_flag 컬럼은 WA 와 OTHERS 로 노출해주시면 됩니다.

```sql
SELECT customerid,
       CASE
         WHEN custstate = 'WA' THEN 'WA'
         ELSE 'OTHERS'
       end 
						AS newstate_flag
FROM   customers c
```