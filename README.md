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

### 5) 'LIKE' 'REGEXP' operators, Exercise:
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

### 6) 'IS NULL'operator, Exercise:
```sql
-- Get the orders that are not shipped
     
SELECT 
    *
FROM
    orders
WHERE
    address shipped_date IS NULL; 
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






