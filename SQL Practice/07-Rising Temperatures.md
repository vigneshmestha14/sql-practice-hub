
---

# 📈 Rising Temperatures

## 📌 Problem Statement

You are given a table `weather` that stores daily temperature records. Write a SQL query using a **Common Table Expression (CTE)** to find all dates where the temperature was **higher than the previous day's temperature**.

---

## 🛠️ Table Creation and Data Insertion

```sql
CREATE TABLE weather (
    id INT,
    record_date DATE,
    temperature INT
);

INSERT INTO weather VALUES (1, '2025-01-01', 10);
INSERT INTO weather VALUES (2, '2025-01-02', 12);
INSERT INTO weather VALUES (3, '2025-01-03', 13);
INSERT INTO weather VALUES (4, '2025-01-04', 11);
INSERT INTO weather VALUES (5, '2025-01-05', 12);
INSERT INTO weather VALUES (6, '2025-01-06', 9);
INSERT INTO weather VALUES (7, '2025-01-07', 10);
```

---

## 🧮 SQL Query Using CTE

```sql
WITH temp_comparison AS (
    SELECT
        w1.record_date,
        w1.temperature,
        w2.temperature AS prev_temperature
    FROM
        weather w1
    JOIN
        weather w2
    ON
        DATEDIFF(w1.record_date, w2.record_date) = 1
)
SELECT
    record_date
FROM
    temp_comparison
WHERE
    temperature > prev_temperature;
```

---

## 📊 Output

| record_date |
|-------------|
| 2025-01-02  |
| 2025-01-03  |
| 2025-01-05  |
| 2025-01-07  |

---
