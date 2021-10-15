# 1. SQL이란 ?~Select , Order by, SELCET DISTINCT

# 1. SQL이란 ?

- SQL = '씨퀄'이라고 불림
    - SQL은 1974년에 SEQUEL (씨퀄)로 탄생 했지만, 지금 이름인 SQL로 변경 되어 지금까지 SQL 불린다. 그러나 에 스-큐-엘 이 아닌 씨퀄로 주로 발음된다.
- 절차적 언어(procedural language)가 아닌 **선언적 언어(descriptive language)**

주석달기 

## 단일 라인 주석 (Single-line comment)

- 한줄 라인 주석은 '--' 를 사용한다.

```sql
--Select all:
SELECT *FROM customer;

```

## 블록 주석 (Multi-line Comment)

- /* 에서 */까지의 모든 내용이 주석 처리 된다.

```sql
/*Select all the columns
of all the records
in the customers table:*/ -- 여기까지 전부 주석처리 

SELECT *FROM customer;
```

ETL이란 ?  ⇒ Extraction Transformation Loading 

A STRUCTURED ENGLISH QUERY LANGUAGE 
⇒ A STRUCIURED QUERY LANGUAGE

- 데이터 처리 방법은 SQL Query Optimizer 가 대신 처리함
    - 필요한 데이터 집합을 SQL 로 정의하면 Query Optimizer 가 SQL를 처리 데이터, 하드웨어, 테이블 구조를 고려하여 처리하기 때문에, Data Scientist, Data Analyst 는 Business Problem 에 더 집중할 수 있음

---

---

- Relational Data Model ⇒ 관계형 데이터 모델 -  EXCEL과 비슷하다고 생각하면됨
- 그럼 비 정형, 반 정형도 있음
- 비정형 - 텍스트 같은거
- 반정형 - json xml 같은거

# 2. Select , Order by

- SELECT  ⇒ 일방적으로 테이블에 저장 된 칼럼명을 조회할때 쓰인다
- ORDER BY ⇒
    - ASC 오름차순 정렬
    - DESC 내림차순 정렬 /
- 마지막에 세미콜론 절대 빼먹으면 안된다!!

---

```sql
SELECT
COLUMN_1
, COLUMN_2
, 중략...

FROM
TABLE_NAME
;
```

- 추출대상컬럼
    - 만약 테이블의 모든 컬럼을 다 보고 싶다면 ‘ * ’을 붙여라 !
    
- 추출 대상 테이블명 입력
⇒ 세미 콜론으로 끝남

```sql
-- 사용방법 
SELECT
COLUMN_1 , COLUMN_2
FROMTBL_NAME
ORDER BY 
  COLUMN_1 ASC
, COLUMN_2 DESC ;

/*
COLUMN_1은 오름차순 정렬 (Default는 ASC)
COLUMN_2은 내림차순 정렬 (Default는 ASC)
*/
```

- 실제예시

```sql
-- 실제 사용 예 

SELECT
FIRST_NAME , LAST_NAME -- 이름과 성을 조회함
FROM
CUSTOMER -- 커스토머 테이블에서 
ORDER BY -- 정렬은 1 Firstname 오름차 먼저하고 2 lastname 내림차 먼저한다.
	1 ASC,
	2 DESC 
;
```

# 3. SELCET DISTINCT

# ⇒ 중복값 제외

📌 SELECT DISTINCT 문법

```sql
SELECT
DISTINCT COLUMN_1
FROM TABLE_NAME;

-- COLUMN_1의 값이 중복값 존재시 중복값을 제거

```

```sql
SELECT
DISTINCT COLUMN_1, COLUMN_2
FROM TABLE_NAME;

-- COLUMN_1+COLUMN_2의 값이 중복값 존재시 중복값을 제거
```

```sql
SELECT
DISTINCT COLUMN_1, COLUMN_2
FROM TABLE_NAME
ORDER BY COLUMN_1, COLUMN_2;

-- COLUMN_1+COLUMN_2의 값이 중복 값 존재 시 중복 값을 제거
-- 결과를 명확하게 하기 위해 ORDERBY절 사용
```

- SELECT DISTINCT 실습 - DISTINCT사용 + 컬럼두개 + ON사용 + DESC정렬

```sql
SELECT
DISTINCT ON (BCOLOR)
	BCOLOR
, FCOLOR
FROM
	T1
ORDER BY
	BCOLOR, FCOLOR DESC;

/*
BCOLOR 컬럼 값 기준 중복 제거함
FCOLOR 컬럼 값은 단 한개 값만을 보여줌

FCOLOR 컬럼값을 보여줄때 내림차순 정렬함
*/
```

⇒ 그러니까 비 컬러 BCOLOR 기준으로 중복제거를 진행한 뒤에 f컬러 기준으로 정렬된 값에서 중복제거되고 남은 값의 행에 있는 값을 리턴한다.