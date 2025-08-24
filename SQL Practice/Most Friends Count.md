
---

````markdown
# Problem: Find the Person with the Most Friends and Their Total Friend Count

We have a `request` table where each row represents a **friend request** between two people.  
We assume:
- Friendship is **bidirectional** (if A sends to B, both A and B are friends).  
- We want to find the **person(s) with the most friends** and their **total friend count**.

---

## Table Definition & Sample Data

```sql
CREATE TABLE request (
    send_id INT,
    receive_id INT,
    date_id DATE
);

INSERT INTO request VALUES (1,2,'2024-01-01');
INSERT INTO request VALUES (1,3,'2024-01-02');
INSERT INTO request VALUES (1,4,'2024-01-03');
INSERT INTO request VALUES (2,3,'2024-01-04');
INSERT INTO request VALUES (3,4,'2024-01-05');
INSERT INTO request VALUES (5,1,'2024-01-06');
````

---

## SQL Solution

```sql
WITH all_friends AS (
    -- Make friendships bidirectional
    SELECT send_id AS person, receive_id AS friend FROM request
    UNION
    SELECT receive_id, send_id FROM request
),
friend_counts AS (
    SELECT person, COUNT(DISTINCT friend) AS total_friends
    FROM all_friends
    GROUP BY person
)
SELECT person, total_friends
FROM friend_counts
WHERE total_friends = (SELECT MAX(total_friends) FROM friend_counts);
```

---

## Step-by-Step Logic

1. **Expand friendships** using `UNION` so both `(send_id → receive_id)` and `(receive_id → send_id)` count.
2. **Count distinct friends** per person.
3. Find the **maximum friend count**.
4. Return the person(s) with that maximum.

---

## Expected Output

| person | total\_friends |
| ------ | -------------- |
| 1      | 4              |
| 3      | 3              |

---


