
````markdown
# Find Customers Who Bought All Products

## Problem Statement
We want to find the `customer_id`s from the `Customers` table  
who have purchased **all** products listed in the `Products` table.

---

```sql
CREATE TABLE Customers (
    customer_id INT,
    product_id INT
);

CREATE TABLE Products (
    product_id INT
);

-- Sample Data
INSERT INTO Products VALUES (1), (2), (3);

INSERT INTO Customers VALUES (1,1);
INSERT INTO Customers VALUES (1,2);
INSERT INTO Customers VALUES (1,3);
INSERT INTO Customers VALUES (2,1);
INSERT INTO Customers VALUES (2,2);
INSERT INTO Customers VALUES (3,1);
INSERT INTO Customers VALUES (3,2);
INSERT INTO Customers VALUES (3,3);
````

---

## SQL Solution (Using GROUP BY and COUNT DISTINCT)

```sql
SELECT c.customer_id
FROM Customers c
GROUP BY c.customer_id
HAVING COUNT(DISTINCT c.product_id) = (SELECT COUNT(*) FROM Products);
```

---

## Explanation

* `COUNT(DISTINCT c.product_id)` → number of unique products bought by each customer.
* `(SELECT COUNT(*) FROM Products)` → total number of products available.
* Customers whose counts match → bought **all** products.

---

## Output

| customer\_id |
| ------------ |
| 1            |
| 3            |

✅ Customer 1 bought {1,2,3} → all products.
✅ Customer 3 bought {1,2,3} → all products.
❌ Customer 2 bought only {1,2}.

---

