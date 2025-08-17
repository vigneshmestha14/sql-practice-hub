# SQL Puzzle - Duplicate Labeling

We are given a table `puzzle` with duplicate entries.\
We need to label duplicates as `DUP1`, `DUP2`, etc., and mark unique
values as `NULL`.

------------------------------------------------------------------------

## Table Structure & Data

``` sql
create table puzzle(id int identity, input varchar(2));

insert into puzzle values('a');
insert into puzzle values('a');
insert into puzzle values('b');
insert into puzzle values('b');
insert into puzzle values('c');
insert into puzzle values('d');
insert into puzzle values('d');
insert into puzzle values('e');
```

### Input Data

  id   input
  ---- -------
  1    a
  2    a
  3    b
  4    b
  5    c
  6    d
  7    d
  8    e

------------------------------------------------------------------------

## SQL Solution

``` sql
WITH DuplicateCheck AS (
    SELECT 
        id,
        input,
        ROW_NUMBER() OVER (PARTITION BY input ORDER BY id) AS rn,
        COUNT(*) OVER (PARTITION BY input) AS cnt
    FROM puzzle
)
SELECT 
    input,
    CASE 
        WHEN cnt > 1 THEN 'DUP' + CAST(rn AS VARCHAR(10))
        ELSE NULL
    END AS result
FROM DuplicateCheck
ORDER BY id;
```

------------------------------------------------------------------------

## Output

  input   result
  ------- --------
  a       DUP1
  a       DUP2
  b       DUP1
  b       DUP2
  c       NULL
  d       DUP1
  d       DUP2
  e       NULL

------------------------------------------------------------------------