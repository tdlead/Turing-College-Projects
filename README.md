# dtatia-ABP.3.3

# Olist Marketplace Review And Delivery Analysis

## Objective 
The objective here is to delve into the impact of delivery delays on customer reviews within the Olist marketplace. We're essentially looking to understand the correlation between the timeliness of deliveries and the subsequent reviews left by customers.

To achieve this, one approach could be to gather data on orders that experienced delays in delivery and then compare the corresponding customer reviews. By identifying patterns or trends, we can gain insights into whether prolonged delivery times have a notable influence on the overall satisfaction and feedback from Olist customers.

As a data visualization tool was used - Power BI, it is not sharebable. 
## Presentation 
https://docs.google.com/presentation/d/1E2OJjRkdkenIeL0dt_zYUvNdGt4ffxEd_v5Rlo1NoKs/edit?usp=sharing 

## SQL extract
Reviews and Delivery Data
```SQL
WITH
  orders AS (
  SELECT
    orders.order_id,
    customer_id,
    order_status,
    order_purchase_timestamp,
    order_approved_at,
    order_delivered_carrier_date,
    order_delivered_customer_date,
    order_estimated_delivery_date,
    DATETIME_DIFF(order_approved_at, order_purchase_timestamp, day) AS hours_to_approve,
    DATETIME_DIFF(order_delivered_carrier_date, order_approved_at, day) AS days_to_carrier,
    DATETIME_DIFF(order_delivered_customer_date, order_delivered_carrier_date, day) AS days_carrier_customers,
    DATETIME_DIFF(order_delivered_customer_date, order_purchase_timestamp, day) AS days_to_deliver,
    DATETIME_DIFF(order_estimated_delivery_date,order_purchase_timestamp, day) AS estimated_days,
    DATETIME_DIFF(order_delivered_customer_date,order_estimated_delivery_date, day) AS difference_days
  FROM
    `tc-da-1.olist_db.olist_orders_dataset` orders),
  reviews_sort AS (
  SELECT
    *,
    RANK() OVER (PARTITION BY order_id ORDER BY review_creation_date DESC, review_answer_timestamp DESC) AS reviews_rank
  FROM
    `olist_db.olist_order_reviews_dataset` reviews),
  reviews AS (
  SELECT
    *
  FROM
    reviews_sort
  WHERE
    reviews_rank=1 ),
  revenue AS (
  SELECT
    order_id,
    SUM(price) AS price,
    SUM(freight_value) AS freight_value
  FROM
    `tc-da-1.olist_db.olist_order_items_dataset`
  GROUP BY
    order_id)
    
    SELECT
  orders.order_id,
  customer_unique_id as customer_id,
  order_status,
  order_purchase_timestamp,
  order_approved_at,
  order_delivered_carrier_date,
  order_delivered_customer_date,
  order_estimated_delivery_date,
  hours_to_approve,
  days_to_carrier,
  days_carrier_customers,
  days_to_deliver,
  estimated_days,
  difference_days,
  review_id,
  review_score,
  review_creation_date,
  review_answer_timestamp,
  review_comment_message_length,
  revenue.price,
  revenue.freight_value,
  (price+freight_value) as total,
  if(difference_days>0, 1,0) is_delay
FROM
  orders
JOIN
  revenue
ON
  orders.order_id = revenue.order_id
  join `olist_db.olist_customesr_dataset` customers 
  on customers.customer_id = orders.customer_id
LEFT JOIN
  reviews
ON
  orders.order_id = reviews.order_id
WHERE
  order_delivered_customer_date IS NOT NULL
  AND order_status ='delivered'
```

Orders and Geolocation And Distance Calculation
```SQL
CREATE TEMP FUNCTION radians(value FLOAT64) AS (
    value * (3.141592653589793 / 180)
);

CREATE TEMP FUNCTION haversine_distance(
    lat1 FLOAT64,
    lon1 FLOAT64,
    lat2 FLOAT64,
    lon2 FLOAT64
) AS (
    6371 * ACOS(
        GREATEST(-1, LEAST(
            COS(radians(lat1)) * COS(radians(lat2)) * COS(radians(lon2 - lon1)) +
            SIN(radians(lat1)) * SIN(radians(lat2)), 1))
    )
);

with geolocations as (select distinct geolocation_zip_code_prefix, 	
min(geolocation_state) as state,min(geolocation_city) as city , min(geolocation_lat) as geolocation_lat, min(geolocation_lng) geolocation_lng

from `tc-da-1.olist_db.olist_geolocation_dataset` geolocation 
group by geolocation_zip_code_prefix),

items as (
    select order_id,seller_id from `olist_db.olist_order_items_dataset` items 
group by order_id,seller_id 
),

table_1 as (SELECT 
orders.order_id, 
orders.customer_id, 
items.seller_id, 
--product_category_name, 
--customer_zip_code_prefix,	
--seller_zip_code_prefix, 
customergeo.state as customer_state,
customergeo.city as customer_city,
--customergeo.geolocation_lat as customer_lat,
--customergeo.geolocation_lng as customer_lng,
sellergeo.state as seller_state,
sellergeo.city as seller_city,
--sellergeo.geolocation_lat as seller_lat,
--sellergeo.geolocation_lng as seller_lng,
haversine_distance(customergeo.geolocation_lat,customergeo.geolocation_lng,sellergeo.geolocation_lat,sellergeo.geolocation_lng) as distance

FROM `tc-da-1.olist_db.olist_orders_dataset` orders
join items
on orders.order_id = items.order_id
-- join `olist_db.olist_products_dataset` products
-- on products.product_id= items.product_id

join `tc-da-1.olist_db.olist_customesr_dataset` customers
on customers.customer_id = orders.customer_id

join `olist_db.olist_sellers_dataset`sellers
on items.seller_id=sellers.seller_id

left join geolocations customergeo
on customergeo.geolocation_zip_code_prefix= customers.customer_zip_code_prefix

left join geolocations sellergeo
on sellergeo.geolocation_zip_code_prefix= sellers.seller_zip_code_prefix)

select * from table_1

```
