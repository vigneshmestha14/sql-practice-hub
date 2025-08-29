
---

````markdown
# Moving Average of Customer Payments (7-Day Window)

## Problem Statement
We have a table `customers(customer_id, name, visited_date, amount)` storing each visit and the amount paid.  
We need to calculate the **7-day moving average** (current day + 6 previous days) of the payment amounts.  
The result should be **rounded to two decimal places**.

---

## Table and Sample Data

```sql
CREATE TABLE customers (
        customer_id INT,
            name VARCHAR(10),
                visited_date DATE,
                    amount INT
);

INSERT INTO customers VALUES (1,'John','2024-12-01',105);
INSERT INTO customers VALUES (2,'Tom','2024-12-02',110);
INSERT INTO customers VALUES (3,'Yash','2024-12-03',107);
INSERT INTO customers VALUES (4,'Kavya','2024-12-04',99);
INSERT INTO customers VALUES (5,'Amit','2024-12-05',111);
INSERT INTO customers VALUES (6,'Danial','2024-12-06',107);
INSERT INTO customers VALUES (7,'Elvis','2024-12-07',108);
INSERT INTO customers VALUES (8,'Maraia','2024-12-08',79);
INSERT INTO customers VALUES (9,'Jaze','2024-12-09',87);
INSERT INTO customers VALUES (1,'John','2024-12-10',91);
INSERT INTO customers VALUES (2,'Tom','2024-12-10',92);
````

---

## SQL Solution

```sql
SELECT 
    c.visited_date,
        ROUND(AVG(c2.amount), 2) AS average_amount
        FROM customers c
        JOIN customers c2
          ON c2.visited_date BETWEEN DATEADD(day, -6, c.visited_date) AND c.visited_date
          GROUP BY c.visited_date
          ORDER BY c.visited_date;
          ```

          ---

          ## Output

          | visited\_date | average\_amount |
          | ------------- | --------------- |
          | 2024-12-01    | 105.00          |
          | 2024-12-02    | 107.50          |
          | 2024-12-03    | 107.33          |
          | 2024-12-04    | 105.25          |
          | 2024-12-05    | 106.40          |
          | 2024-12-06    | 106.50          |
          | 2024-12-07    | 106.71          |
          | 2024-12-08    | 103.00          |
          | 2024-12-09    | 102.43          |
          | 2024-12-10    | 99.29           |

          ---

          