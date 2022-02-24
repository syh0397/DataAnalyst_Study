# 180. Consecutive Numbers

Table: `Logs`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
id is the primary key for this table.

```

Write an SQL query to find all numbers that appear at least three times consecutively.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

```
Input:
Logs table:
+----+-----+
| id | num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+
Output:
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
Explanation: 1 is the only number that appears consecutively for at least three times.
```

---

---

```sql
# Write your MySQL query statement below

select distinct l1.num as ConsecutiveNums
from Logs l1
    join Logs l2
        on l1.id +1  = l2.id
    join Logs l3
        on l1.id +2  = l3.id
where l1.num = l2.num and l1.num  = l3.num
```