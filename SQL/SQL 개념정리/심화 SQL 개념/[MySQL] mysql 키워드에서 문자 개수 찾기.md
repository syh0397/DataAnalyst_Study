# [MySQL] mysql 키워드에서 문자 개수 찾기

```sql
CHAR_LENGTH(cm.card_nm) -CHAR_LENGTH(REPLACE('키워드나 컬럼명', ' ', ''))
```

- `CHAR_LENGTH`  = 문자열의 길이를 return 한다.
- 공백을 찾고싶어서 ' ' 를 사용다른 문자를 찾고싶으면 두번째 ''안에 다른 문자를 넣으면 됨
- ''으로 대체 하는 replace 사용

```sql
SELECT
       CASE
              WHEN Char_length(cm.card_nm) -Char_length(Replace(cm.card_nm, ' ', '')) = 1 THEN '1'
              ELSE
       end
from   테이블
```

이런식으로 나누어 라벨링 가능

---

비슷하게 INSTR 이라는 함수도 있다.

- INSTR() 함수는 다른 문자열에서 문자열이 처음 나타나는 위치를 반환한다
- "[W3Schools.com](http://w3schools.com/)"에서 3을 찾고 싶으면 아래와 같이 사용한다 .

```sql
SELECT Instr("w3schools.com", "3") AS MatchPosition;
```

- 아래를 참고 했다.

[https://www.w3schools.com/sql/func_mysql_instr.asp](https://www.w3schools.com/sql/func_mysql_instr.asp)

```sql
SELECT Instr(
'w3schools.com'
, '3')      as  INSTR1,
Instr(
'w3schools.com'
, '3', 4)    as INSTR2,
Instr(
'w3schools.com'
, '3', 4, 2) as INSTR3
FROM   table_name;
```

- INSTR1은 위에 있는 설명과 같다.
- INSTR2는 4번째 위치부터 3을 찾고
- INSTR3은 시작위치를 4 발생횟수를 2로 정해서 **3번째 3을 찾는다.**
- -1을 넣으면 뒤에서 부터 찾는다.

[Oracle]기반의 설명인데, 오라클과 Mysql은 비슷하니 뭐 ,,, 비슷하지 않을까 생각한다 ! 

### 만약 어떠한 문자를 찾은 위치부터 다시 찾는다면 ?  Instr() 함수 안에 또 instr()함수를 한번 더 사용할 수있을까?

- 글들을 뒤져보니 2014년에는 가능했다.
- 근데 지금은 해보니까 안된다. 아마 어느 패치가 된듯 하다
- 그럴때는 substring으로 잘라내고 instr()함수를 사용하자 !