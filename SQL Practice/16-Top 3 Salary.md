

````markdown
# Top 3 Highest Paid Employees in Each Department (Using CTE)

## Problem Statement
We need to find the **top 3 highest-paid employees in each department** using a **CTE**.

---

## Table and Sample Data

```sql
CREATE TABLE emp (
    eid INT,
        ename VARCHAR(10),
            salary INT,
                did INT
                );

                CREATE TABLE dept (
                    did INT,
                        dname VARCHAR(10)
                        );

                        INSERT INTO emp VALUES (1,'John',1000,1);
                        INSERT INTO emp VALUES (2,'Tom',1100,1);
                        INSERT INTO emp VALUES (3,'Henry',1100,1);
                        INSERT INTO emp VALUES (4,'Sam',1200,1);
                        INSERT INTO emp VALUES (5,'Rahul',1300,1);
                        INSERT INTO emp VALUES (6,'Kavya',900,1);
                        INSERT INTO emp VALUES (7,'Jatin',950,2);
                        INSERT INTO emp VALUES (8,'Ankita',980,2);

                        INSERT INTO dept VALUES (1,'IT');
                        INSERT INTO dept VALUES (2,'Sales');
                        ````

                        ---

                        ## SQL Solution (Using CTE)

                        ```sql
                        WITH RankedEmp AS (
                            SELECT 
                                    eid,
                                            ename,
                                                    salary,
                                                            did,
                                                                    ROW_NUMBER() OVER (PARTITION BY did ORDER BY salary DESC) AS rn
                                                                        FROM emp
                                                                        )
                                                                        SELECT 
                                                                            d.dname,
                                                                                r.ename,
                                                                                    r.salary
                                                                                    FROM RankedEmp r
                                                                                    JOIN dept d ON r.did = d.did
                                                                                    WHERE r.rn <= 3
                                                                                    ORDER BY d.dname, r.salary DESC;
                                                                                    ```

                                                                                    ---

                                                                                    ## Output

                                                                                    | dname | ename  | salary |
                                                                                    | ----- | ------ | ------ |
                                                                                    | IT    | Rahul  | 1300   |
                                                                                    | IT    | Sam    | 1200   |
                                                                                    | IT    | Tom    | 1100   |
                                                                                    | Sales | Ankita | 980    |
                                                                                    | Sales | Jatin  | 950    |

                                                                                    ---

                                                                                                                        