# 3. IN 연산자, Between, Like / Isnull

# 7. IN 연산자

**데이터 조회와 필터링**

```sql
SELECT *
FROM TABLE_NAME
WHERE COLUMN_NAME IN (VALUE1, VALUE2, ...) -- 밸류 1, 밸류 2가 있는지 없는지 확인!! 
;
```

COLUMN_NAME이 가지고 있는 집합에서 VALUE1, VALUE2 등의 값이 존재하는지 확인

```sql
SELECT *
FROM TABLE_NAME
WHERE COLUMN_NAME 
	IN (SELECT COLUMN_NAME2 FROM TABLE_NAME2)  -- 서브쿼리라고 합니다.
;
```

COLUMN_NAME이 가지고 있는 집합에서 TABLE_NAME2의 COLUMN_NAME2의 집합이 존재하는지 확인

---

### 실습

```sql
SELECT   customer_id ,
         rental_id ,
         return_date
FROM     rental
WHERE    customer_id IN (1,
                         2). -- 커스터머 아이디가 1혹은 2가 포함된 데이터만 ㄹㅌ/ 
															--근데 개오래 걸림
ORDER BY return_date         -- 리턴데이터 기준 내림차순!
         DESC;
```

- (아래) OR 적는 것  = , 콤마찍는 것

```sql
SELECT customer_id,
       rental_id,
       return_date
FROM   rental
WHERE  customer_id = 1
        OR customer_id = 2 --or 이라고 적어줘야함
ORDER  BY return_date DESC;
```

```sql
SELECT   customer_id ,
         rental_id ,
         return_date
FROM     rental
WHERE    customer_id NOT IN (1,
                             2)
ORDER BY return_date customer_id DESC;
```

- not in 을 쓰면 1도 2도 아닌것을 return

```sql
SELECT
CUSTOMER_ID , RENTAL_ID
, RETURN_DATE
FROM RENTAL
WHERE    CUSTOMER_ID <> 1 
		 AND CUSTOMER_ID <> 2
ORDER BY RETURN_DATE
DESC;
```

- NOT IN 연산자는 ‘AND’ && ‘<>‘ 과 같다
- 근데 위에 있는게 더 보기 좋다.

IN 연산자실습 - 서브쿼리

- 서브쿼리 ⇒ 메인쿼리에 도움이 되는 역할을 한다.

```sql
SELECT
from   customer_id rental
WHERE  cast (return_date AS date) = '2005-05-27';
```

'RETURN_DATE'가 2'005년 5월 27일'인 CUSTOMER_ID 를 출력한다. data라는 이름으로 

- 테이블 설계 왜 하는가
    
    ⇒ 데이터의 중복을 방지하고자 설계.  정규화와 관련된 개념! 
    

# 8. Between

- **특정 범위안에 들어가는 집합을 출력하는 연산자**

```sql
SELECT *
FROM TABLE_NAME
WHERE COLUMN_NAME BETWEEN VALUE_A AND VALUE_B;
```

COLUMN_NAME >= VALUE_A `AND` COLUMN_NAME <= VALUE_B

```sql
SELECT *
FROM TABLE_NAME
WHERE COLUMN_NAME
NOT BETWEEN VALUE_A AND VALUE_B
```

COLUMN_NAME < VALUE_A `OR` COLUMN_NAME > VALUE_B

실습 

```sql
SELECT CUSTOMER_ID , PAYMENT_ID, AMOUNT
FROM PAYMENT
WHERE AMOUNT BETWEEN 8 AND 9;
```

Amount 가 8과 9 사이인 집합을 선택한다. 

```sql
-- 위와 같은코드
SELECT customer_id,
       payment_id,
       amount
FROM   payment
WHERE  amount >= 8
       AND amount <= 9;
```

어느 것이 가독성이 더 좋을까 ? ⇒ 가독성은 그냥 FORMAT SQL을 하자 ! 

```sql
SELECT customer_id,
       payment_id,
       amount
FROM   payment
WHERE  amount NOT BETWEEN 8 AND 9;
```

AMOUNT가 8부터 9사이가 아닌 집합을 출력한다.

```sql
-- 동일한 결과를 알려줌 
SELECT customer_id,
       payment_id,
       amount
FROM   payment
WHERE  amount < 8
        OR amount > 9;
```

