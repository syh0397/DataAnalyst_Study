# SUBQUERY ์ JOIN ์ ์ฐจ์ด

## ๐ย JOIN ์ ์ข๋ฅ

1. **๋ฌต์์  ์กฐ์ธ**
    
    SELECT ์์ฑ๋ช
    
    FROM ํ์ด๋ธ1, ํ์ด๋ธ2
    
    WHERE ํ์ด๋ธ1.์์ฑ1 = ํ์ด๋ธ2.์์ฑ1
    

1.  **๋ช์์  ์กฐ์ธ**
    
    SELECT ์์ฑ๋ช
    
    FROM ํ์ด๋ธ1 JOIN ํ์ด๋ธ2
    
    ON ํ์ด๋ธ1.์์ฑ1 = ํ์ด๋ธ2.์์ฑ1
    

## ๐ย **SUBQUERY ์ JOIN ์ ์ฐจ์ด**

์๋ธ ์ฟผ๋ฆฌ๋ ๋ณต์กํ SQL ์ฟผ๋ฆฌ๋ฌธ์ ๋ง์ด ์ฌ์ฉ๋ฉ๋๋ค.

๋ฐ๋ฉด์, ์กฐ์ธ์ ์ฌ๋ฌ ๊ฐ์ ์ฟผ๋ฆฌ๋ฅผ ํ์๋ก ํ์ง ์์ต๋๋ค. 

์กฐ์ธ์ ์ฟผ๋ฆฌ๋ฌธ์ด ๋ณต์กํด์ง๋๋ผ๋ ์๋ธ ์ฟผ๋ฆฌ์ ๋นํด ์ฝ์ด๋ด๊ธฐ ์์ํฉ๋๋ค.

- ์๋ธ ์ฟผ๋ฆฌ๋ฅผ ์กฐ์ธ์ผ๋ก ๋์ฒดํ  ์ ์๋ ๊ฒฝ์ฐ
    - ๋ด๋ถ ์ฟผ๋ฆฌ๊ฐ ๋จ์ผํ ๊ฐ์ ๋ฐํํ๋ค๋ฉด ์ด๋ ์กฐ์ธ์ผ๋ก๋ ์ถฉ๋ถํ ๊ตฌํํ  ์ ์๋ค
        - ์ค์นผ๋ผ ์๋ธ์ฟผ๋ฆฌ
    - IN ์ฐ์ฐ์ ์์ ์๋ธ ์ฟผ๋ฆฌ๊ฐ ์๋ค๋ฉด ํด๋น ์๋ธ ์ฟผ๋ฆฌ๋ฅผ ์กฐ์ธ์ผ๋ก ๋ฐ๊ฟ ์ธ ์ ์์ต๋๋ค. ์ด ๊ฒฝ์ฐ๋ ๋ด๋ถ ์ฟผ๋ฆฌ, ์ฆ, ์๋ธ ์ฟผ๋ฆฌ๊ฐ ์ฌ๋ฌ ๊ฐ์ ๊ฐ์ ๋ฐํํฉ๋๋ค.
        - **NOT IN ์ฐ์ฐ์ ์์ ์๋ ์๋ธ ์ฟผ๋ฆฌํฌํจ**
    - EXISTS์ NOT EXISTS ์ฐ์ฐ์ ์์ ์๋ ์๋ธ ์ฟผ๋ฆฌ๋ ์กฐ์ธ์ผ๋ก ๋์ฒด

- **ํ์ ์ฟผ๋ฆฌ๋ฅผ JOIN์ผ๋ก ๋์ฒดํ  ์ ์๋ ๊ฒฝ์ฐ**
    - **GROUP BY๊ฐ ์๋ FROM์ ํ์ ์ฟผ๋ฆฌ**
        
        ```sql
        SELECT city,
               sum_price
        FROM   (SELECT city,
                       Sum(price) AS sum_price
                FROM   sale
                GROUP  BY city) AS s
        WHERE  sum_price < 2100;
        ```
        
    - **WHERE ์ ์์ ์ง๊ณ ๊ฐ์ ๋ฐํํ๋ ํ์ ์ฟผ๋ฆฌ**
        
        ```sql
        SELECT name FROM product
        WHERE cost<(SELECT AVG(price) from sale);
        ```
        
    - **ALL ์ ์ ํ์ ์ฟผ๋ฆฌ**
    
    ## ๐ย ๊ฒฐ๋ก 
    
    - ์ฌ๋งํ๋ฉด `JOIN` ์ฌ์ฉํ์