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














