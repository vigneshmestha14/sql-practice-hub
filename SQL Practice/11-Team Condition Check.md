
---

````markdown
# Problem: Check if Both Conditions are 'Y' for Minimum 2 Members

We have a `puzzle` table where each row represents a team member with two conditions (`condition1`, `condition2`).  

We need to check **per team**:
- If **both `condition1` and `condition2` = 'Y'** for a member, count them.  
- If at least **2 members** satisfy this, return `'Y'`; otherwise return `'N'`.

---

## Table Definition & Sample Data

```sql
CREATE TABLE puzzle (
    teamID INT,
    memberID VARCHAR(10),
    condition1 VARCHAR(10),
    condition2 VARCHAR(10)
);

INSERT INTO puzzle VALUES
(1,'m1','Y','Y'),
(1,'m2','Y','Y'),
(1,'m3','Y','Y'),
(1,'m4','Y','N'),
(2,'m1','Y','Y'),
(2,'m2','Y','N'),
(2,'m3','N','Y'),
(2,'m4','N','N'),
(2,'m5','N','Y'),
(3,'m1','Y','Y'),
(3,'m2','Y','N'),
(3,'m3','N','Y'),
(3,'m4','N','N'),
(3,'m5','Y','Y');
````

---

## SQL Solution

```sql
WITH valid_members AS (
    SELECT teamID, memberID
    FROM puzzle
    WHERE condition1 = 'Y' AND condition2 = 'Y'
),
team_counts AS (
    SELECT teamID, COUNT(*) AS valid_count
    FROM valid_members
    GROUP BY teamID
)
SELECT t.teamID,
       CASE WHEN COALESCE(tc.valid_count,0) >= 2 THEN 'Y' ELSE 'N' END AS result
FROM (SELECT DISTINCT teamID FROM puzzle) t
LEFT JOIN team_counts tc ON t.teamID = tc.teamID;
```

---

## Step-by-Step Logic

1. **Filter** only members where both `condition1` and `condition2` = `'Y'`.
2. **Count** these valid members per `teamID`.
3. **Check** if count ≥ 2.

   * If yes → `'Y'`.
   * Else → `'N'`.

---

## Output

| teamID | result |
| ------ | ------ |
| 1      | Y      |
| 2      | N      |
| 3      | Y      |

---

