# Week 1 - 실습문제 4

각 제품 가격을 5 % 줄이면 어떻게 될까요?

```sql
SELECT productname,
       retailprice,
       retailprice * 0.95 as discount
FROM   products
```

### employees 테이블을 이용하여, 705 아이디를 갖은  직원의 , 이름, 성과  해당 직원의  태어난 해를 확인해주세요.

```sql
SELECT empfirstname,
       emplastname,
       Extract (year FROM empbirthdate) AS birsthday_yyyy
FROM   employees
WHERE  employeeid = 705

--- 나의 답 

SELECT empfirstname,
       emplastname,
			 YEAR (empbirthdate) as Birth_year
FROM   employees
WHERE  employeeid = 705
```

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