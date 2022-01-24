# Week2 - 실습문제 7

### 주문일이 2017-09-02 일에 해당 하는 주문건에 대해서,  어떤 고객이, 어떠한 상품에 대해서 얼마를 지불하여  상품을 구매했는지 확인해주세요

- 얼마를 지불하여 ⇒ 가격 곱하기 수량

```sql
SELECT o.orderdate,
       o.customerid,
       od.productnumber,
       od.quotedprice * od.quantityordered AS prices
FROM   orders AS o
       JOIN order_details AS od
         ON o.ordernumber = od.ordernumber
WHERE  orderdate BETWEEN '2017-09-02' AND '2017-09-02'
ORDER  BY o.orderdate,
          o.customerid
```

```sql
-- 주문일이 2017-09-02 일에 해당 하는 주문건에 대해서,  
-- 어떤 고객이, 어떠한 상품에 대해서 얼마를 지불하여  상품을 구매했는지 확인해주세요

SELECT o.orderdate,
       o.customerid,
       od.productnumber,
       od.quotedprice * od.quantityordered AS Much
FROM   orders o
       JOIN order_details od
         ON od.ordernumber = o.ordernumber
WHERE  orderdate = '2017-09-02'
```

```sql
-- 고객별 해당날짜 얼마나 구입했나 

select 
       o.customerid,
       sum(od.quotedprice * od.quantityordered) as Much
from orders o
join order_details od
on od.ordernumber = o.ordernumber 
where  orderdate = '2017-09-02'
group  by customerid

--------------------------------
1007	69.0000
1001	1283.8500
1003	1492.6000
1012	2607.0000
1024	5544.7500
1009	6601.7300
1014	9820.2900
1002	11912.4500
1018	12751.8500
```

### 헬멧을 주문한 적 없는 고객을 보여주세요.

- 헬맷은, Products 테이블의 productname 컬럼을 이용해서 확인해주세요.

```sql
-- 헬멧을 주문한 고객

select c.customerid,
		p.productname
from customers c 
join orders o on c.customerid = o.customerid
join order_details od on o.ordernumber = od.ordernumber
join products p on p.productnumber = od.productnumber
where p.productname LIKE '%Helmet%'
```

```sql
-- 헬멧을 주문한적 없는 고객

SELECT c.customerid
FROM   customers c
WHERE  NOT EXISTS (SELECT *
                   FROM   orders o
                          INNER JOIN order_details od
                                  ON o.ordernumber = od.ordernumber
                          INNER JOIN products p
                                  ON od.productnumber = p.productnumber
                   WHERE  o.customerid = c.customerid
                          AND p.productname LIKE '%Helmet')

-- 자동으로 orderby 됨
```

```sql
-- 다른 답안
-- 서브쿼리로 테이블 3개를 합치고 헬멧을 뽑은 사람만 뽑아낸뒤 
-- customers table에 left outer join 한다.

SELECT c.customerid
FROM   customers c
       LEFT JOIN (SELECT *
                  FROM   orders o
                         JOIN order_details od
                           ON o.ordernumber = od.ordernumber
                         JOIN products p
                           ON p.productnumber = od.productnumber
                  WHERE  p.productname LIKE '%Helmet%') h
              ON c.customerid = h.customerid
WHERE  h.productname IS NULL
```

```sql
-- 이건 왜 안될까 ? 

SELECT c.customerid
FROM   customers c
WHERE  NOT EXISTS (select *
										from orders o
												join order_details od 
													on o.ordernumber = od.ordernumber
												join products p 
													on p.productnumber = od.productnumber -- 테이블 3개 이너조인
												where p.productname LIKE '%Helmet%');
	

-- c.customerid 와 o.customerid 모두 25개의 헬멧구매자가 있다는건 확인완료
-- 구매하지 않은사람 3명 
```

- `o.customerid = c.customerid AND 구문`이 빠져있는데
    - 서브쿼리에서 일단 orders 에서 products까지 테이블을 다 합쳤고 (orders 테이블에 있는 정보를 기준으로)
- `o.customerid = c.customerid AND 구문`을 빼먹고 쓴 서브-쿼리문은 총 307건의 겹치는 모든 ID를 보여준다. (헬멧을 주문한 모든 건수)
    - distint 해보니 아이디 25건 조회가능
- 거기서 `EXISTS` 를 사용하면 (not 없이) 28건이 다 조회되는데, `EXISTS` 는 테이블에 데이터가 한건이라도 존재하면 다 return 시키기 때문이다.
    - EXISTS는 조건이 맞는 지에 대한 TRUE / FALSE만 확인하기 때문이다
    - 만족하는 결과가 최소 하나가 나오면 바로 TRUE로 판단
    - 해당 서브쿼리에서 값이 존재하는지 확인
    - **IN (Subquery)는 속도가 느리기 때문에 EXISTS 또는 JOIN을 활용하는 편이 낫다고 한다.**
- 고로 28건이 서브쿼리에 다 존재하니까  EXISTS 를 사용하면 ⇒ 서브쿼리에 있는 25개 포함 다 리턴됨
    - **The EXISTS operator is used to test for the existence of any record in a subquery**
- 그럼 NOT EXISTS는? ⇒ 하나도 리턴안함