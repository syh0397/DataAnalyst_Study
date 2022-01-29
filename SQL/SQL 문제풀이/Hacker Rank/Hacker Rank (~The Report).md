# Hacker Rank (~The Report)

**Population Census**

📌 Given the **CITY** and **COUNTRY** tables, query the sum of the populations of all cities where the *CONTINENT* is *'Asia'*.

**Note:** *CITY.CountryCode* and *COUNTRY.Code* are matching key columns.

```sql
SELECT SUM(CITY.POPULATION)
FROM CITY
JOIN COUNTRY 
ON CITY.COUNTRYCODE = COUNTRY.CODE
WHERE COUNTRY.CONTINENT = 'Asia';
```

---

**African Cities**

📌 Given the **CITY** and **COUNTRY** tables, query the names of all cities where the *CONTINENT* is *'Africa'*.

**Note:** *CITY.CountryCode* and *COUNTRY.Code* are matching key columns.

```sql
SELECT city.name
FROM city
INNER JOIN country ON city.countrycode = country.code
WHERE country.continent = 'Africa'
```

---

**Average Population of Each Continent**

📌 Given the **CITY** and **COUNTRY** tables, query the names of all the continents (*COUNTRY.Continent*) and their respective average city populations (*CITY.Population*) rounded *down* to the nearest integer.

```sql
SELECT country.continent
     , FLOOR(AVG(city.population))
FROM city
INNER JOIN country ON city.countrycode = country.code
GROUP BY country.continent
```

---

**The Report**

📌 *Ketty* gives *Eve* a task to generate a report containing three columns: *Name*, *Grade* and *Mark*. *Ketty* doesn't want the NAMES of those students who received a grade lower than *8*. The report must be in descending order by grade 

-- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. 

Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

```sql
SELECT CASE 
            WHEN G.grade > 7 THEN S.NAME
            ELSE 'NULL'
        END AS NAME
    , G.grade
    , s.marks
FROM Students S
    INNER JOIN Grades g 
        ON s.Marks between g.min_mark and g.max_mark
ORDER BY Grade DESC, s.name, marks
```