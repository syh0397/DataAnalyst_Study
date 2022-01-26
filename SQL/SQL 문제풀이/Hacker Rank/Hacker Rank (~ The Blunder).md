# Hacker Rank (~ The Blunder)

**Revising Aggregations - The Count Function**

📌 Query a *count* of the number of cities in **CITY** having a *Population* larger than $100,000$

```sql
select count(*)
from city
where Population > 100000
```

---

**Revising Aggregations - The Sum Function**

📌 Query the total population of all cities in **CITY** where *District* is **California**.

```sql
select sum(population)
from city
where District =  'California'
```

---

**Revising Aggregations - Averages**

📌 Query the average population of all cities in **CITY** where *District* is **California**.

```sql
select avg(population)
from city
where District = 'California'
```

---

**Average Population**

📌 Query the average population for all cities in **CITY**, rounded *down* to the nearest integer.

```sql
select round(avg(population),0)
from city
```

---

**Japan Population**

📌 Query the sum of the populations for all Japanese cities in **CITY**. The *COUNTRYCODE* for Japan is **JPN**.

```sql
SELECT SUM(POPULATION) 
FROM CITY 
WHERE COUNTRYCODE='JPN';
```

---

**Population Density Difference**

📌 Query the difference between the maximum and minimum populations in **CITY**.

```sql
SELECT MAX(POPULATION) - MIN(POPULATION)
FROM CITY
```

---

❇️ **The Blunder**

📌 Samantha was tasked with calculating the average monthly salaries for all employees in the **EMPLOYEES** table, but did not realize her keyboard's  0 key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary.

Write a query calculating the amount of error (i.e.:$actual - miscalculated$)  average monthly salaries), and round it up to the next integer.

- 계산을 완료할 때까지 키보드 키가 고장난 것을 깨닫지 못했습니다 ⇒ ....

 

```sql
SELECT CEIL(
    AVG(Salary)-
    AVG(
        REPLACE(Salary,'0','')
        )
            )
FROM  EMPLOYEES

-- select 에도 replace를 쓸 수 있구나 ! 
```

CEIL?

```sql
CEIL(3.45)  -- 올림
=> 4
FLOOR(3.45) -- 내림
=> 3
ABS(-2) -- 절대값
=> 2
```