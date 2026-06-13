# End-to-End Retail & E-Commerce SQL Project



## 🌍 Project Overview
This project analyzes large-scale transactional data from Target’s Brazilian e-commerce operations.
Using MySQL, the project encompasses the full data lifecycle, from database schema design and data cleaning to complex exploratory data analysis.
The primary objective is to evaluate logistics performance, customer satisfaction, and revenue trends to provide insights that inform strategic business decision-making.

## 🎯Project Objectives 
- Logistics Optimization: Analyze order delivery timelines to identify regional bottlenecks and evaluate the impact of transit times on customer satisfaction.
- Revenue Performance: Identify high-performing product categories and seasonal sales trends to inform inventory and pricing strategies.  
- Customer Insights: Segment the customer base to understand regional purchasing power and correlate payment methods with order value.
- Operational Efficiency: Utilize advanced SQL techniques, including complex joins and window functions, to transform raw transactional data into data-driven recommendations that inform strategic decision-making.

## 🔍 Key Business Questions Answered

- Temporal Trends: What is the order volume growth month-over-month and year-over-year?

- Customer Behavior: At what time of day do Brazilian customers most frequently place their orders (Dawn, Morning, Afternoon, or Night)?

- Geographic Distribution: How are customers distributed across different states and cities in Brazil?

- Revenue Growth: What is the percentage increase in total payment value comparing 2017 to 2018 (January–August period)?

- Logistics Performance: What is the average time between order purchase and delivery, and how does it compare to the estimated delivery date?

- Regional Logistics: Which states experience the highest and lowest average freight costs?

- Financial Insights: How does the preference for payment methods (e.g., credit card, boleto) shift month-over-month?

- Installment Analysis: What is the distribution of orders based on the number of payment installments selected by customers?

## 📄 Dataset Dataset Features
### Customers
| Feature | Description |
|----------|-------------|
| order_id | Unique identifier of the order |
| customer_id | Unique identifier of the customer transaction |
| customer_zip_code_prefix | Postal area code of the customer |
| customer_city | City where the customer is located |
| customer_state | State where the customer is located |

###  Geolocation
| Feature | Description |
|----------|-------------|
| eolocation_zip_code_prefix | Postal area code used for geographic grouping |
| geolocation_state | State where the location is located|
| geolocation_lng | Longitude coordinate of the location |
| geolocation_lat | Latitude coordinate of the location|
| geolocation_city |City where the location is located|


###  order_items
| Feature | Description |
|----------|-------------|
| order_id |  Unique identifier for each order|
| order_item_id | Identifier for each item within an order|
| product_id |  Unique identifier for a product |
| seller_id |Unique identifier for the seller|
| shipping_limit_date |City where the location is located|
| price |Cost of the product (excluding shipping)|
| freight_value |Shipping cost for the item|


### Order_reviews
| Feature | Description |
|----------|-------------|
| review_id | Unique identifier for each review |
| order_id | Identifier of the order associated with the review |
| review_score | Customer rating given to the order |
| review_comment_title | Title of the customer's review |
| review_creation_date | Date when the review was created |
| review_answer_timestamp | Date and time when the review was submitted or recorded |

### orders
| Feature | Description |
|----------|-------------|
| order_id | Unique identifier for each order |
| customer_id | Unique identifier for the customer |
| order_status | Current status of the order |
| order_purchase_timestamp | Date and time when the order was placed |
| order_approved_at | Date and time when the order was approved |
| order_delivered_carrier_date | Date and time when the order was handed to the carrier |
| order_delivered_customer_date | Date and time when the order was delivered to the customer |
| order_estimated_delivery_date | Estimated delivery date for the order |

### payments
| Feature | Description |
|----------|-------------|
| order_id | Unique identifier for each order |
| payment_sequential | Sequence number of the payment for an order |
| payment_type | Method used to pay for the order |
| payment_installments | Number of installments used for the payment |
| payment_value | Total amount paid for the order |

### products
| Feature | Description |
|----------|-------------|
| product_id | Unique identifier for each product |
| product_category | Category to which the product belongs |
| product_name_length | Length of the product name in characters |
| product_description_length | Length of the product description in characters |
| product_photos_qty | Number of photos available for the product |
| product_weight_g | Weight of the product in grams |
| product_length_cm | Length of the product in centimeters |
| product_height_cm | Height of the product in centimeters |
| product_width_cm | Width of the product in centimeters |

