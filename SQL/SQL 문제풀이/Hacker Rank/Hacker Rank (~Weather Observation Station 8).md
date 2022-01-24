# Hacker Rank (~Weather Observation Station 8)

![Imgur](https://i.imgur.com/mMuctUH.png)

📌 Query the **NAME** field for all American cities in the **CITY** table with populations larger than `120000`. The *CountryCode* for America is `USA`.

The **CITY** table is described as follows:

```sql
select name
from city
where population > 120000 and countrycode = 'USA'
```

📌 Query all columns (attributes) for every row in the **CITY** table.

The **CITY** table is described as follows:

```sql
select *
from city
```

📌 Query all columns for a city in **CITY** with the *ID* `1661`.

The **CITY** table is described as follows:

```sql
select *
from city
where id = '1661'
```

📌 Query all attributes of every Japanese city in the **CITY** table. The **COUNTRYCODE** for Japan is `JPN`.

The **CITY** table is described as follows:

```sql
select *
from city
where countrycode = 'JPN'

```

📌 Query the names of all the Japanese cities in the **CITY** table. The **COUNTRYCODE** for Japan is `JPN`.

```sql
select name
from city
where countrycode = 'JPN'
```

📌 Query a list of **CITY** and **STATE** from the **STATION** table.

```sql
select city,
        state
from station
```

📌 Query a list of **CITY** names from **STATION** for cities that have an even **ID** number. Print the results in any order, but exclude duplicates from the answer.

```sql
select distinct city
from STATION
WHERE MOD(ID,2) = 0
```

📌 Find the difference between the total number of **CITY** entries in the table and the number of distinct **CITY** entries in the table.

```sql
select Count(city) - count(distinct city)
from station
```

📌 Query the two cities in **STATION** with the shortest and longest *CITY* names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

```sql
SELECT city,
       Length(city)
FROM   station
ORDER  BY Length(city) ASC,
          city
LIMIT  1;

SELECT city,
       Length(city)
FROM   station
ORDER  BY Length(city) DESC,
          city
LIMIT  1;
```

```sql
SELECT TOP 1 city,
             Len(city)
FROM   station
ORDER  BY Len(city) ASC,
          city ASC;

SELECT TOP 1 city,
             Len(city)
FROM   station
ORDER  BY Len(city) DESC,
          city ASC;
```

MSSQL

```sql
DECLARE @Small INT
DECLARE @Large INT

SELECT @Small = Min(Len(city))
FROM   station

SELECT @Large = Max(Len(city))
FROM   station

SELECT TOP 1 city      AS SmallestCityName,
             Len(city) AS Minimumlength
FROM   station
WHERE  Len(city) = @Small
ORDER  BY city ASC

SELECT TOP 1 city      AS LargestCityName,
             Len(city) AS MaximumLength
FROM   station
WHERE  Len(city) = @Large
ORDER  BY city ASC
```

Oracle

```sql
(SELECT city,
        Char_length(city) AS len_city
 FROM   station
 WHERE  Char_length(city) = (SELECT Char_length(city)
                             FROM   station
                             ORDER  BY Char_length(city)
                             LIMIT  1)
 ORDER  BY city
 LIMIT  1)
UNION ALL
(SELECT city,
        Char_length(city) AS len_city
 FROM   station
 WHERE  Char_length(city) = (SELECT Char_length(city)
                             FROM   station
                             ORDER  BY Char_length(city) DESC
                             LIMIT  1)
 ORDER  BY city DESC
 LIMIT  1)
ORDER  BY Char_length(city);
```

📌 Query the list of *CITY* names starting with vowels (i.e., `a`, `e`, `i`, `o`, or `u`) from **STATION**. Your result *cannot* contain duplicates.

```sql
SELECT DISTINCT city
FROM   station
WHERE  city LIKE 'A%'
        OR city LIKE 'E%'
        OR city LIKE 'I%'
        OR city LIKE 'O%'
        OR city LIKE 'U%';
```

```sql
-- 정규식 활용 

```

📌 Query the list of *CITY* names from **STATION** which have vowels (i.e., *a*, *e*, *i*, *o*, and *u*) as both their first *and* last characters. Your result cannot contain duplicates.

```sql
SELECT distinct CITY 
FROM STATION 
where (CITY LIKE 'a%' 
    OR CITY LIKE 'e%' 
    OR CITY LIKE 'i%' 
    OR CITY LIKE 'o%'
    OR CITY LIKE 'u%'
) AND 
(CITY LIKE '%a' 
    OR CITY LIKE '%e'
    OR CITY LIKE '%i'
    OR CITY LIKE '%o'
    OR CITY LIKE '%u'
)
```

```sql
SELECT DISTINCT city
FROM   station
WHERE  city REGEXP "^[aeiou].*[aeiou]$";
```