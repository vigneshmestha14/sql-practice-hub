# Company Revenue Growth Query

## Problem Statement
We have a table `CompanyRevenue` that stores yearly revenue for different companies.  
We need to **find the company names whose revenue increased every year**.

---

## Table Definition, Sample Data & Solution

```sql
CREATE TABLE CompanyRevenue (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    CompanyName VARCHAR(100),
    Year INT,
    Revenue INT
);

INSERT INTO CompanyRevenue (CompanyName, Year, Revenue) VALUES
('A', 2022, 100),
('A', 2023, 200),
('A', 2024, 150),
('B', 2022, 100),
('B', 2023, 200),
('B', 2024, 300),
('C', 2022, 200),
('C', 2023, 100),
('C', 2024, 300);

-- ✅ Solution: Find companies with revenue increasing every year
WITH RevenueCheck AS (
    SELECT 
        CompanyName,
        Year,
        Revenue,
        LAG(Revenue) OVER (PARTITION BY CompanyName ORDER BY Year) AS PrevRevenue
    FROM CompanyRevenue
)
SELECT CompanyName
FROM RevenueCheck
GROUP BY CompanyName
HAVING COUNT(CASE WHEN Revenue <= PrevRevenue THEN 1 END) = 0;
