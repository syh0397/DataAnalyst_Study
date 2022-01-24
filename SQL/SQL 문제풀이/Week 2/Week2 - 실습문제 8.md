# Week2 - 실습문제 8

### 모든 제품 과 주문 일자를 나열하세요. (주문되지 않은 제품도 포함해서 보여주세요.)

```sql
-- 모든 제품 과 주문 일자를 나열하세요. (주문되지 않은 제품도 포함해서 보여주세요.)
-- 나의 답
SELECT p.productnumber,
       p.productname,
       o.orderdate
FROM   products p
       LEFT JOIN order_details od
              ON p.productnumber = od.productnumber
       LEFT JOIN orders o
              ON o.ordernumber = od.ordernumber
```

### 캘리포니아 주와 캘리포니아 주가 아닌 STATS 로 구분하여 각 주문량을 알려주세요. (CASE문 사용)

```sql
-- 캘리포니아 주와 캘리포니아 주가 아닌 STATS 로 구분하여 각 주문량을 알려주세요. (CASE문 사용)
-- 캘리포니아 주 확인

select * from customers c where c.custstate = 'CA';

-- CASE 문으로 나누기

select c.customerid,
		case 
		when c.custstate = 'CA' then 'CA_yes'
		else 'CA_no'
		end as CA_YN
from customers c ;

-- 주문량 찾아가려면 Order_Details 까지 가야한다. 

select c.customerid,
		case 
		when c.custstate = 'CA' then 'CA_yes'
		else 'CA_no'
		end as CA_YN,
		o.ordernumber,
		od.quantityordered 
from customers c 
join orders o 
on c.customerid = o.customerid 
join order_details od 
on o.ordernumber = od.ordernumber; 

-- 이게 아닌가? 답안을 보니 ordernumber별로 count를 적용해야 하는것 같다.약간 문제가 애매쓰

SELECT Count(CA_count.ordernumber),
       CA_count.ca_yn
FROM   (SELECT c.customerid,
               CASE
                 WHEN c.custstate = 'CA' THEN 'CA_yes'
                 ELSE 'CA_no'
               END AS CA_YN,
               o.ordernumber,
               od.quantityordered
        FROM   customers c
               JOIN orders o
                 ON c.customerid = o.customerid
               JOIN order_details od
                 ON o.ordernumber = od.ordernumber) AS CA_count
GROUP  BY CA_count.ca_yn
```

### 공급 업체 와 판매 제품 수를 나열하세요. 단 판매 제품수가 2개 이상인 곳만 보여주세요.

```sql
SELECT v.vendname,
       Count(DISTINCT pv.productnumber)
FROM   product_vendors AS pv
       JOIN vendors AS v
         ON pv.vendorid = v.vendorid
GROUP  BY v.vendname
HAVING Count(DISTINCT pv.productnumber) >= 2

```

### 가장 높은 주문 금액을 산 고객은 누구인가요?

- 주문일자별, 고객의 아이디별로, 주문번호, 주문 금액도 함께 알려주세요.

```sql
-- 가장 높은 주문 금액을 산 고객은 누구인가요?
-- 
SELECT c.customerid,
       o.orderdate,
       o.ordernumber,
       od.quotedprice,
       od.quantityordered,
       od.quotedprice * od.quantityordered as Price
FROM   customers c
       JOIN orders o
         ON c.customerid = o.customerid
       JOIN order_details od
         ON o.ordernumber = od.ordernumber
ORDER  BY od.quotedprice DESC; 
-- 1013

-- 근데 customer_id가 겹치니까 총합으로 생각하면 
SELECT c.customerid,
       o.orderdate,
       o.ordernumber,
      sum(od.quotedprice * od.quantityordered) as Price
FROM   customers c
       JOIN orders o
         ON c.customerid = o.customerid
       JOIN order_details od
         ON o.ordernumber = od.ordernumber
group by c.customerid,
		o.orderdate,
		o.ordernumber 
ORDER  BY Price desc
limit 1
---
1017	2017-10-03	165	21674.6300
```

### 주문일자별로, 주문 갯수와, 고객 수를 알려주세요.

- ex) 하루에 한 고객이 주문을 2번이상했다고 가정했을때
    - ⇒ 해당의 경우는 고객수는 1명으로 계산해야합니다.

```sql
SELECT orderdate,
       Count(DISTINCT ordernumber) AS cnt_orders,
       Count(DISTINCT customerid)  AS cnt_customer
FROM   (SELECT o.orderdate,
               o.customerid,
               o.ordernumber,
               od.productnumber,
               od.quotedprice * od.quantityordered AS prices
        FROM   orders AS o
               JOIN order_details AS od
                 ON o.ordernumber = od.ordernumber) AS db
GROUP  BY orderdate
ORDER  BY orderdate ASC
```

```sql
-- ### 주문일자별로, 주문 갯수와, 고객 수를 알려주세요.
-- ex) 하루에 한 고객이 주문을 2번이상했다고 가정했을때
-- ⇒ 해당의 경우는 고객수는 1명으로 계산해야합니다.
SELECT o.orderdate,
       Count(o.ordernumber) AS cnt_ornum,
       Count(o.customerid)  AS cnt_cusid
FROM   orders o
       JOIN order_details od
         ON o.ordernumber = od.ordernumber
GROUP  BY o.orderdate
ORDER  BY orderdate

```

- 뭐가 잘못된걸까 ? ⇒ distinct 생략함 <주의하자>

```sql
-- 맞는 답 

SELECT o.orderdate,
       count(distinct o.ordernumber) AS cnt_ornum,
       Count(distinct o.customerid)  AS cnt_cusid
FROM   orders o
       JOIN order_details od
         ON o.ordernumber = od.ordernumber
GROUP  BY o.orderdate
ORDER  BY orderdate
```

### 타이어와 헬멧을 모두 산적이 있는 고객의 ID 를 알려주세요.

- 타이어와 헬멧에 대해서는 , Products 테이블의 productname 컬럼을 이용해서 확인해주세요.

```sql
SELECT o.customerid
FROM   orders AS o
       JOIN order_details AS od
         ON o.ordernumber = od.ordernumber
       JOIN products AS p
         ON od.productnumber = p.productnumber
WHERE  p.productname LIKE '%Tires%'
INTERSECT
SELECT o.customerid
FROM   orders AS o
       JOIN order_details AS od
         ON o.ordernumber = od.ordernumber
       JOIN products AS p
         ON od.productnumber = p.productnumber
WHERE  p.productname LIKE '%Helmet%'
```

### 타이어는 샀지만, 헬멧을 사지 않은 고객의 ID 를 알려주세요. Except 조건을 사용하여, 풀이 해주세요.

- 타이어, 헬멧에 대해서는, Products 테이블의 productname 컬럼을 이용해서 확인해주세요.

```sql
SELECT o.customerid
FROM   salesordersexample.orders AS o
       JOIN salesordersexample.order_details AS od
         ON o.ordernumber = od.ordernumber
       JOIN salesordersexample.products AS p
         ON od.productnumber = p.productnumber
WHERE  p.productname LIKE '%Tires%'
EXCEPT
SELECT distinct o.customerid
FROM   salesordersexample.orders AS o
       JOIN salesordersexample.order_details AS od
         ON o.ordernumber = od.ordernumber
       JOIN salesordersexample.products AS p
         ON od.productnumber = p.productnumber
WHERE  p.productname LIKE '%Helmet%'
```