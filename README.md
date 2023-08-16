# MySQL Learning: Notes and Codes (Source: Programming with Mosh) _ Part 1 & 2 
### Notes 1:
* SQL (Structured Query Language) is a programming language designed for managing and manipulating relational databases. It is widely used for storing, retrieving, and manipulating data in relational database management systems (RDBMS).
* What will we learn? 1) Summarizing data; Writing complex queries; Built-in functions; Views; Stored procedures. 2) Triggers; Events; Transactions; Concurrency. 3) Design databases; Indexing fir high performance; Securing databases
* `SELECT` select variables/columns/fields from the table.
* `FROM` indicates the location of the source table.
* Typical order `SELECT`-`FROM`-`WHERE`-`GROUP BY`-`HAVING`-`ORDER BY`
* Always end a query with a `;`.
### 1) `WHERE`Clause Exercise:
```sql
-- Get the orders placed this year (2023) from the orders table
SELECT 
    *
FROM
    orders
WHERE
    order_date >= '2023-01-01';
```

### 2) `AND` `OR` `NOT` operators, Exercise:
```sql
-- From the order_items table, get the item
    -- For order #6
    -- where total price is >= 30
SELECT 
    *
FROM
    order_items
WHERE
    order_id =6 AND unit_price * quantity >= 30;
```

### 3) `IN` operators, Exercise:
```sql
-- Return products with
    -- quantity in stock equal to 49, 38,72
    -- 
SELECT 
    *
FROM
    products
WHERE
    quantity_in_stock IN (49, 38,72);
```


### 4) `BETWEEN` operators, Exercise:
```sql
-- Return customres born
    -- between 1/1/1990 and 1/1/2000
    -- 
SELECT 
    *
FROM
    customers
WHERE
    birth_date BETWEEN '1990-01-01' AND '2000-01-01';
```

### 5) & 6) 'LIKE' 'REGEXP' operators, Exercise:
```sql
-- Get the customres whose
    -- address contain TRAIL or AVENUE
    -- 
SELECT 
    *
FROM
    customers
WHERE
    address LIKE '%TRAIL%' OR
    address LIKE '%AVENUE%'; 
```
* silimar type
```sql
-- Get the customres whose
    -- address contain TRAIL or AVENUE
    -- 
SELECT 
    *
FROM
    customers
WHERE
    address REGEXP 'TRAIL|AVENUE';
```

```sql
-- Get the customres whose
    -- address end with IL or start with AV
    -- 
SELECT 
    *
FROM
    customers
WHERE
    address REGEXP 'IL$|^AV';
```

### 7) 'IS NULL'operator, Exercise:
```sql
-- Get the orders that are not shipped
     
SELECT 
    *
FROM
    orders
WHERE
    address shipped_date IS NULL; 
```

### 8) 'ORDER BY'clause, Exercise:
```sql
--  Order the customers by state and first name, by descending orders
     
SELECT 
    *
FROM
    customers
ORDER BY
    state DESC, first_name DESC;
```


### 9) 'LIMIT' clause
```sql
--  Get the top 3 loyal customers (with highest points)
     
SELECT 
    *
FROM
    customers
ORDER BY
    points DESC
LIMIT 3;
```

# MySQL Learning: Notes and Codes (Source: Programming with Mosh) _ Part 3
### Notes 2: Select columns from multiple tables

### 1) 'Inner Joins' Exercise:

```sql
-- Look at the order_items table, this table has 4 columns: order_id, product_id, quantity, and unit_price
-- For each order, join on the products table, return both product_it and its name
SELECT *
FROM order_items oi
JOIN products p
    ON oi.product_id = p.product_id
```

### 2) 'Joining Across Databases' Exercise:

```sql
-- How to combine columns from tables across multiple databases
USE sql_inventory; ## database 1
SELECT *
FROM store.order_items oi ## database 2
JOIN products p  ## table
    ON oi.product_id = p.product_id 
```

