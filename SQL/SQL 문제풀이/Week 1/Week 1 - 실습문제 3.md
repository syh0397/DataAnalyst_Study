# Week 1 - ์ค์ต๋ฌธ์  3

### ๐ย ๋ฌธ์ 12๋ฒ) 
address ํ์ด๋ธ์ ์ด์ฉํ์ฌ, postal_code ๊ฐ์ดย  ๊ณต๋ฐฑ('') ์ด๊ฑฐ๋ 35200, 17886 ์ ํด๋นํ๋ address ์ ๋ชจ๋  ์ ๋ณด๋ฅผ ํ์ธํด์ฃผ์ธ์.

- Cast(postal_code AS VARCHAR) ๋ฅผ ์งํํ๋ฉด VARCHARํ์์ผ๋ก ๋ณ๊ฒฝ๋๋ค.
- ์ฌ์ค ์์จ๋ ๋ฌธ์ ์ ๋ฐ๋ฅธ ์๊ตฌ ์กฐ๊ฑด์ ๋ง์กฑ๋จ

```sql
SELECT *,
       CASE
         WHEN postal_code = '' THEN 'empty'
         ELSE Cast(postal_code AS VARCHAR)
       END 
					AS postal_code_emptyflag
FROM   address
WHERE  postal_code IN ( '', '35200', '17886' )

--- ๋ด๊ฐ ์์ฑํ ๋ต๋ณ 

SELECT *,
FROM   address
WHERE  postal_code = ''
        OR postal_code = '35200'
        OR postal_code = '17886';
```

๋ฌธ์ 13๋ฒ) address ํ์ด๋ธ์ ์ด์ฉํ์ฌ,ย  address ์ ์์ธ์ฃผ์(=address2) ๊ฐ์ดย  ์กด์ฌํ์ง ์๋ ๋ชจ๋  ๋ฐ์ดํฐ๋ฅผ ํ์ธํ์ฌ ์ฃผ์ธ์.

- COALESCE
    
    COALESCE(NULL, 'A', 'B');
    ์์ ๊ฐ์ด ์ฌ์ฉํ๋ฉด NULL์ด ์๋๋ฉด์
    ๊ฐ์ฅ ๋จผ์  ์๋ ฅํ ๊ฐ์ธ 'A'๊ฐ ๋์ค๊ฒ ๋๋ค
    
    - ์๋๋ address2 ์ค์ empty ์ธ๊ฒ๋ค๋ง ๋์จ๋ค๋ ๋ป
    - โ ๊ทผ๋ฐ ๊ตณ์ด ?
    

```sql
select *,ย  
COALESCE(address2,'empty') as new_address2
from address
where address2 is null

-- ๋ด ๋ต์
-- ์ฐจ์ด๋ coalesce

SELECT *
FROM   address
WHERE  address2 IS NULL
```

๋ฌธ์ 14๋ฒ) staff ํ์ด๋ธ์ ์ด์ฉํ์ฌ, staff ์ย  pictureย  ์ฌ์ง์ ๊ฐ์ด ์๋ย  ์ง์์ย  id, ์ด๋ฆ,์ฑ์ ํ์ธํด์ฃผ์ธ์.ย  ๋จ ์ด๋ฆ๊ณผ ์ฑ์ย  ํ๋์ ์ปฌ๋ผ์ผ๋ก ์ด๋ฆ, ์ฑ์ํํ๋กย  ์๋ก์ด ์ปฌ๋ผ name ์ปฌ๋ผ์ผ๋ก ๋์ถํด์ฃผ์ธ์.

```sql
SELECT staff_id,
       first_name
       || ', '
       || last_name AS name
FROM   staff
WHERE  picture IS NOT NULL
```

![Untitled](Week%201%20-%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%203%2026d119803a6148ba8a425f4dab1c647c/Untitled.png)

๋ฌธ์ 15๋ฒ) rental ํ์ด๋ธ์ ์ด์ฉํ์ฌ,ย  ๋์ฌ๋ํ์ผ๋ ์์ง ๋ฐ๋ฉ ๊ธฐ๋ก์ด ์๋ ๋์ฌ๊ฑด์ ๋ชจ๋  ์ ๋ณด๋ฅผ ํ์ธํด์ฃผ์ธ์.\

```sql
SELECT *
FROM   rental
WHERE  return_date IS NULL
```