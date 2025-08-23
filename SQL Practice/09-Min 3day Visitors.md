
---

````markdown
# Problem: Find Minimum Consecutive 3-Day Visitors Where Count > 100

We have a table `visit` that stores daily visitor counts.  
We need to find the **minimum sum of visitors over 3 consecutive days**, but only for days where the visitor count is strictly greater than 100.

---

## Table Definition & Sample Data

```sql
CREATE TABLE visit (
    id INT,
    visit_date DATE,
    visitors INT
);

INSERT INTO visit VALUES
(1,'2025-01-01',101),
(2,'2025-01-02',110),
(3,'2025-01-03',70),
(4,'2025-01-04',105),
(5,'2025-01-05',107),
(6,'2025-01-06',70),
(7,'2025-01-07',107),
(8,'2025-01-08',120),
(9,'2025-01-09',109),
(10,'2025-01-10',99);
````

---

## SQL Solution

```sql
WITH filtered AS (
    SELECT *
    FROM visit
    WHERE visitors > 100
),
consecutive AS (
    SELECT v1.id AS start_id,
           v1.visit_date AS start_date,
           v1.visitors +
           v2.visitors +
           v3.visitors AS total_visitors
    FROM filtered v1
    JOIN filtered v2 ON v2.id = v1.id + 1
    JOIN filtered v3 ON v3.id = v1.id + 2
)
SELECT start_id, start_date, total_visitors
FROM consecutive
ORDER BY total_visitors
LIMIT 1;
```

---

---

## Expected Output

| start\_id | start\_date | total\_visitors |
| --------- | ----------- | --------------- |
| 4         | 2025-01-04  | 319             |

---


