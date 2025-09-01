
---


````markdown
# Find Product Prices on a Given Date

## Problem Statement
We need to find the prices of all products on `2024-12-16`.  
- If a product has no price change before that date, the default price is **10**.  
- Otherwise, use the most recent change **on or before** `2024-12-16`.

---

## Table and Sample Data

```sql
CREATE TABLE products(
    product_id INT,
    new_price INT,
    change_date DATE
);

INSERT INTO products VALUES (1,20,'2024-12-14');
INSERT INTO products VALUES (2,50,'2024-12-14');
INSERT INTO products VALUES (1,30,'2024-12-15');
INSERT INTO products VALUES (1,35,'2024-12-16');
INSERT INTO products VALUES (2,65,'2024-12-17');
INSERT INTO products VALUES (3,20,'2024-12-18');
````

---

## SQL Solution (Using Window Function)

```sql
WITH price_history AS (
    SELECT 
        product_id,
        new_price,
        change_date,
        ROW_NUMBER() OVER (
            PARTITION BY product_id 
            ORDER BY change_date DESC
        ) AS rn
    FROM products
    WHERE change_date <= '2024-12-16'
)
SELECT p.product_id,
       COALESCE(ph.new_price, 10) AS price_on_2024_12_16
FROM (SELECT DISTINCT product_id FROM products) p
LEFT JOIN price_history ph
    ON p.product_id = ph.product_id AND ph.rn = 1;
```

---

---

## Output

| product\_id | price\_on\_2024\_12\_16 |
| ----------- | ----------------------- |
| 1           | 35                      |
| 2           | 50                      |
| 3           | 10                      |

---


