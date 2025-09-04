
## Problem Statement
We need to find the **fraction of players** who logged in for at least **two consecutive days starting from their first login date**.  

---

## Table and Sample Data

```sql
CREATE TABLE activity (
    player_id INT,
    device_id INT,
    event_date DATE
);

INSERT INTO activity VALUES (1,2,'2024-12-01');
INSERT INTO activity VALUES (1,2,'2024-12-02');
INSERT INTO activity VALUES (2,3,'2024-12-05');
INSERT INTO activity VALUES (3,1,'2024-12-07');
INSERT INTO activity VALUES (3,4,'2024-12-09');
````

---

## SQL Solution (Using Window Functions)

```sql
WITH first_login AS (
    SELECT 
        player_id,
        MIN(event_date) AS first_date
    FROM activity
    GROUP BY player_id
),
consecutive_check AS (
    SELECT 
        a.player_id,
        a.event_date,
        f.first_date,
        LEAD(a.event_date) OVER (PARTITION BY a.player_id ORDER BY a.event_date) AS next_date
    FROM activity a
    JOIN first_login f ON a.player_id = f.player_id
)
, flagged AS (
    SELECT DISTINCT player_id
    FROM consecutive_check
    WHERE event_date = first_date 
      AND next_date = DATEADD(day,1,event_date)
)
SELECT 
    CAST(COUNT(DISTINCT f.player_id) AS FLOAT) / 
    (SELECT COUNT(DISTINCT player_id) FROM activity) AS fraction_players
FROM flagged f;
```

---

## Explanation

1. `first_login` → first login date of each player.
2. `consecutive_check` → look at each player's next login date using `LEAD()`.
3. `flagged` → pick players whose **first\_date + 1 = next\_date**.
4. Final division → players with consecutive logins ÷ total players.

---

## Output

| fraction\_players |
| ----------------- |
| 0.3333            |

✅ Only **Player 1** logged in consecutively (Dec 1, Dec 2).
Total players = 3 → fraction = 1/3 = **0.3333**.

---


