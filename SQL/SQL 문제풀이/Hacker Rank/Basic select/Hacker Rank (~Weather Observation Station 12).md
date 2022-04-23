# Hacker Rank (~Weather Observation Station 12)

**Weather Observation Station 09**

📌 Query the list of *CITY* names from **STATION** that *do not start* with vowels. Your result cannot contain duplicates.

```sql
select distinct city
from station
where substring(city,1,1) not in('A','E','I','O','U');

-- SUBSTRING => 몇번째 자리부터 어디까지 찾을건지 
-- 위의 예시는 첫번째 자리부터 1번째까지
```

```sql
-- vowel 로 시작하지 않는 것들 검색하기 
-- regex

SELECT DISTINCT CITY 
FROM STATION 
WHERE CITY RLIKE '^[^aeiouAEIOU].*';

---

SELECT DISTINCT CITY
FROM STATION
WHERE NOT CITY REGEXP '^[AEIOU]'

---

SELECT DISTINCT CITY
FROM STATION
WHERE CITY NOT REGEXP '^[AEIOU].*'

```

- NOT CITY 와 CITY NOT 의 차이

**Weather Observation Station 10**

📌 Query the list of *CITY* names from **STATION** that *do not end* with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY NOT REGEXP '[aeiou]$'
```

**Weather Observation Station 11**

📌 Query the list of CITY names from **STATION** that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY NOT RLIKE '^[aeiou]'
OR CITY NOT REGEXP '[aeiou]$'

```

**Weather Observation Station 12**

📌Query the list of CITY names from **STATION** that do not start with vowels and do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY NOT REGEXP '^[aeiou]'
AND CITY NOT REGEXP '[aeiou]$'
```