# Hacker Rank (Draw The Triangle 1)

*P(R)* represents a pattern drawn by Julia in *R* rows. The following pattern represents *P(5)*:

```
* * * * *
* * * *
* * *
* *
*

```

Write a query to print the pattern *P(20)*.

---

```sql
SET @number = 21;

SELECT REPEAT('* ', @number := @number - 1)
FROM   information_schema.tables
LIMIT  20;
```

---

- '@set = ' 활용
- Repeat
- 대입연산자 ':='

---

```sql
WITH Recursive_CTE AS (
    SELECT 20 AS counter
    UNION ALL
    SELECT counter - 1
    FROM Recursive_CTE
    WHERE counter > 0
)
SELECT REPLICATE('* ', counter)
FROM Recursive_CTE
```

---

```sql
DECLARE @counter INT = 20

WHILE ( @counter > 0 )
  BEGIN
      PRINT Replicate('* ', @counter)

      SET @counter = @counter - 1
  END
```