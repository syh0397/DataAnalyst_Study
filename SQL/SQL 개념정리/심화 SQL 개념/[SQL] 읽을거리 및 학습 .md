# S Q L

## ๐ฅย ์ผ์๋ณ CHECK (์ฃผ์ฐจ๋ณ ํ ๊ธ) ๐ฅ


# ๐ **๋จผ์  ์ฝ์ด๋ณผ ๋งํฌ ์ ๋ฆฌ**

- **`๋จผ์  ์ฝ์ด๋ณผ ๋งํฌ ์ ๋ฆฌ ์๋๋ค.`  ๊ผญ ์ฝ์ด๋ณด์ธ์ !!!**
    - ๊ฐ์ฅ ์ค์ํ ๋ฐ์ดํฐ๋ ์ด๋ฏธ ๋ด๋ถ์ ์๋ค
    
    [https://publy.co/content/433?utm_source=brunch_minu&utm_medium=brunch_minu&utm_content=data_sql](https://publy.co/content/433?utm_source=brunch_minu&utm_medium=brunch_minu&utm_content=data_sql) 
    
    - ๋ฐ์ดํฐ ๋ถ์, SQL๋ง ์ ๋ค๋ค๋ ๋จน๊ณ  ๋ค์ด๊ฐ๋๋ค.
    
    [https://brunch.co.kr/@minu-log/4](https://brunch.co.kr/@minu-log/4)
    
    - ๋ฐ์ดํฐ ๋ถ์, ๋จน๊ณ  ๋ค์ด๊ฐ๊ธฐ ์ํ SQL ๊ณต๋ถ๋ฒ(1ํธ)
    
    [https://brunch.co.kr/@minu-log/5](https://brunch.co.kr/@minu-log/5)ใฑ
    

---

---

## 1. SQL ๊ณต๋ถ์์ (์์จ ๊ณต๋ถ(์ค์ ๋ฌธ์ ) -  (repo / ํด์ฆ / ํ ๋ก )

- ๊นํ ๋ ํฌ ์ ๋ฆฌ reference
    
    [https://github.com/JessyMin/SQL-recipe-book-practice/blob/master/11_finding_patterns_of_users.md](https://github.com/JessyMin/SQL-recipe-book-practice/blob/master/11_finding_patterns_of_users.md)
    

## 2. ๊นํ ๋ ํฌ ์ ๋ฆฌ์ ์ฐธ๊ณ  ์ฌํญ

- ์ถ๊ฐ์ ์ผ๋ก '๋ฏธ๋' ํฌํธํด๋ฆฌ์ค๋ฅผ ์ ์ํฉ๋๋ค. (Code_์๋จ์ ๋์ ์ถ์ฒ)
    
    ๋ฌธ์ ์ ๊ธฐ :  ex ) ์นดํ๊ณ ๋ฆฌ๋ณ๋ก ๋งค์ถ๊ณผ ์๊ณ๋ฅผ ๊ณ์ฐํ๊ธฐ 
    ๋ด๊ฐ ํ๊ณ ์ ํ๋ ๋ฐฉํฅ :
    ~~~~์ค์  ์ฌ์ฉํ ์ฟผ๋ฆฌ :
    ๋ถ์ ๊ฒฐ๊ณผ :
    ~~~~์ธ์ฌ์ดํธ :
    

```sql
-- ์ฐ์ตํ  DB๋ ์ ๊ณตํด ๋๋ฆฝ๋๋ค. 
-- ํด์ฆ๋ ๋ค๋ฅธ DB๋ฅผ ํตํด ์งํํฉ๋๋ค. 
```
## 1.  3์ฃผ์ฐจ ๊น์ง ๊ฐ์ SQL์ ์ด๋์ ์ด๋ป๊ฒ ๋ฐฐ์ฐ๋  ์๊ด์์ต๋๋ค.

<aside>
๐ ์๋ ๋งํฌ๋ ๋ฐฐ์ธ๋งํ ๊ณณ ์๋๋ค. LINK

</aside>

- ๋ฐฐ์ธ๋งํ ๋งํฌ 3๊ฐ
    
    > Introduction to SQL for Data Science (DataCamp)
    > 
    - [https://www.datacamp.com/courses/introduction-to-sql](https://www.datacamp.com/courses/introduction-to-sql)
    
    > SQL Fundamentals (SOLOLEARN)
    > 
    - [https://www.sololearn.com/learning/1060](https://www.sololearn.com/learning/1060)
    
    > The SQL Tutorial for Data Analysis (Mode Analytics)
    > 
    - [https://mode.com/sql-tutorial/introduction-to-sql/](https://mode.com/sql-tutorial/introduction-to-sql/)
- +  ์ถ๊ฐ ๋ฐฐ์ธ๋งํ ๋ชฉ๋ก 3๊ฐ (^^)
    
    **(1) ๋ฌธ๋ฒ ์์ฒด ๊ณต๋ถํ๊ธฐ: ์ฑ ๋๋ ์จ๋ผ์ธ ๊ฐ์ข.**
    
    ๋ฌธ๋ฒ ์์ฒด๋ฅผ ๊ณต๋ถํ๊ธฐ์๋ ์ฑ ๋๋ ์จ๋ผ์ธ ๊ฐ์ข๊ฐ ์ข์ต๋๋ค. ๋ค๋ง ์์์ ๋ง์๋๋ฆฐ ๊ฒ์ฒ๋ผ '์์ง๋์ด'๋ฅผ ์ํ ์ฑ๊ณผ ๊ฐ์ข๊ฐ ๋ค์์ธ ๋งํผ, ์๋ฃ๋ฅผ ์ ๊ฑธ๋ฌ๋ด๋ ๊ฒ๋ง ํด๋ ๋ถ๋ด์ค๋ฌ์ด ์ผ์๋๋ค. ๊ทธ๋์ ์ธ๋งํ ์๋ฃ๋ฅผ ๋ช ๊ฐ ์ถ์ฒํด ๋๊ฒ ์ต๋๋ค.
    
    ### **1) ์ฑ:ย [์นผํด์กฑ ๊น๋๋ฆฌ๋ ์๊ณ  ๋๋ง ๋ชจ๋ฅด๋ SQL ๊ธฐ์ดํธ](http://book.naver.com/bookdb/book_detail.nhn?bid=8073252)**
    
    ์ด์  ๊ธ์์ ์ธ๊ธํ๋ ์์นด(SOCAR)์ ๋ง์ผํฐ ๋ช ๋ถ์ ์ด ์ฑ์ผ๋ก SQL ๊ธฐ๋ณธ ๋ฌธ๋ฒ์ ๊ณต๋ถํ์ต๋๋ค. (์์นด์์๋ ๊ฑฐ์ ๋ฒ ์คํธ์๋ฌ ๊ธ์ด์์ต๋๋ค. ์ฌ๋ฌด์ค์ ์ด ์ฑ๋ง 6๊ถ ์ด์ ์์์ต๋๋ค.)'๊ธฐ์ดํธ'์ด ์์ผ๋ '์ค๊ธํธ', '์ฌํํธ'๋ ์์ด์ผ ํ  ๊ฒ๋ง ๊ฐ์ง๋ง, ํ์์์ ๋์ค์ง ์์์ต๋๋ค.
    
    [https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/16ei/image/v7H1gejrOIcMl9DKnGecigOuxHY.png](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/16ei/image/v7H1gejrOIcMl9DKnGecigOuxHY.png)
    
    - ์ฅ์ :ย **๋ช ์ ๋๋ ์ฐ๋ฆฌ๋ง ์๋ฃ์๋๋ค.**ย ๋ง์ง๋ง ํ ์ฑํฐ๋ฅผ ์ ์ธํ๋ฉด ๋ถ์๊ณผ ๋ฌด๊ดํ ๋ด์ฉ์ด ์์ต๋๋ค.
    - ๋จ์ : ์๋ ์ด๊ฑฐํ  ์จ๋ผ์ธ ๊ฐ์ข์ ๋ฌ๋ฆฌ, ์ค์ต์ ์ํ ํ๊ฒฝ์ ์ ๊ณตํ์ง๋ ์์ต๋๋ค.
    
    ### **2) ๋ฌด๋ฃ ์จ๋ผ์ธ ๊ฐ์ข(์ด๊ธ):ย [Introduction to SQL for Data Science (DataCamp)](https://www.datacamp.com/courses/intro-to-sql-for-data-science)**
    
    - 2019๋ 3์ ๊ธฐ์ค์ผ๋ก, ์ด ๊ฐ์๋ ์ ๋ฃ ๊ฐ์์๋๋ค. ํ์ง๋ง ์ ์ฝ 30๋ฌ๋ฌ ์ ๋์ ๊ฐ๊ฒฉ์ผ๋ก ๋ฐ์ดํฐ์บ ํ์ ๋ชจ๋  ๊ฐ์๋ฅผ ์๊ฐํ์ค ์ ์์ผ๋, ํฐ ๋ถ๋ด์ ์๋์ค ๊ฑฐ๋ผ ์๊ฐํฉ๋๋ค. :)
    
    ๋ฐ์ดํฐ ๋ถ์์ ์ํ ๊ฐ์ข ๊ฐ์ข๋ค์ด ๋ชจ์ฌ ์๋ DataCamp์ SQL ๊ฐ์ข์๋๋ค. SELECT, filtering, grouping, sorting ๋ฑ ๊ธฐ์ด์ ์ธ ๋ด์ฉ์ ๋ค๋ฃน๋๋ค. ํ์ ๊ฐ์์ ํด์ผ ์ด์ฉํ  ์ ์์ง๋ง, ๋ฌด๋ฃ ๊ฐ์ข์๋๋ค.
    
    [https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/16ei/image/Q2FA97DavptlMIoAAQqC5qXGLTM.png](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/16ei/image/Q2FA97DavptlMIoAAQqC5qXGLTM.png)
    
    ์ด๋ฐ ์์ผ๋ก ๋ฐฐ์ด ๋ด์ฉ์ ์น๋ธ๋ผ์ฐ์ ์์ ๋ฐ๋ก ์ค์ตํ  ์ ์๋ ํ๊ฒฝ์ ์ ๊ณตํฉ๋๋ค.
    
    - ์ฅ์ : ๋ฏฟ๊ณ  ๋ณด๋ DataCamp ๊ฐ์ข์๋๋ค. DataCamp๋ ์จ๋ผ์ธ ๊ฐ์ข ์ ๋ฌธ ์์ฒด์ด๊ธฐ ๋๋ฌธ์, ํ์ต์ ์ํ ์ธํฐํ์ด์ค๊ฐ ์ ๋ง๋ค์ด์ ธ ์์ต๋๋ค. ์ฐจ๊ทผ์ฐจ๊ทผ ์ค์ต์ ํ๋ฉฐ ํจ๊ป ๊ฐ์๋ฅผ ๋ฐ๋ผ๊ฐ๋ค ๋ณด๋ฉด ๊ธฐ๋ณธ SQL ๋ฌธ๋ฒ์ ์ตํ ์ ์์ต๋๋ค.
    - ๋จ์ : ๊ตณ์ด ๊ผฝ์๋ฉด, ์์ด ๊ฐ์ข๋ผ๋ ์ ? (๊ทธ๋ ์ง๋ง ๊ธฐ์ ์ ์ธ ๋ด์ฉ์ ๊ณต๋ถํ  ๋ ์์ด๋ ํ์์๋๋ค.)
    
    ### **3) ๋ฌด๋ฃ ์จ๋ผ์ธ ๊ฐ์ข(์ด๊ธ~๊ณ ๊ธ):ย [The SQL Tutorial for Data Analysis (Mode Analytics)](https://community.modeanalytics.com/sql/tutorial/introduction-to-sql/)**
    
    ๋ฐ์ดํฐ ๋ถ์ ์๋ฃจ์ ์์ฒด์ธ Mode Analytics์์ ์ ๊ณตํ๋ ๊ฐ์ข์๋๋ค. ์ค์ ๋ก ์ค์ต์ ํ๋ ค๋ฉด ๊ณ์ ์ ๋ง๋ค์ด์ผ ํ์ง๋ง, ๊ฐ์ข ์์ฒด๋ ๋ก๊ทธ์ธ ์์ด ๋ณผ ์ ์์ต๋๋ค.
    
    [https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/16ei/image/59TmoZxcjyWSRgh75NiuLTixhc0.png](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/16ei/image/59TmoZxcjyWSRgh75NiuLTixhc0.png)
    
    - ์ฅ์ : ์ด๊ธ, ์ค๊ธ, ๊ณ ๊ธ ์์ค๋ณ๋ก SQL ๋ฌธ๋ฒ์ ์ ์ค๋ชํด ๋์์ต๋๋ค. ์ฌ๊ธฐ์ ์๋ ๋ด์ฉ๋ง ์ ๊ณต๋ถํด ๋๋ฉด ์ด๋ ๊ฐ์ SQL ์ข ํ๋ค๊ณ  ์๋ํ  ์ ์์ต๋๋ค.
    - ๋จ์ :ย ๊ฐ์ข ์ ๋ฌธ ์๋น์ค๊ฐ ์๋๋, DataCamp์ฒ๋ผ ์น์ ํ ์ธํฐํ์ด์ค๋ฅผ ๊ธฐ๋ํ๊ธฐ๋ ์ด๋ ต์ต๋๋ค. ๊ทธ๋ฆฌ๊ณ  ์์ ๋ค์ ์๋น์ค(Mode Analytics)๋ฅผ ํ๋ณดํ๋ ๋ชฉ์ ๋ ์์ด์, ์ค๊ฐ์ค๊ฐ Mode Analytics์๋ง ์ ์ฉ๋๋ ๋ด์ฉ์ด ๋์ค๊ธฐ๋ ํฉ๋๋ค. ๊ทธ๋๋ ํฌ๊ฒ ๊ฑฐ์ฌ๋ฆฌ๊ฑฐ๋ ํ์ต์ ๋ฐฉํด๋  ์ ๋๋ ์๋๋๋ค.
- ์ธํ๋ฐ - [SQL-์ธ์ ์-๊ณต๊ฒฉ-๊ธฐ๋ณธ-๋ฌธ๋ฒ](https://www.inflearn.com/course/SQL-%EC%9D%B8%EC%A0%9D%EC%85%98-%EA%B3%B5%EA%B2%A9-%EA%B8%B0%EB%B3%B8-%EB%AC%B8%EB%B2%95#reviews)
    
    [https://www.inflearn.com/course/SQL-์ธ์ ์-๊ณต๊ฒฉ-๊ธฐ๋ณธ-๋ฌธ๋ฒ#reviews](https://www.inflearn.com/course/SQL-%EC%9D%B8%EC%A0%9D%EC%85%98-%EA%B3%B5%EA%B2%A9-%EA%B8%B0%EB%B3%B8-%EB%AC%B8%EB%B2%95#reviews)
    

---

## 2. **๊ฐ์ ๊ณต๋ถ๋ฅผ ์งํํ๊ณ  ํด์ฆ๋ฅผ ์งํํฉ๋๋ค.** + ํ ,์ผ ์ค ๋ชจ์ฌ ํผ๋๋ฐฑ & ์๋ผ

ํด์ฆ๋ ๊ธ์์ผ์ ์ฌ๋ ค๋๋ฆฝ๋๋ค.  /  ์ผ์์ผ์ ์ ๋ต์ ์ฌ๋ ค๋๋ฆฝ๋๋ค.(๊ทธ ํ ์นดํก๋ฐฉ์ ๊ณต์งํด ๋๋ฆฝ๋๋ค ! ) 

```sql
ํผ๋๋ฐฑ ์์ -- (๋จ์ ์์์๋๋ค. ๋ฐ๊พธ์๋ ๋ฉ๋๋ค.)
=>  ๊ฐ๋์ฑ์ด ์ข์๊ฐ? 
=>  ์ข์ง ์๋ค๋ฉด ๋ฌด์์ ๋ ์ถ๊ฐํด์ผ ํ๋๊ฐ? ์ด๋ป๊ฒ ์์ฑํด์ผ ํ๋๊ฐ?
=>  ๋ ์ข์ ๋ฐฉ์์ด ์๋๊ฐ?
=>  ๋ ์ฝ๊ฒ ํธ๋ ๋ฐฉ๋ฒ์ด ์๋๊ฐ?
=>  ๋ต์ ์ณ๊ฒ ๋์๋๊ฐ? ๋ฑ๋ฑ                                                
```

---

# ๐ย ์ค์ตํ๊ฒฝ Setting

- ์ค์ต ํ๊ฒฝ ์ธํ ์๊ฐ - dbeaver ์ด์ฉ ( ERD ํฌํจ) โ ์ฌ๊ธฐ์ ์ธํํ์๋ฉด ๋ฉ๋๋ค
    
    [์ํ db [https://www.postgresqltutorial.com/postgresql-sample-database/](https://www.postgresqltutorial.com/postgresql-sample-database/)](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled.zip)
    
    ์ํ db [https://www.postgresqltutorial.com/postgresql-sample-database/](https://www.postgresqltutorial.com/postgresql-sample-database/)
    
    ![S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled.png](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled.png)
    
    SQL ์ฟผ๋ฆฌ Tool [https://dbeaver.io/download/](https://dbeaver.io/download/)
    
    ์ค์น ์ฐธ๊ณ  ์ฌ์ดํธ
    
    Window : https://3rdscholar.tistory.com/37
    Mac : https://jybaek.tistory.com/858
    
    
    
    ![ERD ](S%20Q%20L%20652a8da88475490c9583cc1c305da234/dvd-rental-sample-database-diagram.png)
    
    ERD 
    
- ์ค์ต ํ๊ฒฝ ์ธํ ์๊ฐ - Mysql ์ด์ฉ์ - (03๊ธฐ ๊น์ ๊ธฐ ๋ ver 1.0)
    
    [https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/) - mysql ์ค์น ํ์ด์ง
    
    ![mysql community server์์ ๋ค์ด๋ฐ๋๋ค.(ํจํค์ง)](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%204.png)
    
    mysql community server์์ ๋ค์ด๋ฐ๋๋ค.(ํจํค์ง)
    
    ![์ฉ๋์ด ๋ง์ ํญ๋ชฉ ํด๋ฆญํด์ ๋ค์ด๋ฐ๋๋ค.](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%205.png)
    
    ์ฉ๋์ด ๋ง์ ํญ๋ชฉ ํด๋ฆญํด์ ๋ค์ด๋ฐ๋๋ค.
    
    ![Developer Default ์ ํ](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%206.png)
    
    Developer Default ์ ํ
    
    ![next](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%207.png)
    
    next
    
    ![installation์ด ๋ค์๊ณผ ๊ฐ์ด ๋ชฉ๋ก์ด ๋์ค๋ฉด ๋๋ค.](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%208.png)
    
    installation์ด ๋ค์๊ณผ ๊ฐ์ด ๋ชฉ๋ก์ด ๋์ค๋ฉด ๋๋ค.
    
    ![๋คํธ์ํฌ๋ ๊ธฐ๋ณธ ์ค์ ์ผ๋ก ์งํ](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%209.png)
    
    ๋คํธ์ํฌ๋ ๊ธฐ๋ณธ ์ค์ ์ผ๋ก ์งํ
    
    ![root ๋น๋ฐ๋ฒํธ ์ค์  ์ํ๋ ๋น๋ฐ๋ฒํธ๋ก ์ค์ ํด์ค๋ค.](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2010.png)
    
    root ๋น๋ฐ๋ฒํธ ์ค์  ์ํ๋ ๋น๋ฐ๋ฒํธ๋ก ์ค์ ํด์ค๋ค.
    
    ![์ค์ ํ ๋น๋ฐ๋ฒํธ ์๋ ฅํ check](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2011.png)
    
    ์ค์ ํ ๋น๋ฐ๋ฒํธ ์๋ ฅํ check
    
    ![์ดํ ์คํ์ ์งํํ๋ฉด workbench์ ํจ๊ป shell์ด ์คํ๋๋ฉด ์ ์์๋.](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2012.png)
    
    ์ดํ ์คํ์ ์งํํ๋ฉด workbench์ ํจ๊ป shell์ด ์คํ๋๋ฉด ์ ์์๋.
    
    ![workbench main ํ๋ฉด์ด script์ ํจ๊ป ๋์จ๋ค.](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2013.png)
    
    workbench main ํ๋ฉด์ด script์ ํจ๊ป ๋์จ๋ค.
    
    ![use sakila ๋ช๋ น์ด๋ก ๊ธฐ์กด ๊น๋ ค์๋ sakila sample db๋ก ์ ๊ทผํด์ค๋ค.](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2014.png)
    
    use sakila ๋ช๋ น์ด๋ก ๊ธฐ์กด ๊น๋ ค์๋ sakila sample db๋ก ์ ๊ทผํด์ค๋ค.
    
    ![์ฟผ๋ฆฌ๋ฌธ์ ์๋ ฅํด์ฃผ๊ณ  ์ ์ ์๋ํ์ธ](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2015.png)
    
    ์ฟผ๋ฆฌ๋ฌธ์ ์๋ ฅํด์ฃผ๊ณ  ์ ์ ์๋ํ์ธ
    
    EER Diagram์ ํ์ธํ๊ณ  ์ถ์๋
    
    ![Reverse Engineer ์ ํ](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2016.png)
    
    Reverse Engineer ์ ํ
    
    ![Next](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2017.png)
    
    Next
    
    ![์ํ๋ db ์ ํ](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2018.png)
    
    ์ํ๋ db ์ ํ
    
    ![excute ์งํ](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2019.png)
    
    excute ์งํ
    
    ![๋ค์ด์ด๊ทธ๋จ ํ์ธ ๊ฐ๋ฅ](S%20Q%20L%20652a8da88475490c9583cc1c305da234/Untitled%2020.png)
    
    ๋ค์ด์ด๊ทธ๋จ ํ์ธ ๊ฐ๋ฅ
    
- DBEAVER๋จ์ถํค
    
    [https://meyouus.tistory.com/52](https://meyouus.tistory.com/52)
    
    
- ์ถ๊ฐ๋ฌธ์ 
    1. ํด์ปค๋ญํฌ ์ญ ํ์ด๋ณด์๋ฉด ๋ฉ๋๋ค ! 
    
    [https://www.hackerrank.com/domains/sql](https://www.hackerrank.com/domains/sql)
    
    1. Datacamp๋ ์ ๋ฃ,,,, 

---

---

- ์ฟผ๋ฆฌ๋ฌธ (์ด์๊ฒ) ์ ๋ ฌ ํด์ฃผ๋ ์ฌ์ดํธ - Format SQL click
    
    [https://www.dpriver.com/pp/sqlformat.htm](https://www.dpriver.com/pp/sqlformat.htm)
    

# ์ฐธ๊ณ ์๋ฃ

- ์ฐธ๊ณ ์๋ฃ
    
    [https://www.slideshare.net/zzsza/bigquery-147073606](https://www.slideshare.net/zzsza/bigquery-147073606)
    
    - ๊ฐ์ฅ ์ค์ํ ๋ฐ์ดํฐ๋ ์ด๋ฏธ ๋ด๋ถ์ ์๋ค
    
    [https://publy.co/content/433?utm_source=brunch_minu&utm_medium=brunch_minu&utm_content=data_sql](https://publy.co/content/433?utm_source=brunch_minu&utm_medium=brunch_minu&utm_content=data_sql) 
    
    - ๋ฐ์ดํฐ ๋ถ์, SQL๋ง ์ ๋ค๋ค๋ ๋จน๊ณ  ๋ค์ด๊ฐ๋๋ค.
    
    [https://brunch.co.kr/@minu-log/4](https://brunch.co.kr/@minu-log/4)
    
    - ๋ฐ์ดํฐ ๋ถ์, ๋จน๊ณ  ๋ค์ด๊ฐ๊ธฐ ์ํ SQL ๊ณต๋ถ๋ฒ(1ํธ)
    
    [https://brunch.co.kr/@minu-log/5](https://brunch.co.kr/@minu-log/5)
    

---

---

- SQL ๋ฐฐ์ฐ๊ธฐ
    - [SQL ๋ ๋ฒจ์](http://www.yes24.com/24/goods/24089836?scode=032&OzSrank=2)ย : SQL ๊ธฐ์ด ๊ณต๋ถ
    - [๋ฐ์ดํฐ ๋ถ์์ ์ํ SQL ๋ ์ํผ](http://www.yes24.com/24/goods/59411396?scode=032&OzSrank=1)ย : SQL ์ฌํ ๊ณต๋ถ!  ์ค๋ฌด์์ ํ์ฉํ ๋ด์ฉ ๊ฐ๋ (์ด๋ผ๊ณ  ํ์๋ค์)

[[แแฎแแญแแฅแซแแกแแขแจ] แแตแแงแผแแฉแแด SQL+SQLD แแตแแตแฏแแฉแแณ.pdf](S%20Q%20L%20652a8da88475490c9583cc1c305da234/%E1%84%86%E1%85%AE%E1%84%85%E1%85%AD%E1%84%8C%E1%85%A5%E1%86%AB%E1%84%8C%E1%85%A1%E1%84%8E%E1%85%A2%E1%86%A8_%E1%84%8B%E1%85%B5%E1%84%80%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A9%E1%84%8B%E1%85%B4_SQLSQLD_%E1%84%87%E1%85%B5%E1%84%86%E1%85%B5%E1%86%AF%E1%84%82%E1%85%A9%E1%84%90%E1%85%B3.pdf)
