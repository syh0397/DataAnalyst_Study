# Hacker Rank (~Occupations)

**The PADS**

📌 Generate the following two result sets:

1. Query an *alphabetically ordered* list of all names in **OCCUPATIONS**, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: `AnActorName(A)`, `ADoctorName(D)`, `AProfessorName(P)`, and `ASingerName(S)`.
2. Query the number of ocurrences of each occupation in **OCCUPATIONS**. Sort the occurrences in *ascending order*, and output them in the following format:
    
    > There are a total of [occupation_count] [occupation]s.
    > 
    
    where `[occupation_count]` is the number of occurrences of an occupation in **OCCUPATIONS** and `[occupation]` is the *lowercase* occupation name. If more than one *Occupation* has the same `[occupation_count]`, they should be ordered alphabetically.
    

**Note:** There will be at least two entries in the table for each type of occupation.

```sql
SELECT CONCAT(NAME,'(',LEFT(OCCUPATION, 1),')') 
FROM OCCUPATIONS 
ORDER BY NAME;

SELECT CONCAT('There are a total of ', COUNT(*), ' ', LOWER(OCCUPATION), 's.') 
FROM OCCUPATIONS 
GROUP BY OCCUPATION 
ORDER BY COUNT(*), OCCUPATION;
```

- concat ⇒

---

**Occupations**

- ✅ 참고
    
    [https://velog.io/@beemo/SQL-HackerRank-Occupations](https://velog.io/@beemo/SQL-HackerRank-Occupations)
    

📌 Pivot the *Occupation* column in **OCCUPATIONS** so that each *Name* is sorted alphabetically and displayed underneath its corresponding *Occupation*. The output column headers should be *Doctor*, *Professor*, *Singer*, and *Actor*, respectively.

**Note:** Print **NULL** when there are no more names corresponding to an occupation.

```sql
SELECT Max(CASE
             WHEN occupation = 'Doctor' THEN NAME
           END) 'Doctor',
       Max(CASE
             WHEN occupation = 'Professor' THEN NAME
           END) 'Professor',
       Max(CASE
             WHEN occupation = 'Singer' THEN NAME
           END) 'Singer',
       Max(CASE
             WHEN occupation = 'Actor' THEN NAME
           END) 'Actor'
FROM   (SELECT *,
               Row_number()
                 OVER (
                   partition BY occupation
                   ORDER BY NAME) rn
        FROM   occupations) t
GROUP  BY rn
```