```sql
SELECT customer_id ,
       payment_id ,
       amount ,
       payment_date
FROM   payment
WHERE  to_char(payment_date, 'YYYY-MM-DD’) 
BETWEEN '2007-02-07' AND '2007-02-15';
```

```sql
-- 위랑 결과가 같음

WHERE
CAST(PAYMENT_DATE AS DATE)
BETWEEN '2007-02-07' AND '2007-02-15'
```

> CAST 와 TO_CHAR 의 차이
> 
- 둘다 형 변환하는 함수인데 [https://aljjabaegi.tistory.com/462](https://aljjabaegi.tistory.com/462)에 차이가 나와있다.
- CAST(형변환할 컬럼 AS 변환할타입)
- CAST 함수를 사용할 때 주의 하셔야 될 점 - 소수점을 포함하는 숫자 타입 변환 시 만약 기존 자릿수보다 작은 자릿수로 CAST 하게 되면 ROUND(반올림) 되어 처리됩니다.

# 9. Like / Isnull

```sql
SELECT *
FROM TABLE_NAME
WHERE COLUMN_NAME LIKE 특정패턴
```

특정 패턴과 유사한 집합을 나타낸다 

```sql
SELECT *
FROM TABLE_NAME
WHERE COLUMN_NAME NOT LIKE 특정패턴
```

특정 패턴과 유사하지 않은 집합

LIKE 연산자는 조회 조건 값이 명확하지 않을 때 사용합니다. LIKE 연산자는 ‘~와 같다’라는 의미입니다.

■ LIKE 연산자는 %와 _ 같은 기호 연산자(wild card)와 함께 사용합니다.

- 조건에는 문자나 숫자를 포함할 수 있습니다.
- %는 ‘모든 문자’라는 의미고,
- _는 ‘한 글자’라는 의미입니다.

> '-' : 글자숫자를 정해줌(EX 컬럼명 LIKE '홍_동')
> 

> '%' : 글자숫자를 정해주지않음(EX 컬럼명 LIKE '홍%')
> 

```sql
--A로 시작하는 문자를 찾기--
SELECT 컬럼명
FROM   테이블
WHERE  컬럼명 LIKE 'A%'

--A로 끝나는 문자 찾기--
SELECT 컬럼명
FROM   테이블
WHERE  컬럼명 LIKE '%A'

--A를 포함하는 문자 찾기--
SELECT 컬럼명
FROM   테이블
WHERE  컬럼명 LIKE '%A%'

--A로 시작하는 두글자 문자 찾기--
SELECT 컬럼명
FROM   테이블
WHERE  컬럼명 LIKE 'A_'

--첫번째 문자가 'A''가 아닌 모든 문자열 찾기--
SELECT 컬럼명
FROM   테이블
WHERE  컬럼명 LIKE'[^A]'

--첫번째 문자가 'A'또는'B'또는'C'인 문자열 찾기--
SELECT 컬럼명
FROM   테이블
WHERE  컬럼명 LIKE '[ABC]'

SELECT 컬럼명
FROM   테이블
WHERE  컬럼명 LIKE '[A-C]'
```

```sql
SELECT first_name,
       last_name
FROM   customer
WHERE  first_name LIKE 'Jen%';
```

FIRST_NAME이 ‘Jen’으로 시작하는 집합을 출력한다. 즉 ‘Jen’ 이후의 문자 혹은 문자열은 모두 매칭된다.

---

```sql
SELECT *
FROM TABLE_NAME
WHERE COLUMN_NAME IS NULL;
```

COLUNM_NAME 컬럼의 값이 NULL인 집합을 출력한다.

```sql
SELECT *
FROM TABLE_NAME
WHERE COLUMN_NAME IS NOT NULL;
```

COLUNM_NAME 컬럼의 값이 널이 아닌 집합을 출력한다.

```sql
SELECT id,
       first_name,
       last_name,
       email,
       phone
FROM   contacts
WHERE  phone = NULL;
```

PHONE 컬럼의 값이 NULL인 집합을 출력하고자 한다.

- **결과집합이 공집합이다. 널은 ‘=‘ 연산으로 비교할 수 없다.**
- =를 쓰지말고 다른걸 써야함

정답은 바로 IS NULL;

아니면 is not null