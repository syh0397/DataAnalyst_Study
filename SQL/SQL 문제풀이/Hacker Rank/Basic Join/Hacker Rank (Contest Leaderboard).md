# Hacker Rank (Contest Leaderboard)

**Contest Leaderboard**

๐ย  

You did such a great job helping Julia with her last coding contest challenge that she wants you to work on this one, too!

The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print theย *`hacker_id`*,ย *`name`*, and `total score of the hackers` ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascendingย *`hacker_id`*. Exclude all hackers with a total score ofย ย 0 from your result.

ํด์ปค์ ์ด์ ์ ๋ชจ๋  challenges์ ๋ํ ์ต๋ ์ ์์ ํฉ์๋๋ค. ๋ด๋ฆผ์ฐจ์์ผ๋ก ์ ๋ ฌ๋ ํด์ปค์ *`hacker_id`*, *`name`*, `total score of the hackers`์ ์ถ๋ ฅํ๋ ์ฟผ๋ฆฌ๋ฅผ ์์ฑํฉ๋๋ค. 

๋ ๋ช ์ด์์ ํด์ปค๊ฐ ๋์ผํ ์ด์ ์ ์ป์ ๊ฒฝ์ฐ hacker_id ์ค๋ฆ์ฐจ์์ผ๋ก ๊ฒฐ๊ณผ๋ฅผ ์ ๋ ฌํฉ๋๋ค. ๊ฒฐ๊ณผ์์ ์ด์ ์ด 0์ธ ๋ชจ๋  ํด์ปค๋ฅผ ์ ์ธํฉ๋๋ค.

```sql
SELECT h.hacker_id,
       h.NAME,
       Sum(sscore)
FROM   hackers h
       INNER JOIN (SELECT s.hacker_id,
                          Max(score) AS sscore
                   FROM   submissions s
                   GROUP  BY s.hacker_id,
                             s.challenge_id) st
               ON h.hacker_id = st.hacker_id
GROUP  BY h.hacker_id,
          h.NAME
HAVING Sum(sscore) > 0
ORDER  BY Sum(sscore) DESC,
          h.hacker_id ASC;
```