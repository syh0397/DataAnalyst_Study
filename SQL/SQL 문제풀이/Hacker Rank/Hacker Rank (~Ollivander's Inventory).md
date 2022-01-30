# Hacker Rank (~Ollivander's Inventory)

**Ollivander's Inventory**

📌 Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.

Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each *non-evil* wand of high power and age. Write a query to print the *id*, *age*, *coins_needed*, and *power* of the wands that Ron's interested in, sorted in order of descending *power*. If more than one wand has same power, sort the result in order of descending *age*.

```sql
SELECT w.id,
       p.age,
       w.coins_needed,
       w.power
FROM   wands w
       INNER JOIN wands_property p
               ON w.code = p.code
WHERE  w.coins_needed = (SELECT Min(coins_needed)
                         FROM   wands w2
                                INNER JOIN wands_property p2
                                        ON w2.code = p2.code
                         WHERE  p2.is_evil = 0
                                AND w.power = w2.power
                                AND p.age = p2.age)
ORDER  BY w.power DESC,
          p.age DESC
```