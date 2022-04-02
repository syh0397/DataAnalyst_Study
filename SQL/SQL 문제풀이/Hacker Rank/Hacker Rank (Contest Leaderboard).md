# Hacker Rank (Contest Leaderboard)

**Contest Leaderboard**

📌  

You did such a great job helping Julia with her last coding contest challenge that she wants you to work on this one, too!

The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the *`hacker_id`*, *`name`*, and `total score of the hackers` ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending *`hacker_id`*. Exclude all hackers with a total score of  0 from your result.

해커의 총점은 모든 challenges에 대한 최대 점수의 합입니다. 내림차순으로 정렬된 해커의 *`hacker_id`*, *`name`*, `total score of the hackers`을 출력하는 쿼리를 작성합니다. 

두 명 이상의 해커가 동일한 총점을 얻은 경우 hacker_id 오름차순으로 결과를 정렬합니다. 결과에서 총점이 0인 모든 해커를 제외합니다.

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