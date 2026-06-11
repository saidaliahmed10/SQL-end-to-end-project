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





	










