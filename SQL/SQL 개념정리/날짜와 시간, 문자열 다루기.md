# 날짜와 시간, 문자열 다루기

# 날짜와 시간

날짜와 시간은 date 와  string 을 왔다갔다 하므로 잘 비교해줘야 한다.

날짜를 문자형 형태로 비교하기 위해서는 date 타입을 string으로 변환

**TO_CHAR()** 함수를 이용

```sql
WHERE '20220115' > TO_CHAR(date, 'YYYY') as Year -> 2022
WHERE '20220115' > TO_CHAR(date, 'mm') as month -> 01
WHERE '20220115' > TO_CHAR(date, 'YYYY') as day -> 15
```

string 형태를 date 형태로 비교하고 싶은 경우 

**TO_DATE()** 함수를 이용

```sql
WHERE SYSDATE > TO_DATE('20220115', 'YYYY-MM-DD') -> 2022-01-15

```

| 함수 | 내용 |
| --- | --- |
| DATE_FORMAT(날짜, 'FORMAT') | 날짜를 해당 포멧으로 변환 |
| DATE(날짜) | 날짜를 '연도-월-일'로 변환 |
| YEAR(날짜) | 날짜의 연도 반환 |
| MONTH(날짜) | 날짜의 월 반환 |
|  |  |

# 날짜와 문자열

데이터베이스를 사용하다면 날짜를 문자열로 또는 문자를 날짜로 바꿀 때가 있다

## **오라클에서 사용하는 함수**

숫자 또는 날짜 데이터 → 문자형

 **TO_CHAR(데이터, '출력 형식')**

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD')  
--  오늘 날짜의 출력 형식을 결정해서 문자로 리턴
```

```sql
SELECT TO_CHAR(100000, '00000000')
```

다음은 문자→ 날짜형으로 변환

**TO_DATE(데이터, '날짜 형식')**

```sql
SELECT TO_DATE('20080101', 'YYYY/MM/DD')  
--  20080101이라는 문자를 2008/01/01의 형태의 날짜로 리턴
```

## **MYSQL에서 사용하는 함수**

날짜→ 문자열

**DATE FORMAT(날짜, 출력 형식)**을 사용한다

```sql
SELECT DATE_FORMAT('2019-09-16 20:23:12', '%Y/%M/%D')  
--  2019/09/16 출력
```

문자열을 날짜로 변환할 때는 

**STR_TO_DATE(문자, 출력 형식)** 

```sql
SELECT STR_TO_DATE('20080101', '%Y-%M-%D')  
--  20080101이라는 문자를 2008-01-01의 형태의 날짜로 리턴
```

## **MSSQL**

날짜→ 문자

**CONVERT(포맷(길이), 날짜, 변환 형식)**

```sql
SELECT CONVERT(VARCHAR, GETDATE(), 120)  
--  결과 : 2019-09-15 15:30:11
```

120은 YYYY-MM-DD HH24:MI:SS 형식이다 

문자 → 날짜

**CONVERT(날짜 형식, 문자)**

```sql
SELECT CONVERT(DATE, '2008-01-01')  
--  2008-01-01 문자가 날짜로 변경된다
```

```sql
SELECT CONVERT(DATETIME, '2008-01-01 15:14:13')  
--  2008-01-01 15:14:13 문자가 날짜/시간으로 변경된다
```