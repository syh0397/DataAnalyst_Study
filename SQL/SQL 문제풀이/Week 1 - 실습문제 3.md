# Week 1 - 실습문제 3

### 📌 문제12번) 
address 테이블을 이용하여, postal_code 값이  공백('') 이거나 35200, 17886 에 해당하는 address 에 모든 정보를 확인해주세요.

- Cast(postal_code AS VARCHAR) 를 진행하면 VARCHAR타입으로 변경된다.
- 사실 안써도 문제에 따른 요구 조건은 만족됨

```sql
SELECT *,
       CASE
         WHEN postal_code = '' THEN 'empty'
         ELSE Cast(postal_code AS VARCHAR)
       END 
					AS postal_code_emptyflag
FROM   address
WHERE  postal_code IN ( '', '35200', '17886' )

--- 내가 작성한 답변 

SELECT *,
FROM   address
WHERE  postal_code = ''
        OR postal_code = '35200'
        OR postal_code = '17886';
```

문제13번) address 테이블을 이용하여,  address 의 상세주소(=address2) 값이  존재하지 않는 모든 데이터를 확인하여 주세요.

- COALESCE
    
    COALESCE(NULL, 'A', 'B');
    위와 같이 사용하면 NULL이 아니면서
    가장 먼저 입력한 값인 'A'가 나오게 된다
    
    - 아래는 address2 중에 empty 인것들만 나온다는 뜻
    - ⇒ 근데 굳이 ?
    

```sql
select *,  
COALESCE(address2,'empty') as new_address2
from address
where address2 is null

-- 내 답안
-- 차이는 coalesce

SELECT *
FROM   address
WHERE  address2 IS NULL
```

문제14번) staff 테이블을 이용하여, staff 의  picture  사진의 값이 있는  직원의  id, 이름,성을 확인해주세요.  단 이름과 성을  하나의 컬럼으로 이름, 성의형태로  새로운 컬럼 name 컬럼으로 도출해주세요.

```sql
SELECT staff_id,
       first_name
       || ', '
       || last_name AS name
FROM   staff
WHERE  picture IS NOT NULL
```

![Untitled](Week%201%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%203%2026d119803a6148ba8a425f4dab1c647c/Untitled.png)

문제15번) rental 테이블을 이용하여,  대여는했으나 아직 반납 기록이 없는 대여건의 모든 정보를 확인해주세요.\

```sql
SELECT *
FROM   rental
WHERE  return_date IS NULL
```