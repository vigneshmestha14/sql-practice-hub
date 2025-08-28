# Problem Statement
We have a table `events(hall_id, from_date, to_date)` storing hall booking ranges.  
Some bookings overlap or touch. We need to **merge them into single continuous events per hall**.

---

## Table and Sample Data

```sql
CREATE TABLE events (
    hall_id INT,
    from_date DATE,
    to_date DATE
);

INSERT INTO events VALUES (1,'2025-01-05','2025-01-09');
INSERT INTO events VALUES (1,'2025-01-06','2025-01-10');
INSERT INTO events VALUES (1,'2025-01-07','2025-01-10');
INSERT INTO events VALUES (1,'2025-01-11','2025-01-15');
INSERT INTO events VALUES (2,'2025-02-15','2025-02-19');
INSERT INTO events VALUES (2,'2025-02-16','2025-02-20');
INSERT INTO events VALUES (2,'2025-02-21','2025-02-25');
````

---

## SQL Solution

```sql
WITH ordered AS (
    SELECT
        hall_id,
        from_date,
        to_date,
        LAG(to_date) OVER (PARTITION BY hall_id ORDER BY from_date) AS prev_end
    FROM events
),
grouped AS (
    SELECT
        hall_id,
        from_date,
        to_date,
        SUM(CASE WHEN from_date <= COALESCE(prev_end, from_date) THEN 0 ELSE 1 END)
            OVER (PARTITION BY hall_id ORDER BY from_date) AS grp
    FROM ordered
)
SELECT
    hall_id,
    MIN(from_date) AS merged_from,
    MAX(to_date)  AS merged_to
FROM grouped
GROUP BY hall_id, grp
ORDER BY hall_id, merged_from;
```

---

## Output

| hall_id | merged_from | merged_to |
| -------- | ------------ | ---------- |
| 1        | 2025-01-05   | 2025-01-10 |
| 1        | 2025-01-11   | 2025-01-15 |
| 2        | 2025-02-15   | 2025-02-20 |
| 2        | 2025-02-21   | 2025-02-25 |

---
