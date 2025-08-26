
---

````markdown
# Problem: Categorize Salary Based on Income Ranges

We have an `accounts` table containing account IDs and their income.  
We need to categorize each row into:  
- **Low Salary** → income < 20000  
- **Average Salary** → 20000 to 40000  
- **High Salary** → income > 40000  

---

## Table Definition & Sample Data

```sql
CREATE TABLE accounts (
    account_id INT,
    income INT
);

INSERT INTO accounts VALUES
(1,90000),
(2,17000),
(3,22000),
(4,13000),
(5,50000),
(6,56000),
(7,19000);
````

---

## SQL Solution

```sql
SELECT account_id,
       income,
       CASE 
            WHEN income < 20000 THEN 'Low Salary'
            WHEN income BETWEEN 20000 AND 40000 THEN 'Average Salary'
            ELSE 'High Salary'
       END AS category
FROM accounts
ORDER BY account_id;
```

---

## Output

| account\_id | income | category       |
| ----------- | ------ | -------------- |
| 1           | 90000  | High Salary    |
| 2           | 17000  | Low Salary     |
| 3           | 22000  | Average Salary |
| 4           | 13000  | Low Salary     |
| 5           | 50000  | High Salary    |
| 6           | 56000  | High Salary    |
| 7           | 19000  | Low Salary     |

---



