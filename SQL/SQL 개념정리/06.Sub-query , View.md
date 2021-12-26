# 6. Sub-query , 뷰

- 서브쿼리
    - 집합연산자와 서브쿼리 = ANY연산
    - 집합연산자와 서브쿼리 = ALL연산
    - 집합연산자와 서브쿼리 = EXISTS연산
    

![Untitled](6%20Sub-query%20,%20%E1%84%87%E1%85%B2%20946192b965f843279b04b934a4eb2cb8/Untitled.png)

### 📌 Sub-query란 ?

- 하나의 SQL 문에 포함되어 있는 또 다른 SQL 문
    - SELECT문 내의 또다른 SELECT문 같은거
    

### 📌 Sub-query 사용시 주의사항

- 서브쿼리를 괄호로 감싸서 사용한다.
- 서브쿼리는 단일 행 또는 복수 행 비교 연산자와 함께 사용 가능하다.
- 서브쿼리에서는 ORDER BY 를 사용하지 못한다. ㅠㅠ

### 📌 Sub-query 가 사용 가능한 곳

- SELECT 절 FROM 절 WHERE 절
- HAVING 절 ORDER BY 절 - Groupby X
- INSERT 문의 VALUES 절
- UPDATE 문의 SET 절

### 📌 Sub-query의 예시

```sql
-- '이순신' 의 데이터를 가져오되 그 부서의 평균 연봉을 가져오라.

-- 출처: https://aljjabaegi.tistory.com/14 [알짜배기 프로그래머]

SELECT T1.*,
       (SELECT Avg(salary)
        FROM   amt_mst_test S1
        WHERE  S1.dept_cd = T1.dept_cd) AS AVG_SALARY
FROM   amt_mst_test T1
WHERE  T1.emp_nm = '이순신'

```

![Untitled](6%20Sub-query%20,%20%E1%84%87%E1%85%B2%20946192b965f843279b04b934a4eb2cb8/Untitled%201.png)

- 위에서 AVG라는 그룹 함수를 사용하는 경우 결과값이 1건이기 때문이 단일 행 서브쿼리로써 사용 가능
- 서브쿼리가 **단일 행 비교 연산자(=, <, <=, >, >=, <>)와 함께 사용할 때**는 서브쿼리의 결과 건수가 반드시 1건 or 0건 이여야 합니다.
    - 만약 결과가 2건 이상인 경우 오류가 발생
    - 2건 이상인 경우 = 이 아닌 IN 을 사용해야 합니다.

```sql
-- '이순신' 의 데이터를 가져오되 그 부서의 평균 연봉을 가져오라.

-- 출처: https://aljjabaegi.tistory.com/14 [알짜배기 프로그래머]

SELECT T1.*,
       (SELECT Avg(salary)
        FROM   amt_mst_test S1
        WHERE  S1.dept_cd = T1.dept_cd) AS AVG_SALARY
FROM   amt_mst_test T1
WHERE  T1.emp_nm IN '이순신'

```

```sql
SELECT c1,
       c2,
       c3
FROM   t1
WHERE  c1 IN (SELECT c1 -- Where 조건문에 IN을 사용한다. 
              FROM   t2
              WHERE  c2 = '3')
```

**🥕 다중 칼럼 서브쿼리**

- 서브쿼리 결과로 여러 개의 컬럼이 반환되어 메인쿼리의 조건과 동시에 비교되는 것을 의미함
    - 서브쿼리의 결과가 여러 칼럼인 경우 다중 컬럼 서브쿼리라고 한다.
- 메인쿼리 WHERE절과 서브쿼리에서 반환하는 컬럼의 수가 반드시 같아야 한다.
- 아래의 예처럼 2개의 컬럼(부서번호, sal의 최솟값)을 반환하는 서브쿼리를 사용할 경우 WHERE절에도 2개의 컬럼명을 적어주고 괄호로 묶어준다.

```sql
-- 형태
SELECT c1,
       c2,
       c3
FROM   t1
WHERE  ( c1, c2 ) IN (SELECT c1,
                             c2
                      FROM   t2 
                      WHERE  c2 = '3') 
-- 괄호는 t2 테이블에서 c2값이 3인 c1c2컬럼을 가져와라 
```

```sql
-- 부서별로 가장 작은 급여(sal)를 받는 직원을 조회
SELECT deptno,
       ename,
       sal
FROM   emp
WHERE  ( deptno, sal ) IN (SELECT deptno,
                                  Min(sal)
                           FROM   emp
                           GROUP  BY deptno)
ORDER  BY deptno;
```

<aside>
💡     DEPTNO ENAME                       SAL
---------- -------------------- ----------
        10 MILLER                     1300
        20 SMITH                       800
        30 JAMES                       950

</aside>

**🥕 연관 서브쿼리**

- 서브쿼리 내에 메인쿼리 컬럼이 사용된 서브쿼리 입니다.
- 메인쿼리의 값을 서브쿼리가 사용하고, 서브쿼리의 값을 받아서 메인쿼리가 계산하는 구조의 쿼리
    - 메인쿼리의 값을 서브쿼리에 주고 서브쿼리를 수행한 다음
    - 그 결과를 다시 메인쿼리로 반환해서 수행하는 쿼리

