
---

# 📈 Rolling Sum of Sales Analysis

## 📌 Problem Statement

Given a `sales` table containing product sales data with `product_id`, `product_name`, `price`, and `sold_date`, write SQL queries to:

1. Calculate the **cumulative rolling sum** of orders for each product ordered by `sold_date`.
2. Calculate the **rolling sum of orders for each product over the current date and the two preceding dates**.

---

## 🗃️ SQL Table Creation and Insert Statements

```sql
CREATE TABLE sales (
    product_id INT,
    product_name VARCHAR(10),
    price INT,
    sold_date DATE
);

INSERT INTO sales VALUES
(1, 'mouse', 150, '2025-01-01'),
(1, 'mouse', 145, '2025-01-02'),
(1, 'mouse', 280, '2025-01-03'),
(1, 'mouse', 300, '2025-01-04'),
(1, 'mouse', 150, '2025-01-05'),
(2, 'key-board', 350, '2025-01-01'),
(2, 'key-board', 380, '2025-01-02'),
(2, 'key-board', 390, '2025-01-03'),
(2, 'key-board', 400, '2025-01-04');
```

---

## 🧮 SQL Queries Using CTE

### 1️⃣ Cumulative Rolling Sum by Product

```sql
WITH SalesWithCumulativeSum AS (
    SELECT 
        product_id,
        product_name,
        sold_date,
        price,
        SUM(price) OVER (
            PARTITION BY product_id 
            ORDER BY sold_date 
            ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
        ) AS cumulative_sum
    FROM sales
)
SELECT * FROM SalesWithCumulativeSum;
```

### 2️⃣ Rolling Sum Over Current and Two Previous Dates

```sql
WITH SalesWith3DayRollingSum AS (
    SELECT 
        product_id,
        product_name,
        sold_date,
        price,
        SUM(price) OVER (
            PARTITION BY product_id 
            ORDER BY sold_date 
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
        ) AS rolling_3_day_sum
    FROM sales
)
SELECT * FROM SalesWith3DayRollingSum;
```

---

## 📊 Output

### Cumulative Rolling Sum

| product_id | product_name | sold_date  | price | cumulative_sum |
|------------|--------------|------------|-------|----------------|
| 1          | mouse        | 2025-01-01 | 150   | 150            |
| 1          | mouse        | 2025-01-02 | 145   | 295            |
| 1          | mouse        | 2025-01-03 | 280   | 575            |
| 1          | mouse        | 2025-01-04 | 300   | 875            |
| 1          | mouse        | 2025-01-05 | 150   | 1025           |
| 2          | key-board    | 2025-01-01 | 350   | 350            |
| 2          | key-board    | 2025-01-02 | 380   | 730            |
| 2          | key-board    | 2025-01-03 | 390   | 1120           |
| 2          | key-board    | 2025-01-04 | 400   | 1520           |

### Rolling 3-Day Sum

| product_id | product_name | sold_date  | price | rolling_3_day_sum |
|------------|--------------|------------|-------|-------------------|
| 1          | mouse        | 2025-01-01 | 150   | 150               |
| 1          | mouse        | 2025-01-02 | 145   | 295               |
| 1          | mouse        | 2025-01-03 | 280   | 575               |
| 1          | mouse        | 2025-01-04 | 300   | 725               |
| 1          | mouse        | 2025-01-05 | 150   | 730               |
| 2          | key-board    | 2025-01-01 | 350   | 350               |
| 2          | key-board    | 2025-01-02 | 380   | 730               |
| 2          | key-board    | 2025-01-03 | 390   | 1120              |
| 2          | key-board    | 2025-01-04 | 400   | 1170              |

---

