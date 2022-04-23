# Hacker Rank (Print Prime Numbers)

Write a query to print all *prime numbers* less than or equal to 1000 . 

Print your result on a single line, and use the ampersand (&) character as your separator (instead of a space).

For example, the output for all prime numbers ≤ 10 would be:

(1000 이하의 자연수들 중 모든 소수값들을 하나의 줄로 출력해라. 이 때 소수값들 사이에 구분자를 '&'를 삽입해 출력해라.)

```
2&3&5&7
```

---

```sql
SELECT Group_concat(numb SEPARATOR '&')
FROM   (SELECT @num := @num + 1 AS NUMB
        FROM   information_schema.tables t1,
               information_schema.tables t2,
               (SELECT @num := 1) tmp) tempNum
WHERE  numb <= 1000
       AND NOT EXISTS (SELECT *
                       FROM   (SELECT @nu := @nu + 1 AS NUMA
                               FROM   information_schema.tables t1,
                                      information_schema.tables t2,
                                      (SELECT @nu := 1) tmp1
                               LIMIT  1000) tempNum1
                       WHERE  Floor(numb / numa) = ( numb / numa )
                              AND numa < numb
                              AND 1 < numa)
```

---

테이블이 주어지지 않았는데 대체 어떻게 쿼리를 하란 말인가? 이를 해결하기 위해서는 MySQL 자체에 내장되어 있는 테이블들이 담겨 있는 즉, 우리가 로컬에 MySQL을 깔게 되면 **애초부터 존재하는 테이블들을 담고 있는 information_schema 를 이용**해야 한다. information_schema에 대한 자세한 내용은 [여기](https://poqw.tistory.com/24)를 살펴보자.

[[SQL] SQL로 소수(Prime Number) 출력하기(HackerRank - Print Prime Numbers 문제)](https://techblog-history-younghunjo1.tistory.com/173)