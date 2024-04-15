# About 

This project showcases an interactive report for a Global Superstore. The report focuses on 3 European markets, Ireland, France, and Germany. The report breaks down sales performance, the performance of each product type, and customer profiles.

Along with the dashboard we have created a SQL code to convert the data into an easy-to-use database. This allows for more niche data queries without having to alter the dashboard.

This data was chosen because it has an excellent variation of data types and before processing, had close to 50,000 rows of data. It was chosen by our group because it had excellent potential for gaining valuable insights using a visualisation tool like Power BI. 

It is of critical importance for an analyst using Power BI to set up and structure their data appropriately before creating visuals. This is because any mistakes that are noticed after transforming the data can have significant knock-on effects and can take time to fix. This dataset was designed in a way that was relatively well structured, and only required a small number of transformations. The rich variation of the data from profits and returns, to shipping data, meant that we could extract a myriad of valuable information and present it to our viewers. 

The data was free of significant outliers, but many of the variables had incorrect data types assigned to them. There were several fields with null values, and a number of date related fields that we used to analyse shipment efficiencies. All of these factors had to be accounted for before we began creating visualisations. 

The data itself was retrieved from a public database, and was free to access. The data was fictional, but in a real setting we may have had to remove customer names due to GDPR regulations to protect their privacy.

The Power BI dashboard/reports can be viewed in the file provided.

[Link to the free-to-use dataset](https://powerbidocs.com/tag/sample-superstore-sales-excel-xls/?ref=hackernoon.com)

[View more projects like this!](https://jeff-bow.github.io/)

# This project also contains an SQl file we created.

# Creating the Database - SQL

First, we create a database to store our data.

```SQL
CREATE DATABASE global_superstore_sales;
```

Then we can create our tables, we will have three tables "customers", "products", and "orders". "orders" will hold the foreign key for the other two tables. 

```SQL
CREATE TABLE customers(
customer_id VARCHAR(12) PRIMARY KEY,
customer_name VARCHAR(22),
segment VARCHAR(11),
city VARCHAR(35),
state VARCHAR(36),
country VARCHAR(32),
region VARCHAR(17),
market VARCHAR(12));
```

```SQL
CREATE TABLE products(
product_id VARCHAR(11)PRIMARY KEY,
category VARCHAR(15),
sub_category VARCHAR(11),
product_name VARCHAR(127));
```

```SQL
CREATE TABLE orders(
order_number INT PRIMARY KEY,
order_id VARCHAR(24),
order_date DATE,
shipping_date DATE,
shipping_mode VARCHAR(14),
sales NUMERIC(10,2),
quantity INT,
discount NUMERIC(10,2),
profit NUMERIC(10,2),
shipping_cost NUMERIC(10,2),
order_priority VARCHAR(8),
product_id VARCHAR(11),
customer_id VARCHAR(12),
FOREIGN KEY (product_id) REFERENCES products(product_id),
FOREIGN KEY (customer_id) REFERENCES customers(customer_id));
```

Next, we import the dataset into each table. The data has been split into multiple files to make importing easier.

```SQL
COPY customers(customer_id, customer_name,segment,city,state,country,region,market)
FROM 'C:\Users\Public\Public Doc4SQL\customers.csv'
DELIMITER ','
CSV HEADER;
```

```SQL
COPY products(product_id, category,sub_category,product_name)
FROM 'C:\Users\Public\Public Doc4SQL\products.csv'
DELIMITER ','
CSV HEADER;
```

```SQL
COPY orders(order_number, order_id, order_date,shipping_date,shipping_mode,sales,quantity,discount,profit,shipping_cost,order_priority,product_id,customer_id)
FROM 'C:\Users\Public\Public Doc4SQL\orders.csv'
DELIMITER ','
CSV HEADER;
```

Now that our data is set up we can easily query the data. Below I will highlight some examples.

```SQL
-- Examples of Data Queries
-- Top 5 Products by Total Revenue
SELECT product_name, SUM(sales)
FROM products
JOIN orders ON products.product_id = orders.product_id
GROUP BY product_name
ORDER BY SUM DESC
LIMIT 5;
```

```SQL
-- Segments by Total Profit - Lowest to Highest
SELECT segment, SUM(profit)
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY segment
ORDER BY SUM ASC;
```

```SQL
-- Minimum Order Quantity by Country
SELECT country, AVG(quantity) as average_quantity
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY country
HAVING AVG(quantity) > 1
ORDER BY average_quantity ASC
LIMIT 5;
```
