# MatchingPair SQL Challenge

## Problem Statement
We have a table `MatchingPair` that contains an `Id` and a `PersonType`.  
`Id` starts with a letter (A for Adult, C for Child) followed by a number.  
We want to **match each Adult with the Child having the same number** in their `Id`.  
If no Child exists for an Adult, the result should still display the Adult with `NULL` in the Child column.

---

## Sample Data

| Id   | PersonType |
|------|------------|
| A1   | Adult      |
| C1   | Child      |
| A2   | Adult      |
| C2   | Child      |
| A3   | Adult      |
| C3   | Child      |
| A4   | Adult      |

---

## SQL Solution (T-SQL)

```sql
-- Create table
CREATE TABLE MatchingPair (
    Id VARCHAR(5),
    PersonType VARCHAR(10)
);

-- Insert sample data
INSERT INTO MatchingPair (Id, PersonType) VALUES
('A1', 'Adult'),
('C1', 'Child'),
('A2', 'Adult'),
('C2', 'Child'),
('A3', 'Adult'),
('C3', 'Child'),
('A4', 'Adult');

-- Step 1: Extract numeric part of Id for Adults and Children
WITH Adults AS (
    SELECT 
        Id,
        RIGHT(Id, LEN(Id) - 1) AS NumPart
    FROM MatchingPair
    WHERE PersonType = 'Adult'
),
Children AS (
    SELECT 
        Id,
        RIGHT(Id, LEN(Id) - 1) AS NumPart
    FROM MatchingPair
    WHERE PersonType = 'Child'
)

-- Step 2: Match Adults and Children on numeric part
SELECT 
    a.Id AS AdultId,
    c.Id AS ChildId
FROM Adults a
LEFT JOIN Children c
    ON a.NumPart = c.NumPart
ORDER BY a.Id;
