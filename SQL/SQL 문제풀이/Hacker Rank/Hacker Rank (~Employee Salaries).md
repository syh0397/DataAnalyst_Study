# Hacker Rank (~Employee Salaries)

---

**Higher Than 75 Marks**

📌 Query the *Name* of any student in **STUDENTS** who scored higher than  *Marks*. Order your output by the *last three characters* of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending *ID*.

```sql
SELECT Name
FROM STUDENTS
WHERE Marks > 75
ORDER BY RIGHT(Name, 3), ID

--

SELECT Name
FROM STUDENTS
WHERE Marks > 75
ORDER BY substr(Name, -3), ID

```

**Employee Names / Salaries**

📌 Write a query that prints a list of employee names (i.e.: the *name* attribute) from the **Employee** table in alphabetical order.

```sql
SELECT name
FROM Employee
ORDER BY name
```

**Employee Salaries**

📌 Write a query that prints a list of employee names (i.e.: the *name* attribute) for employees in **Employee** having a salary greater than  per month who have been employees for less than  months. Sort your result by ascending *employee_id*.

```sql
SELECT name
FROM Employee
WHERE salary > 2000 AND months < 10
ORDER BY employee_id
```