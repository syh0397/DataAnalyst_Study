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
DECLARE @NUM  INT,
        @TIME INT -- 변수 선언

SET @NUM = 20
SET @TIME = 0

WHILE @NUM > @TIME -- 20이 1이 될 때까지
  BEGIN
      SELECT Replicate('* ', @NUM) -- NUM만큼 '* '을 출력

      SET @NUM = @NUM - 1 -- NUM이 하나씩 내려가도록
  END;
```