```sql
SELECT   ename,
         sal,
         deptno
FROM     emp
ORDER BY deptno,
         sal;

ENAME sal deptno
-------------------- ---------- ----------
miller 1300 10 
clark 2450 10 
king 5000 10 
smith 800 20 
adams 1100 20 
jones 2975 20 
scott 3000 20 
ford 3000 20 
james 950 30 
martin 1250 30 
ward 1250 30 
turner 1500 30 
allen 1600 30 
blake 2850 30 
14 개의 행이 선택되었습니다.

------------------------------------------------------------
-- 부서별 평균 sal
SELECT   deptno,
         Avg(sal)
FROM     emp
GROUP BY deptno;

DEPTNO avg(sal)
------------------------------------------------------------
30 1566.66667 
20 2175 
10 2916.66667 

---------------------------------------------------------------------------
SELECT   ename,
         deptno,
         sal 
FROM     emp e1 
WHERE    sal >
         (
                SELECT avg(sal) 
                FROM   emp e2 
                WHERE  e2.deptno=e1.deptno) 
ORDER BY deptno;

ENAME deptno sal

------------------------------------------------------------
king 10 5000 
jones 20 2975 
scott 20 3000 
ford 20 3000 
allen 30 1600 
blake 30 2850 

6 개의 행이 선택되었습니다.
--------------------------------------------------
-- 1) 메인쿼리에서 부서번호(e1.deptno)를 읽어서 서브쿼리로 전달
-- 2) 서브쿼리는 메인쿼리에서 받은 부서번호로 평균 급여 계산
-- 3) 다시 메인쿼리는 서브쿼리의 평균 급여보다 큰 급여의 직원 출력
```

**🥕 FROM 절에 사용하는 서브쿼리**

- 인라인 뷰 라고 합니다
    - **SELECT 절의 결과를 FROM 절에서 하나의 테이블처럼 사용하고 싶을 때 사용**
    - 기존 단일 쿼리로는 '**테이블에서 각 부서별 최대 연봉**' 까지 알 수 있었다면,
    - 서브쿼리를 통해서 **누가 최대 연봉자인지 확인할 수 있게 되었습니다.**
- 기본적으로 FROM 절에는 테이블 명이 오도록 되어있습니다. 그런데 서브쿼리가 FROM 절에 사용되면 동적으로 생성된 테이블인 것처럼 사용할 수 있습니다.
- 인라인 뷰는 SQL 문이 실행될 때만 임시적으로 생성되는 동적인 뷰이기 때문에 데이터베이스에 해당 정보가 저장되지 않습니다.
- 인라인 뷰는 동적으로 조인 방식을 사용하는 것과 같습니다.
    - 

```sql
SELECT t1.c1,
       T2.c1,
       T2.c2
FROM   t1 T1,
       (SELECT c1,
               c2
        FROM   t2) T2
WHERE  t1.c1 = T2.c1;
```

즉, FROM 절에 서브쿼리를 사용하면 특정 조건식을 갖는 SELECT 문을 테이블처럼 사용할 수 있습니다. 

이를 통해 SELECT 문을 효율적이고 간결하게 작성할 수 있습니다. 

이는 마치 가상 테이블, 즉 뷰(view)**[2](https://thebook.io/006977/ch07/05/#footnote-448160-2)**와 같은 역할을 한다고 해서 인라인 뷰(inline view)라고도 부릅니다.

뷰

---

- 테이블은 실제로 데이터를 가지고 있는 반면, 뷰는 실제 데이터를 가지고 있지 않습니다.
- 질의에서 뷰가 사용되면 뷰 정의를 참조해서 **DBMS 내부적으로 질의를 재작성**하여 질의를 수행합니다.
- 뷰는 실제 데이터를 가지고 있지 않지만, 테이블이 수행하는 역할을 수행하기 때문에 **가상 테이블**이라고도 합니다.

🥕 뷰 사용의 장점

뷰(View)의 특징

1. 뷰는 기본테이블로부터 유도된 테이블이기 때문에 기본 테이블과 같은 형태의 구조를 사용하며, 조작도 기본 테이블과 거의 같다.

2. 뷰는 가상 테이블이기 때문에 물리적으로 구현되어 있지 않다.

3. 데이터의 논리적 독립성을 제공할 수 있다.

4. 필요한 데이터만 뷰로 정의해서 처리할 수 있기 때문에 관리가 용이하고 명령문이 간단해진다.

5. 뷰를 통해서만 데이터에 접근하게 하면 뷰에 나타나지 않는 데이터를 안전하게 보호하는 효율적인 기법으로 사용할 수 있다.

6. 기본 테이블의 기본키를 포함한 속성(열) 집합으로 뷰를 구성해야지만 삽입, 삭제, 갱신, 연산이 가능하다.

7. 일단 정의된 뷰는 다른 뷰의 정의에 기초가 될 수 있다.

8. 뷰가 정의된 기본 테이블이나 뷰를 삭제하면 그 테이블이나 뷰를 기초로 정의된 다른 뷰도 자동으로 삭제된다.

 

뷰(View)사용시 장 단점

장점

1. 논리적 데이터 독립성을 제공한다.

2. 동일 데이터에 대해 동시에 여러사용자의 상이한 응용이나 요구를 지원해 준다.

3. 사용자의 데이터관리를 간단하게 해준다.

4. 접근 제어를 통한 자동 보안이 제공된다.

 

단점

1. 독립적인 인덱스를 가질 수 없다.

2. ALTER VIEW문을 사용할 수 없다. 즉 뷰의 정의를 변경할 수 없다.

3. 뷰로 구성된 내용에 대한 삽입, 삭제, 갱신, 연산에 제약이 따른다.

---

---

참고 : [https://mozi.tistory.com/233](https://mozi.tistory.com/233)

[https://data-make.tistory.com/25](https://data-make.tistory.com/25)
