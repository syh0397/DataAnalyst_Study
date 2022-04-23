# Hacker Rank (~Challenges)

**Challenges**

📌 Julia asked her students to create some coding challenges. Write a query to print the *hacker_id*, *name*, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by *hacker_id*. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.

```sql
SELECT h.hacker_id,
       h.name,
       Count(c.challenge_id) AS cnt
FROM   hackers h
       JOIN challenges c
         ON c.hacker_id = h.hacker_id
GROUP  BY h.hacker_id,
          h.name
HAVING cnt = (SELECT Count(c2.challenge_id) AS c_max
              FROM   challenges AS c2
              GROUP  BY c2.hacker_id
              ORDER  BY c_max DESC
              LIMIT  1)
        OR cnt IN (SELECT DISTINCT c_compare AS c_unique
                   FROM   (SELECT h2.hacker_id,
                                  h2.name,
                                  Count(challenge_id) AS c_compare
                           FROM   hackers h2
                                  JOIN challenges c
                                    ON c.hacker_id = h2.hacker_id
                           GROUP  BY h2.hacker_id,
                                     h2.name) counts
                   GROUP  BY c_compare
                   HAVING Count(c_compare) = 1)
ORDER  BY c_count DESC,
          h.hacker_id;
```

---

---