### 3) 'Self Joins' Exercise:
```sql
-- Join a table with itself
-- Try to refind the manager's name (which is the name that employees report to)
USE sql_hr; ## the employee table contains the columns related to both the employee (e) and manager (m) (i.e., table e= table m)
SELECT
    e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM
    employees e
JOIN
    employees m
    ON
    e.reports_to = m.employee_id  ## The returned table contain 3 columns: employees' id, employees' first name, and corresponding manager's name
```

### 4) Joining Multiple Tables

```sql
USE invoicing;
SELECT *
FROM payments p 
JOIN clients c
    ON p.client_id = c.client_id
JOIN payment_methods pm
    ON p.payment_method = pm.payment_method_id
```

### 5) Compound Join Conditions

```sql
USE store;
SELECT *
FROM order_items oi
JOIN order_item_notes oin
    ON oi.order_id = oin.order_id
    AND oi.product_id = oin.product_id

## ON AND are the so called Compound condition, we have multiple conditions to join two tables

```

### 6) Implicit Join Syntax

```sql
USE store;
SELECT *
FROM orders o
JOIN customers c
    ON o.customer_id =c.customer_id
```

--There is another way to write this query using Implicit Join Syntax

```sql
SELECT *
FROM orders o, customers c
WHERE o.customer_id = c.customer_id
```
### 7) Outer Joins

```sql
## INNER JOIN = JOIN (whenever you use JOIN, it means you use INNER JOIN)
USE store;
SELECT 
    c.customer_id,
	c.first_name,
    o.order_id
FROM orders o
JOIN customers c
    ON o.customer_id =c.customer_id
## As there are some customers don't have id in one table, the returned results only contain the samples meet the demand "o.customer_id =c.customer_id", some samples were not returned
```

``` sql
SELECT 
    c.customer_id,
	c.first_name,
    o.order_id
FROM customers c
LEFT JOIN orders o
    ON o.customer_id =c.customer_id
ORDER BY c.customer_id

## returned results contain the NULL result for order_id
## LEFT JOIN: we will return all the customers no matter whether the condition is true or not;
## RIGHT JOIN: return same with results of INNER JOIN. In this case, if we want to check all the customers, change the sequence to "FROM orders" "RIGHT JOIN orders o"
```
-- Exercise
```sql
#Exercise: join the product table and order_items table, select three columns: product_id, name, and quantity, keep the NULL
USE store;
SELECT 
    p.product_id,
	p.name,
    oi.quantity
FROM products p
LEFT JOIN order_items oi
    ON p.product_id = oi.product_id

```

### 8) Outer Join Between Multiple tables
```sql
USE store;
SELECT 
    c.customer_id,
	c.first_name,
    o.order_id
FROM customers c
LEFT JOIN orders o
    ON o.customer_id = c.customer_id
LEFT JOIN shippers sh
    ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id    
```
--Exercise
```sql
USE store;
SELECT
    o.order_date,
    o.order_id,
    c.first_name,
    s.name AS shipper,
    os.name AS status
FROM orders o
LEFT JOIN customers c
    ON o.customer_id = c.customer_id
LEFT JOIN shippers s
    ON o.shipper_id = s.shipper_id
LEFT JOIN order_statuses os
    ON o.status = os.order_status_id
ORDER BY shipper
```
### 9) Self Outer Joins
```sql
USE sql_hr;
SELECT
    e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.reports_to = m.employee_id
```

### 10) The USING Clause
```sql
--If columns' names are same, we can use a more simple USING clause to replace ON clause
USE store;
SELECT *
FROM order_items oi
JOIN order_item_notes oin
    ON oi.order_id = oin.order_id AND oi.product_id = oin.product_id
```
```sql
USE store;
SELECT *
FROM order_items oi
JOIN order_item_notes oin
    USING (order_id, product_id)
```
### 11) Natural Joins
```sql
--Not recommended, Because it can produce unexpected result
SELECT *
FROM orders o
NATURAL JOIN customers c
```

### 12) Outer Joins
```sql
--Do a cross join between shippers and products; using the implicit syntax; and then using the explicit syntax
USE store;
SELECT
    sh.name AS shipper,
    p.name AS product
FROM shippers sh, products p 
ORDER BY sh.name
```

