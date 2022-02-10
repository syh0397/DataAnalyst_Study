# 184. Department Highest Salary

Table: `Employee`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+
id is the primary key column for this table.
departmentId is a foreign key of the ID from theDepartmenttable.
Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.

```

Table: `Department`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
id is the primary key column for this table.
Each row of this table indicates the ID of a department and its name.

```

Write an SQL query to find employees who have the highest salary in each of the departments.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

```
Input:
Employee table:
+----+-------+--------+--------------+
| id | name  | salary | departmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
Department table:
+----+-------+
| id | name  |
+----+-------+
| 1  | IT    |
| 2  | Sales |
+----+-------+
Output:
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
| IT         | Max      | 90000  |
+------------+----------+--------+
Explanation: Max and Jim both have the highest salary in the IT department and Henry has the highest salary in the Sales department
.
```

---

내가 작성한 정답 

```sql
# Write your MySQL query statement below

SELECT d.name   AS department,
       e.name   AS employee,
       e.salary
FROM   employee e
       JOIN department d
         ON e.departmentid = d.id
WHERE  (e.departmentid, e.salary ) IN (SELECT departmentid,
                                         Max(salary)
                                      FROM   employee
                                      GROUP  BY departmentid);
```

정답

```sql
SELECT d.name   AS Department,
       e.name   AS employee,
       e.salary AS salary
FROM   employee e
       INNER JOIN (
                  -- 부서에서 가장 높은 임금 근로자 
                  SELECT departmentid,
                         Max(salary) AS max_salary
                   FROM   employee
                   GROUP  BY departmentid) AS dh
               ON e.departmentid = dh.departmentid
                  AND e.salary = dh.max_salary
       INNER JOIN department AS d
               ON d.id = e.departmentid
```