# Hacker Rank (~Weather Observation Station 20)

**Top Earners**

📌 We define an employee's *total earnings* to be their monthly  $salary$ x $months$ worked, and the *maximum total earnings* to be the maximum total earnings for any employee in the **Employee** table. Write a query to find the *maximum total earnings* for all employees as well as the total number of employees who have maximum total earnings. Then print these values as 2 space-separated integers.

```sql
SELECT e.months * e.salary AS earnings,
       Count(*)
FROM   employee e
GROUP  BY earnings
ORDER  BY earnings DESC
LIMIT  1;

```

---

**Weather Observation Station 2**

📌 Query the following two values from the **STATION** table:

1. The sum of all values in LAT_N rounded to a scale of 2 decimal places.
2. The sum of all values in LONG_W rounded to a scale of 2 decimal places.**Input Format**

```sql
Select round(sum(LAT_N),2),
        round(sum(LONG_W),2)
from STATION
```

---

**Weather Observation Station 13**

📌 Query the sum of Northern Latitudes (LAT_N) from **STATION** having values greater than 38.7880 and less than 137.2345. Truncate your answer to decimal places.

```sql
SELECT Round(Sum(lat_n), 4)
FROM   station
WHERE  lat_n > 38.7880
       AND lat_n < 137.2345;
```

---

**Weather Observation Station 14**

📌  Query the greatest value of the Northern Latitudes (LAT_N) from **STATION** that is less than 137.2345 . Truncate your answer to  decimal places.

```sql
SELECT ROUND(MAX(LAT_N),4)
FROM STATION
WHERE LAT_N < 137.2345;
```

---

**Weather Observation Station 15**

📌  Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in **STATION** that is less than 137.2345. Round your answer to 4 decimal places.

```sql
SELECT ROUND(LONG_W,4)
FROM STATION
WHERE LAT_N IN (
    SELECT MAX(LAT_N)    
    FROM STATION
    WHERE LAT_N < 137.2345
);
```

---

**Weather Observation Station 16**

📌  Query the smallest *Northern Latitude* (*LAT_N*) from **STATION** that is greater than38.7780 . Round your answer to  4 decimal places.

```sql
SELECT ROUND(min(LAT_N),4)
FROM STATION
WHERE LAT_N > 38.7780;
```

---

**Weather Observation Station 17**

📌  Query the *Western Longitude* (*LONG_W*)where the smallest *Northern Latitude* (*LAT_N*) in **STATION** is greater than . Round your answer to  decimal places.

```sql
select round(LONG_W,4)
from STATION
where LAT_N in (
        select min(LAT_N)
        from STATION
        where LAT_N > 38.7780);
```

---

**Weather Observation Station 18**

📌  Consider P1(a,b) and P2(c,d) to be two points on a 2D plane.

- a happens to equal the minimum value in Northern Latitude (LAT_N in **STATION**).
- b happens to equal the minimum value in Western Longitude (LONG_W in **STATION**).
- c happens to equal the maximum value in Northern Latitude (LAT_N in **STATION**).
- d happens to equal the maximum value in Western Longitude (LONG_W in **STATION**).

Query the [Manhattan Distance](https://xlinux.nist.gov/dads/HTML/manhattanDistance.html) between points P1 and P2 and round it to a scale of  decimal places.

- $***Manhattan Distance는 |x1 - x2| + |y1 - y2|***$

```sql
SELECT Round(( Abs(Max(lat_n) - Min(lat_n))
               + Abs(Max(long_w) - Min(long_w)) ), 4)
FROM   station;
```

---

**Weather Observation Station 19**

📌  Consider P1(a,c) and P2(b,d) to be two points on a 2D plane where (a,b) are the respective minimum and maximum values of Northern Latitude (LAT_N) and (c,d) are the respective minimum and maximum values of Western Longitude (LONG_W) in **STATION**.

Query the [Euclidean Distance](https://en.wikipedia.org/wiki/Euclidean_distance) between points P1 and P2 and format your answer to display 4 decimal digits.

```sql
SELECT
    ROUND(SQRT(
        POWER(MAX(LAT_N)  - MIN(LAT_N),  2)
      + POWER(MAX(LONG_W) - MIN(LONG_W), 2)
    ), 4)
FROM 
    STATION;
```

---

**Weather Observation Station 20**

📌  A *[median](https://en.wikipedia.org/wiki/Median)* is defined as a number separating the higher half of a data set from the lower half. Query the *median* of the *Northern Latitudes* (*LAT_N*) from **STATION** and round your answer to  4 decimal places.

- 중앙값 ??

```sql
-- oracle

SELECT ROUND(MEDIAN(LAT_N),4)
FROM STATION;

```

```sql
-- my sql

SELECT ROUND(LAT_N,4)
FROM (SELECT LAT_N, PERCENT_RANK() OVER (ORDER BY LAT_N ASC) percent
      FROM STATION) A
WHERE percent=0.5;
```

```sql
SET @N := 0;
SELECT COUNT(*) FROM STATION INTO @TOTAL;

SELECT
    ROUND(AVG(A.LAT_N), 4)
FROM (SELECT @N := @N +1 AS ROW_ID, LAT_N FROM STATION ORDER BY LAT_N) A
WHERE
    CASE WHEN MOD(@TOTAL, 2) = 0 
            THEN A.ROW_ID IN (@TOTAL/2, (@TOTAL/2+1))
            ELSE A.ROW_ID = (@TOTAL+1)/2
    END
;
```