```sql
USE store;
SELECT
    sh.name AS shipper,
    p.name AS product
FROM shippers sh
CROSS JOIN products p 
ORDER BY sh.name
--We use CROSS JOIN to combine or join every record from the first table with every record in the second table
```
### 13) Unions

```sql
-- Combine multiple rows from tables
USE store;
SELECT
    customer_id,
    first_name,
    points,
    'Bronze' AS type
FROM
customers
WHERE points < 2000 
UNION 
SELECT
    customer_id,
    first_name,
    points,
'Silver' AS type
FROM customers
WHERE points BETWEEN 2000 AND 3000 
```

# MySQL Learning: Notes and Codes (Source: Programming with Mosh) _ Part 4

### 1) Column Attributes
### 2) Inserting a Row
```sql
USE store;
INSERT INTO customers (
    last_name,
	first_name,
    birth_date,
    address,
    city,
    state)
VALUES (
    'Smith',
    'John',
    '1990-01-01',
    'adress',
    'city',
    'CA'
    )
```

### 3) Inserting Multiple Rows
```sql
-- Insert three rows in the products table;
INSERT INTO products (name, quantity_in_stock, unit_price)
VALUES ('Product1', 10, 1.95),
    ('Product2', 11, 1.95),
    ('Product3', 12, 1.95)
```
### 4) Inserting Hierarchical Rows
```sql
-- How to insert data into multiple tables
-- 'order' table is parent, while 'order_items' table is child
INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-02', 1);
INSERT INTO order_items
VALUES (LAST_INSERT_ID(), 1, 1, 2.95),
       (LAST_INSERT_ID(), 2, 1, 3.95)
```
### 5) Creating a Copy of a Table
```sql
-- How to copy data from one table to another
CREATE TABLE orders_archived AS 
SELECT * FROM orders
```

```sql
-- If you want to copy only a subset of the table into the new table, e.g., all the orders before 2019
INSERT INTO orders_archived
SELECT * 
FROM orders
WHERE order_date < '2019-01-01'
```
### Exercise
```sql
USE invoicing;
CREATE TABLE invoice_archived AS
SELECT
    i.invoice_id,
    i.number,
    c.name AS client,
    i.invoice_total,
    i.payment_total,
    i.invoice_date,
    i.payment_date,
    i.due_date
FROM invoices i
JOIN clients c
    USING (client_id)
WHERE payment_date IS NOT NULL
```

### 6) Updating a Single Row
```sql
-- How to update the data in sql
-- SET Clause: Specify new values for obe or more columns
UPDATE invoices
SET payment_total = 10,
 payment_date ='2019-03-01'
 WHERE invoice_id = 1 # specify the row
```
```sql
UPDATE invoices
SET 
payment_total = invoice_total * 0.5,
payment_date = due_date
WHERE invoice_id = 3
```


### 7) Updating Multiple Rows
```sql
USE invoices;
UPDATE invoices
SET 
payment_total = invoice_total * 0.5,
payment_date = due_date
WHERE client_id IN (3, 4)
```
### Exercise
```sql
--Write a SQL statement to give any customers born before 1990, 50 extra points
USE store;
UPDATE customers
SET points = points + 50
WHERE birth_date < '1990-01-01'
```

### 8) Using Subqueries in Updates
```sql
# what if we don't have the corresponding id
payment_total = invoice_total * 0.5,
payment_date = due_date
WHERE client_id IN 
    (SELECT client_id
    FROM clients
    WHERE state IN ('CA','NY')) 
```

```sql
USE store;
UPDATE orders
SET 
comments = 'Gold Customer'
WHERE ponits IN 
    (SELECT customer_id
    FROM customers
    WHERE points > 3000) 
```

### 9) Deleting Rows

```sql
DELETE FROM invoices
WHERE client_id = (
    SELECT *
    FROM clients
    WHERE name = 'Myworks'
    )
```
### 10) Restoring the database

















