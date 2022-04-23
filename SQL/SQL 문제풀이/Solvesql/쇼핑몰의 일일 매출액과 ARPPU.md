# 쇼핑몰의 일일 매출액과 ARPPU

## **문제 설명**

Brazilian E-Commerce Public Dataset by Olist 데이터셋은 브라질의 이커머스 웹사이트인 Olist Store의 판매 데이터 입니다. 그 중 `olist_orders_dataset` 테이블에는 주문 ID, 고객 ID, 주문 상태, 구매 시각 등 주문 내역 데이터가 들어있습니다. `olist_order_payments_dataset` 테이블에는 주문 ID, 결제 방법, 결제 금액 등 각 주문의 결제와 관련된 정보가 저장되어 있습니다. 두 테이블을 이용해 2018년 1월 1일 이후 일별로 집계된 쇼핑몰의 결제 고객 수, 매출액, ARPPU를 계산하는 쿼리를 작성해주세요.

ARPPU는 Average Revenue Per Paying User의 약자로, 결제 고객 1인 당 평균 결제 금액을 의미합니다. 전체 매출액을 결제 고객 수로 나누면 ARPPU를 계산할 수 있습니다.

주문 각각에 대해 매출이 일어나는 시점은 `olist_orders_dataset` 테이블의 `order_purchase_timestamp` 컬럼에 기록되고, 주문 금액은 `olist_order_payments_dataset` 테이블의 `payment_value` 컬럼에 기록됩니다.

쿼리 결과는 아래 네 개의 컬럼을 포함해야 하고, 매출 날짜 기준으로 오름차순 정렬되어 있어야 합니다. 매출액과 ARPPU는 반올림 해 소수점 둘째자리까지 출력해주세요.

- `dt` - 매출 날짜 (예: 2018-01-01)
- `pu` - 결제 고객 수
- `revenue_daily` - 해당 날짜의 매출액
- `arppu` - 결제 고객 1인 당 평균 결제 금액

---

```sql
-- 필요한 컬럼을 들고옵니다. 날짜와 결제 고객(중복방지),총 결제 금액,(총결제 금액)/(총 결제 고객수)
-- 가입고객으로 하면 -> ARPU

SELECT Date(o.order_purchase_timestamp)                            AS dt,
       Count(DISTINCT o.customer_id)                               AS pu,
       round(Sum(p.payment_value),2)                     As revenue_daily,
       Round(Sum(p.payment_value) / Count(DISTINCT o.customer_id),2) AS arppu
-- 데이터를 합쳐줍니다.
FROM   olist_orders_dataset o
       JOIN olist_order_payments_dataset p
         ON o.order_id = p.order_id
-- '2018-01-01'에 해당하는 고객만 들고옵니다.
WHERE  date(o.order_purchase_timestamp) >= '2018-01-01'
-- 집계함수를 사용했으니 , 사용하지 않은 컬럼으로 그룹화를 해줍니다.
GROUP  BY dt 
-- 순서대로 정렬합니다. 
order by dt
```