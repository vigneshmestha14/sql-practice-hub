
---

````markdown
# Problem: Find Last Person Who Can Board the Bus Without Exceeding Weight Limit

We have a `bus` table with the following details:  
- `personId`  
- `personName`  
- `personWeight`  
- `turn` (boarding order)  

The bus has a **maximum weight limit of 400**.  
We need to find the **name of the last person who can fit** without exceeding the total weight.

---

## Table Definition & Sample Data

```sql
CREATE TABLE bus (
    personId INT,
    personName VARCHAR(100),
    personWeight INT,
    turn INT
);

INSERT INTO bus VALUES
(5,'john',120,2),
(4,'tom',100,1),
(3,'rahul',95,4),
(6,'bhavna',100,5),
(1,'ankita',79,6),
(2,'Alex',80,3);
````

---

## SQL Solution

```sql
WITH ordered AS (
    SELECT personId, personName, personWeight, turn
    FROM bus
    ORDER BY turn
),
running AS (
    SELECT personId,
           personName,
           personWeight,
           turn,
           SUM(personWeight) OVER (ORDER BY turn) AS cum_weight
    FROM ordered
)
SELECT personName
FROM running
WHERE cum_weight <= 400
ORDER BY turn DESC
LIMIT 1;
```

---

## Step-by-Step Logic

1. **Order people by `turn`** (boarding sequence).
2. Compute a **running sum of weights**.
3. Pick only rows where `cum_weight ≤ 400`.
4. Return the **last person in order**.

---

## Output

| personName |
| ---------- |
| rahul      |

---

