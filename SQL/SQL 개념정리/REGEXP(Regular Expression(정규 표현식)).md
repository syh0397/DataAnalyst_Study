# REGEXP(Regular Expression(정규 표현식))

## 1. **정규 표현식 이란?**

- **특정한 조건의 문자**를 **‘`검색`’**하거나 **‘`치환`’**하는 과정을 **매우 간편하게 처리할 수 있도록 해주는 수단**
    - **해당 `Pattern`과 일치하는 문자열을 검색하는 것**
    - = 대상 문자열에 정규표현식을 적용해서 찾을 문자열을 검색해내는 것
    

## 2. 패턴의 종류

### Matching

| Pattern | 기능 | 예시 | 설명 |
| --- | --- | --- | --- |
| . | 문자 하나 | "..." | 문자열의 길이가 세 글자 이상인 것을 찾음. |
| I(수직선) | 또는 (OR). I(수직선)로 구분된 문자에 해당하는 문자열을 찾음. | "데이터I(수직선)데이타" | ‘데이터’ 또는 ‘데이타’에 해당하는 문자열을 찾음. |
| [] | [] 안에 나열된 패턴에 해당하는 문자열을 찾음. | "[123]d" | 대상 문자열에서 ‘1d’ 또는 ‘2d’ 또는 ‘3d’인 문자열을 찾음. |
| ^ | 시작하는 문자열을 찾음. | "^안녕" | 대상 문자열에서 ‘안녕’으로 시작하는 문자열을 찾음. |
| $ | 끝나는 문자열을 찾음. | "잘가$" | 대상 문자열에서 ‘잘가’로 끝나는 문자열을 찾음. |

### Numbers Limit

| Pattern | 기능 | 예시 | 설명 |
| --- | --- | --- | --- |
| * | 0회 이상 나타나는 문자 | "a*" | ‘a’가 0번 이상 등장하는 문자열을 찾음. ‘b’, ‘a’, ‘aa’ 모두 해당. |
| + | 1회 이상 나타나는 문자 | "국+" | ‘국’이 1번 이상 등장하는 문자열을 찾음. ‘한국’, ‘미역국’, ‘국거리’ 모두 해당. |
| {m,n} | m회 이상 n회 이하 반복되는 문자 | "치{1,2}" | ‘치’가 1회 이상 2회 이하 반복하는 문자열을 찾음. ‘치커리’, ‘치카치카’ 모두 해당. |
| ? | 0~1회 나타나는 문자 | "[가나다]?" | ‘가’ 또는 ‘나’ 또는 ‘다’가 0~1회 등장하는 문자열을 찾음. ‘가지마’, ‘나라’, ‘안녕’ 모두 해당. |

### char

| 패턴 | 기능 | 사용 예시 | 설명 |
| --- | --- | --- | --- |
| [A-z] 또는 [:alpha:] 또는 \a | 알파벳 대문자 또는 소문자인 문자열을 찾음 | "[A-z]+" | 대상 문자열에서 알파벳이 한 개 이상인 문자열을 찾음 |
| [0-9] 또는 [:digit:] 또는 \d | 숫자인 문자열을 찾음 | "^[0-9]+" | 한 개 이상의 숫자로 시작하는 문자열을 찾음 |

### Not

| Pattern | 기능 | 예시 | 설명 |
| --- | --- | --- | --- |
| [^문자] | 괄호 안의 문자를 포함하지 않은 문자열을 찾음 | "[^길로그]" | ‘길’ 또는 ‘로’ 또는 ‘그’를 포함하지 않는 문자열을 찾음. ‘길가’, ‘로그’, ‘그리고’ 모두 제외됨. |

```sql
-- vowel 로 시작하지 않는 것들 검색하기 

SELECT DISTINCT CITY 
FROM STATION 
WHERE CITY RLIKE '^[^aeiouAEIOU].*';
```

---

## 3. 예시

```sql
--  '설' 또는 '유"가 포함된 문자열을 찾고 싶을 때

--  정규표현식을 사용하지 않을 때

SELECT *
FROM tbl -- table
WHERE data like '%설%'
OR data like '%유%'
;

--  정규표현식을 사용할 때

SELECT *
FROM tbl
WHERE data REGEXP '설|유'  -- 엔터키 위에 '|' 
;
```

```sql
# 길이 7글자인 문자열 중 2번째 자리부터 '설'를 포함하는 문자열을 찾고 싶을 때

# 정규표현식을 사용하지 않을 때
SELECT *
FROM tbl
WHERE CHAR_LENGTH(data) = 7 
	AND SUBSTRING(data, 2, 3) = '설';

-- CHAR_LENGTH = 문자열의 길이 check  
-- => 숫자로 return

# 정규표현식을 사용할 때
SELECT *
FROM tbl
WHERE data REGEXP ('^.설...$');

-- . 온점을 가지고 자릿수를 만들었다.

```

```sql
# 텍스트와 숫자가 섞여 있는 문자열에서 **숫자로만 이루어진 문자열**을 찾고 싶을 때

# 정규표현식을 사용하지 않을 때
SELECT *
FROM tbl
WHERE data LIKE ??????????

-- >??? 

# 정규표현식을 사용할 때
SELECT *
FROM tbl
WHERE data REGEXP ('^[0-9]+$'); 
-- OR data REGEXP ('^\d$') 
-- OR data REGEXP ('^[:digit:]$');

-- $ -> 끝나는 문자열을 찾기
-- [0-9]+ -> 숫자인 문자열 
```

## 4. python

- Python에서는 `re`라는 모듈로 정규식 활용

### 정규식 원리 시각화

[https://regexper.com/#SELECT DISTINCT CITY FROM STATION
WHERE CITY RLIKE '^[^aeiouAEIOU].*'%3B](https://regexper.com/#SELECT%20DISTINCT%20CITY%20FROM%20STATION%20%0AWHERE%20CITY%20RLIKE%20'%5E%5B%5EaeiouAEIOU%5D.*'%3B)

![Untitled](REGEXP(Regular%20Expression(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%80%E1%85%B2%20%E1%84%91%E1%85%AD%E1%84%92%E1%85%A7%E1%86%AB%E1%84%89%E1%85%B5%E1%86%A8))%20255c0565f52c4bc18f843ab2b6aa3211/Untitled.png)

---

## **참고**

[https://velog.io/@gillog/MySQL-REGEXPRegular-Expression정규-표현식](https://velog.io/@gillog/MySQL-REGEXPRegular-Expression%EC%A0%95%EA%B7%9C-%ED%91%9C%ED%98%84%EC%8B%9D)

[https://yurimkoo.github.io/analytics/2019/10/26/regular_expression.html](https://yurimkoo.github.io/analytics/2019/10/26/regular_expression.html)