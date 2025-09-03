
---

## ✅ Problem Statement

You are given a table `tourist` that records the number of people visiting a tourist location each month. Write a SQL query to find **three consecutive rows** where the number of `visited_people` is **greater than 500**.

---

## 🛠️ Table Creation and Data Insertion

```sql
CREATE TABLE tourist (
    id INT IDENTITY,
    date_id DATE,
    visited_people INT
);

INSERT INTO tourist VALUES ('2024-01-01', 700);
INSERT INTO tourist VALUES ('2024-02-01', 460);
INSERT INTO tourist VALUES ('2024-03-01', 550);
INSERT INTO tourist VALUES ('2024-04-01', 510);
INSERT INTO tourist VALUES ('2024-05-01', 550);
INSERT INTO tourist VALUES ('2024-06-01', 540);
INSERT INTO tourist VALUES ('2024-07-01', 90);
INSERT INTO tourist VALUES ('2024-08-01', 650);
INSERT INTO tourist VALUES ('2024-09-01', 580);
INSERT INTO tourist VALUES ('2024-10-01', 590);
```

---

## 🧮 SQL Query to Find 3 Consecutive Rows with `visited_people > 500`

```sql
WITH consecutive_visits AS (
    SELECT
        t1.id AS id1,
        t2.id AS id2,
        t3.id AS id3,
        t1.date_id AS date1,
        t2.date_id AS date2,
        t3.date_id AS date3
    FROM
        tourist t1
        JOIN tourist t2 ON t2.id = t1.id + 1
        JOIN tourist t3 ON t3.id = t2.id + 1
    WHERE
        t1.visited_people > 500 AND
        t2.visited_people > 500 AND
        t3.visited_people > 500
)
SELECT date1, date2, date3 FROM consecutive_visits;
```

---

## 📊 Output

| date1       | date2       | date3       |
|-------------|-------------|-------------|
| 2024-03-01  | 2024-04-01  | 2024-05-01  |
| 2024-04-01  | 2024-05-01  | 2024-06-01  |
| 2024-08-01  | 2024-09-01  | 2024-10-01  |

