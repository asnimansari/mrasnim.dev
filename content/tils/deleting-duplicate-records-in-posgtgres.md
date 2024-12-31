+++
title = "Deleting Duplicate Records In Posgtgres"
date = 2024-12-31
type = "til"
description = "Description about Deleting Duplicate Records In Posgtgres"
in_search_index = true
[taxonomies]
tags = ["postgres", "sql"]
+++

```sql
-- Create sample table
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    department VARCHAR(50),
    salary NUMERIC(10, 2)
);

-- Insert sample data with duplicates
INSERT INTO employees (first_name, last_name, email, department, salary) VALUES
('John', 'Doe', 'john.doe@example.com', 'IT', 50000.00),
('John', 'Doe', 'john.doe@example.com', 'IT', 50000.00),
('Jane', 'Smith', 'jane.smith@example.com', 'HR', 55000.00),
('Jane', 'Smith', 'jane.smith@example.com', 'HR', 55000.00),
('Mike', 'Johnson', 'mike.johnson@example.com', 'Finance', 60000.00),
('Mike', 'Johnson', 'mike.johnson@example.com', 'Finance', 60000.00);

-- Method 1: Delete duplicates keeping lowest ID
WITH duplicates AS (
    SELECT
        id,
        ROW_NUMBER() OVER (
            PARTITION BY first_name, last_name, email
            ORDER BY id
        ) AS row_num
    FROM employees
)
DELETE FROM employees
WHERE id IN (
    SELECT id
    FROM duplicates
    WHERE row_num > 1
);

-- Method 2: Delete duplicates using CTID
DELETE FROM employees
WHERE ctid NOT IN (
    SELECT MIN(ctid)
    FROM employees
    GROUP BY first_name, last_name, email
);

-- Method 3: Advanced deletion with custom criteria
WITH cte AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY first_name, last_name, email
            ORDER BY salary DESC
        ) AS rn
    FROM employees
)
DELETE FROM employees
WHERE id IN (
    SELECT id
    FROM cte
    WHERE rn > 1
);

-- Verify results
SELECT * FROM employees;
```
