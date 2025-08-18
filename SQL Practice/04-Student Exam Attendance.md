# Student Exam Attendance

## Problem Statement
Write a SQL query to find how many times each student attended each subject exam.
Return results ordered by `student_id` and `subject_name`.

---

## Table Definition & Sample Data

```sql
CREATE TABLE Students (
    student_id INT,
    student_name VARCHAR(20)
);

CREATE TABLE Examinations (
    exam_id INT,
    student_id INT,
    subject_name VARCHAR(20)
);

-- Sample Inserts
INSERT INTO Students VALUES (1, 'Alice');
INSERT INTO Students VALUES (2, 'Bob');
INSERT INTO Students VALUES (3, 'John');
INSERT INTO Students VALUES (4, 'Alex');

INSERT INTO Examinations VALUES (10, 1, 'Math');
INSERT INTO Examinations VALUES (11, 1, 'Math');
INSERT INTO Examinations VALUES (12, 1, 'Physics');
INSERT INTO Examinations VALUES (13, 1, 'Math');
INSERT INTO Examinations VALUES (14, 1, 'Programming');
INSERT INTO Examinations VALUES (15, 1, 'Physics');
INSERT INTO Examinations VALUES (16, 2, 'Programming');
INSERT INTO Examinations VALUES (17, 2, 'Math');
INSERT INTO Examinations VALUES (18, 3, 'Math');
INSERT INTO Examinations VALUES (19, 3, 'Physics');
INSERT INTO Examinations VALUES (20, 3, 'Programming');
INSERT INTO Examinations VALUES (21, 4, 'Math');
```

---

SQL Solution & Output

```sql
SELECT 
    s.student_id,
    s.student_name,
    e.subject_name,
    COUNT(*) AS attend_exams
FROM Students s
JOIN Examinations e
    ON s.student_id = e.student_id
GROUP BY s.student_id, s.student_name, e.subject_name
ORDER BY s.student_id, e.subject_name;
```

**Output**

| student_id | student_name | subject_name | attend_exams |
|------------|--------------|--------------|--------------|
| 1          | Alice        | Math         | 3 |
| 1          | Alice        | Physics      | 2 |
| 1          | Alice        | Programming  | 1 |
| 2          | Bob          | Math         | 1 |
| 2          | Bob          | Programming  | 1 |
| 3          | John         | Math         | 1 |
| 3          | John         | Physics      | 1 |
| 3          | John         | Programming  | 1 |
| 4          | Alex         | Math         | 1 |