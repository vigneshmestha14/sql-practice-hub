
---

````markdown
# Problem: Calculate Yearly Total Sales for Each Product

We have a table `products` that contains the product ID, sales validity period (start and end date), and the average daily sales amount.  
We need to calculate the **total sales for each product in every calendar year** between the start and end dates.

---

## Table Definition & Sample Data

```sql
CREATE TABLE products (
    product_id INT,
    start_date DATE,
    end_date DATE,
    avg_sales_amount INT
);

INSERT INTO products VALUES (1,'2024-01-01','2024-12-31',50);
INSERT INTO products VALUES (2,'2024-01-01','2024-04-30',70);
INSERT INTO products VALUES (3,'2024-01-01','2025-03-05',40);
````

---

## SQL Solution

```sql
WITH years AS (
    -- Generate distinct years from the data
    SELECT DISTINCT EXTRACT(YEAR FROM start_date) AS yr FROM products
    UNION
    SELECT DISTINCT EXTRACT(YEAR FROM end_date) FROM products
),
expanded AS (
    -- Cross join each product with possible years in its range
    SELECT p.product_id, y.yr,
           GREATEST(p.start_date, MAKE_DATE(y.yr,1,1)) AS period_start,
           LEAST(p.end_date, MAKE_DATE(y.yr,12,31)) AS period_end,
           p.avg_sales_amount
    FROM products p
    JOIN years y
      ON y.yr BETWEEN EXTRACT(YEAR FROM p.start_date) AND EXTRACT(YEAR FROM p.end_date)
)
SELECT product_id,
       yr AS year,
       SUM((period_end - period_start + 1) * avg_sales_amount) AS total_sales
FROM expanded
GROUP BY product_id, yr
ORDER BY product_id, yr;
```

---

## Expected Output

| product\_id | year | total\_sales |
| ----------- | ---- | ------------ |
| 1           | 2024 | 18250        |
| 2           | 2024 | 8470         |
| 3           | 2024 | 14600        |
| 3           | 2025 | 1720         |

---

