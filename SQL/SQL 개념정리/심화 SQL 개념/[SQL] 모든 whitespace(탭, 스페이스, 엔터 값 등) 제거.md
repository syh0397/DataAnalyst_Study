# [SQL] 모든 whitespace(탭, 스페이스, 엔터 값 등) 제거

```sql
replace (키워드,'바꿀내용','어떻게바꿀건지내용')
```

을

```sql
replace (키워드, ' ', '')
```

으로 작성하면 공백만 바뀐다.

모든 화이트 스페이스 제거를 위해서는 (외부에서 오는 데이터는 언제든 이상한 값으로 올수 있기 때문에 )

```sql
Regexp_replace (키워드,  '\S*', '
```  
```

로 작성해야 한다.

---

- Oracle 공식문서는 여기

[https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions130.htm](https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions130.htm)

![Untitled](%5BSQL%5D%20%E1%84%86%E1%85%A9%E1%84%83%E1%85%B3%207853d/Untitled.png)

REGEXP_REPLACE는 문자열에서 정규식 패턴을 검색할 수 있도록 하여 REPLACE 함수의 기능을 확장합니다. 기본적으로 이 함수는 정규식 패턴이 나타날 때마다 replace_string으로 교체된 source_char를 반환합니다. 반환된 문자열은 source_char와 동일한 문자 집합에 있습니다. 함수는 첫 번째 인수가 LOB가 아니면 VARCHAR2를 반환하고 첫 번째 인수가 LOB이면 CLOB를 반환합니다.

---

**문법**

regexp_replace() 함수의 구문(Syntax)은 다음과 같다.

```sql
SELECT regexp_replace( string, pattern 
			[, replacement_string 
			[, start_position 
			[, nth_appearance [, match_parameter ] ] ] ] )
FROM   table_name
```

source_char : 대상 문자열

pattern : 정규표현식 패턴

replace_string : 바꿔치기할 문자열

position : 문자열내에서 (패턴을 체크할) 처음 시작 위치

occurrence : 몇번째 일치하는 건지.  ( 0 이면 전부 바꿔치기 )

match_param : 'i' (대소문자 무시),  'c' (대소문자 구분)

---

### **자주 사용하는 응용편**

1. "["와 "]" 사이에 문자를 공백 처리하기,  괄호의 정의를 정하고 사이의 내용을 제거하면 됩니다.

```sql
**regexp_replace(s, "\\[.*\\]", "")**
```

2. 숫자와 문자를 제외하고 모두 제거

```sql
**regexp_replace(nm, '[^A-Z0-9 ]', '')**
```

3. 공백이 2개 이상인 부분을 제거

```sql
**REGEXP_REPLACE('Kontext is a website for data engineers.','[\s]{2,}', '')**
```

4. 끝에 문자가 _(으로 시작하고)_(으로 사작하지 않는) 문자로 끝나는 것

```sql
**regexp_replace('The_quick brown fox jumped over the_fence', '_[^_]*$','')**
```

[https://nicola-ml.tistory.com/57](https://nicola-ml.tistory.com/57) 참고