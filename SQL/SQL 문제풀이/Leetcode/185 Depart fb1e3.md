# 185. Department Top Three Salaries

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

A company's executives are interested in seeing **who earns the most money in each of the company's departments**. A **high earner** in a department is an employee who has a salary in the **top three unique** salaries for that department.

Write an SQL query to find the employees who are **high earners** in each of the departments.

Return the result table **in any order**.

The query result format is in the following example.

**Example 1:**

```
Input:
Employee table:
+----+-------+--------+--------------+
| id | name  | salary | departmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
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
| IT         | Max      | 90000  |
| IT         | Joe      | 85000  |
| IT         | Randy    | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
Explanation:
In the IT department:
- Max earns the highest unique salary
- Both Randy and Joe earn the second-highest unique salary
- Will earns the third-highest unique salary

In the Sales department:
- Henry earns the highest salary
- Sam earns the second-highest salary
- There is no third-highest salary as there are only two employees
```

출처 : [leetcode Database 185. Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/)

[Department Top Three Salaries - LeetCode](https://leetcode.com/problems/department-top-three-salaries/)

---

- 정답

```sql
# Write your MySQL query statement below 
SELECT d.name   Department,
       e.name   Employee,
       e.salary Salary
FROM   employee e
       JOIN department d
         ON e.departmentid = d.id
WHERE  e.salary IN (SELECT *
                    FROM   (SELECT DISTINCT( salary )
                            FROM   employee
                            WHERE  employee.departmentid = d.id
                            ORDER  BY salary DESC
                            LIMIT  3) 
																		as A )
```

- 설명

```sql
(SELECT DISTINCT(salary )
	FROM   employee
	-- WHERE  e. departmentid = d.id
	ORDER  BY salary DESC
	LIMIT  3) 
						as A

-- 라는 테이블로  먼저 salary의 top 3를 구해줍니다. 숫자만 
-- {"headers": ["salary"], "values": [[90000], [85000], [80000]]}

그다음

SELECT d.name   Department,
       e.name   Employee,
       e.salary Salary
FROM   employee e
       JOIN department d
         ON e.departmentid = d.id
라는 테이블을 만들어주고 
Where 조건절에 해당하는 조건을 넣으면 됨 
```

- 다른정답

```sql

select tbl.department,
				tbl.employee,
				tbl.salary
from (select d.name as department
			,e.name as employee
			,e.salary
			,Dense_rank() over (partition by departmentid order by salary DESC) as dr 
from employee e
	join department d 
			on e.departmentid = d.id) as tbl 
where tbl.dr <= 3
```