### Sellers
| Feature | Description |
|----------|-------------|
| seller_id | Unique identifier for each seller |
| seller_zip_code_prefix | ZIP code prefix of the seller's location |
| seller_city | City where the seller is located |
| seller_state | State where the seller is located |

### 🛠 Tools Used
- MySQL – Used for data storage, querying, and analysis.


## SQL Analysis and Queries 

#### checking the structure and characters of the dataset(customers& geolocations)
```sql
select*
from target_db.customers
limit 10;
select* 
from target_db.geolocation
limit 5;
```
#### The time range between which the orders where placed 

```sql
select
min(order_purchase_timestamp) as start_time, 
max(order_purchase_timestamp) as end_time
from target_db.orders;
-- the time range which orders where place: 2016-09-04 21:15:19	2018-10-17 17:30:18
```

#### Display the details  cities and states of customers who ordered during the given period.

```sql
select 
c.customer_city, c.customer_state
from orders as o 
join customers as c
on o.customer_id= c.customer_id
where extract(Year from o.order_purchase_timestamp)= 2018
and extract( month from order_purchase_timestamp) between 1 and 3;
```
#### is there a growing trend in the number of orders placed over the past years?
```sql
SELECT
    EXTRACT(MONTH FROM order_purchase_timestamp) AS month,
    COUNT(order_id) AS order_number
FROM orders
GROUP BY EXTRACT(MONTH FROM order_purchase_timestamp)
ORDER BY order_number DESC;
--- The highest number of orders occurred in August (10,843 orders).
---The lowest number of orders occurred in September (4,305 orders).
---Overall, order activity is higher in mid-year months (May–August) and drops toward the end of the
```
####   During what time of the day , do Brazlians customers mostly place thier orders? (Dawn, morning,afternoon, night)?
- ##### 0-6 hours- Dawn time 
- ##### 7-12 hours- morning
- #####  13-18- Afternoon 
- ##### 19-23 hours- Night 

```sql

SELECT
    EXTRACT(hour FROM order_purchase_timestamp) AS time,
    COUNT(order_id) AS order_number
FROM orders
GROUP BY EXTRACT(hour FROM order_purchase_timestamp)
ORDER BY order_number DESC;

---Most purchases occur during the afternoon (13:00–18:00), with 38,135 orders.
---Customer activity decreases significantly during the dawn period (00:00–06:00), which records the lowest number of purchases.	

```
#### What is the month-over-month number of orders?
```sql
SELECT
    EXTRACT(YEAR FROM order_purchase_timestamp) AS year,
    EXTRACT(MONTH FROM order_purchase_timestamp) AS month,
    COUNT(*) AS numb_orders
FROM orders
GROUP BY
    EXTRACT(YEAR FROM order_purchase_timestamp),
    EXTRACT(MONTH FROM order_purchase_timestamp)
ORDER BY
    year, month DESC;

--- Order volumes increased steadily from 2016 to 2018. Sales were very low in 2016, grew significantly throughout 2017, 
and remained consistently high during most of 2018. 
---- The unusually low order counts in September and October 2018 likely reflect incomplete data rather than a real decrease in demand.

```

#### What is the distribution of customers across the states of Brazil?
```sql
select customer_city,customer_state,
count(Distinct customer_id) as customer_count 
from customers
group by customer_city, customer_state
order by customer_count desc;
--- São Paulo (SP) has the largest customer base with 13,748 customers, followed by Rio de Janeiro (RJ) with 6,090 customers.
--- Other major customer hubs include Belo Horizonte (MG) (2,426 customers), Brasília (DF) (1,889 customers), and Curitiba (PR) (1,340 customers).
--- The majority of high-customer cities are concentrated in the Southeast region of Brazil, particularly in the state of São Paulo (SP).
--- This indicates that customer demand is highly concentrated in Brazil's largest metropolitan areas.

```

#### What was the percentage increase in the total cost of orders from 2017 to 2018 (considering only orders placed between January and August)?

