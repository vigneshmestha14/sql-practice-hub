
````markdown
# Swap Seat IDs of Every Two Consecutive Students

## Problem Statement
We need to **swap the seat IDs of every two consecutive students**.  
- If the last student has an odd ID, their seat remains unchanged.

---

## Table and Sample Data

```sql
CREATE TABLE seat (
    id INT,
    name VARCHAR(10)
);

INSERT INTO seat VALUES (1,'John');
INSERT INTO seat VALUES (2,'Tom');
INSERT INTO seat VALUES (3,'Yash');
INSERT INTO seat VALUES (4,'Bhavna');
INSERT INTO seat VALUES (5,'Aryan');
INSERT INTO seat VALUES (6,'Kiran');
INSERT INTO seat VALUES (7,'Prativa');
INSERT INTO seat VALUES (8,'Ash');
````

---

## SQL Solution

```sql
SELECT 
    CASE
        WHEN id % 2 = 1 AND id + 1 <= (SELECT MAX(id) FROM seat) THEN id + 1
        WHEN id % 2 = 0 THEN id - 1
        ELSE id
    END AS new_id,
    name
FROM seat
ORDER BY new_id;
```

---

## Explanation

* If the `id` is **odd** and not the last record → shift it to `id+1`.
* If the `id` is **even** → shift it to `id-1`.
* If the `id` is the **last odd seat** → keep it unchanged.

---

## Output

| new\_id | name    |
| ------- | ------- |
| 1       | Tom     |
| 2       | John    |
| 3       | Bhavna  |
| 4       | Yash    |
| 5       | Kiran   |
| 6       | Aryan   |
| 7       | Ash     |
| 8       | Prativa |

---

