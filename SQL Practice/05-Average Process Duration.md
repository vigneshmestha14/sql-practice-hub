The Markdown file has been successfully created with the following sections:

---

### 📝 **SQL Analysis: Average Process Duration per Machine**

#### 📌 Problem Statement
Given an `Activity` table that records the start and end times of processes on different machines, calculate the average time each machine takes to complete a process.

---

#### 📥 SQL Insert Statements

```sql
CREATE TABLE Activity (
    machine_id INT,
    process_id INT,
    activity_type VARCHAR(10),
    timestamp DECIMAL(10,3)
);

INSERT INTO Activity (machine_id, process_id, activity_type, timestamp) VALUES
(0, 0, 'start', 0.712),
(0, 0, 'end', 1.520),
(0, 1, 'start', 3.140),
(0, 1, 'end', 4.120),
(1, 0, 'start', 0.550),
(1, 0, 'end', 1.550),
(1, 1, 'start', 0.430),
(1, 1, 'end', 1.420),
(2, 0, 'start', 4.100),
(2, 0, 'end', 4.512),
(2, 1, 'start', 2.500),
(2, 1, 'end', 5.000);
```

---

#### 🧮 Solution

```sql
WITH ProcessDurations AS (
    SELECT 
        machine_id,
        process_id,
        MAX(CASE WHEN activity_type = 'start' THEN timestamp END) AS start_time,
        MAX(CASE WHEN activity_type = 'end' THEN timestamp END) AS end_time
    FROM Activity
    GROUP BY machine_id, process_id
)

SELECT 
    machine_id,
    AVG(end_time - start_time) AS avg_process_duration
FROM ProcessDurations
GROUP BY machine_id;
```

---

#### 📊 Output

| machine_id | avg_process_duration |
|------------|----------------------|
| 0          | 0.894                |
| 1          | 0.995                |
| 2          | 1.456                |

---