-  Get the % increase in the cost of ordres from year 2017 to 2018(iclude months Bw Jan and Augst only) 
- we can use the "pyment_vlue") column in the pyment table to get the cost of orders
- step one: calculate the total pyments per year 
```sql
WITH yearly_totals AS (
    SELECT
        EXTRACT(YEAR FROM o.order_purchase_timestamp) AS year,
        SUM(p.payment_value) AS total_payment
    FROM payments AS p
    JOIN orders AS o
        ON p.order_id = o.order_id
    WHERE EXTRACT(YEAR FROM o.order_purchase_timestamp) IN (2017, 2018)
      AND EXTRACT(MONTH FROM o.order_purchase_timestamp) BETWEEN 1 AND 8
    GROUP BY EXTRACT(YEAR FROM o.order_purchase_timestamp)
),

- Step 2: Use LEAD window function to compare each year's payments with the previous year
yearly_comparisons AS (
```sql
SELECT
        year,
        total_payment,
        LEAD(total_payment) OVER (ORDER BY year DESC) AS previous_year_payment
    FROM yearly_totals
)
```
- Step 3: Calculate percentage increase
```sql
SELECT
    year,
    total_payment,
    previous_year_payment,
    ((total_payment - previous_year_payment)
      / previous_year_payment) * 100 AS percentage_increase
FROM yearly_comparisons;

--- 2017 (Jan–Aug): Total payments = 3,669,022.12
--- 2018 (Jan–Aug): Total payments = 8,694,733.84
---Percentage increase: 136.98%
```

#### What are the average and total price and freight values by customer state?



# mean & sum of price and freight value by customer state 
```sql
select
c.customer_state,
avg(price) as avg_price,
sum(price) as sum_price,
avg(freight_value) as avg_freight,
sum(freight_value) as sum_freight
from orders as o 
join order_items as oi 
on o.order_id= oi.order_id
join customers as c 
on o.customer_id = c.customer_id
Group By c.customer_state;
--- The analysis compares the average and total product prices and freight costs across Brazilian states. 
--- Results show that customer spending and shipping costs vary considerably by state. 
--- States such as São Paulo (SP) and Minas Gerais (MG) have the highest total sales and freight values, reflecting their larger customer bases, 
while some states exhibit higher average freight costs, indicating relatively more expensive shipping per order.
```


#### How can I calculate the number of days between the purchase date, the delivery date, and the estimated delivery date?
```sql
SELECT 
    order_id,

    DATEDIFF(
        order_delivered_customer_date,
        order_purchase_timestamp
    ) AS days_to_delivery,

    DATEDIFF(
        order_delivered_customer_date,
        order_estimated_delivery_date
    ) AS diff_estimated_delivery

FROM orders;
--- The company generally performs well in meeting delivery expectations, as most orders are delivered earlier than the estimated delivery date. 
--- However, there is variability in delivery times, with some orders experiencing significant delays. 
--- This indicates inconsistency in logistics performance and potential areas for optimization in delivery planning and forecasting
```

#### What are the top five states with the highest and lowest average freight values?
```sql
SELECT 
    c.customer_state, 
    AVG(DATEDIFF(o.order_delivered_customer_date, o.order_purchase_timestamp)) AS avg_time_to_delivery
FROM orders AS o
JOIN customers AS c
    ON o.customer_id = c.customer_id
GROUP BY c.customer_state
ORDER BY avg_time_to_delivery DESC
LIMIT 5;

--- Roraima (RR) has the longest average delivery time at 29 days, indicating potential logistics challenges.
--- States in the Northern region (RR, AP, AM, PA) generally experience slower delivery times.
--- This suggests that geographical distance and infrastructure limitations significantly impact delivery speed.
```

```sql
SELECT 
    p.payment_type, 
    EXTRACT(YEAR FROM o.order_purchase_timestamp) AS year,
    EXTRACT(MONTH FROM o.order_purchase_timestamp) AS month,
    COUNT(DISTINCT o.order_id) AS order_count
FROM orders AS o 
INNER JOIN payments AS p
    ON o.order_id = p.order_id 
GROUP BY p.payment_type, year, month 
ORDER BY p.payment_type, year, month desc;
--- Credit Card: Remains the dominant payment method throughout the entire period. It shows significant growth from late 2016, peaking in March 2018 (5,674 orders).

--- UPI: Shows impressive adoption and steady growth. Starting from 63 orders in October 2016, it grew rapidly to exceed 1,000 monthly orders by mid-2018, reaching a peak of 1,518 in January 2018.

--- Voucher: Usage is consistently present, typically ranging between 100 and 300 orders per month, with a notable peak of 304 in January 2018.

--- Debit Card: Shows the lowest volume among major methods, generally maintaining a steady, albeit smaller, footprint compared to credit cards and UPI.

```












