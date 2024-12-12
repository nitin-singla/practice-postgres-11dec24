# PostgreSQL Practice Exercises - Solutions

## Basic Queries

```sql
-- List all employees in IT department
SELECT * 
FROM employees 
WHERE department = 'IT';

-- Find employees with salaries > 70000
SELECT * 
FROM employees 
WHERE salary > 70000;

-- List active employees ordered by hire date
SELECT * 
FROM employees 
WHERE is_active = true 
ORDER BY hire_date;

-- Find employees hired between 2019 and 2020
SELECT * 
FROM employees 
WHERE hire_date BETWEEN '2019-01-01' AND '2020-12-31';
```

- - -

## Logical Expressions

```sql
-- IT or high salary
SELECT * 
FROM employees 
WHERE department = 'IT' OR salary > 70000;

-- Active and recent hires
SELECT * 
FROM employees 
WHERE is_active = true 
AND hire_date > '2019-12-31';

-- Not in Marketing
SELECT * 
FROM employees 
WHERE department != 'Marketing';

-- Salary comparison with department average
SELECT 
    e.name,
    e.salary,
    e.department,
    (e.salary > (
        SELECT AVG(salary) 
        FROM employees e2 
        WHERE e2.department = e.department
    )) as above_dept_average
FROM employees e;
```

- - -

## Joins

```sql
-- Employees with department details
SELECT 
    e.name,
    e.salary,
    d.dept_name,
    d.location
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;

-- Departments and employee count
SELECT 
    d.dept_name,
    COUNT(e.id) as employee_count
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_name;

-- Projects and assigned employees
SELECT 
    p.project_name,
    e.name as employee_name,
    ep.role,
    ep.hours_allocated
FROM projects p
JOIN employee_projects ep ON p.project_id = ep.project_id
JOIN employees e ON ep.employee_id = e.id;

-- Employees not assigned to projects
SELECT e.*
FROM employees e
LEFT JOIN employee_projects ep ON e.id = ep.employee_id
WHERE ep.employee_id IS NULL;

-- Departments with no projects
SELECT d.*
FROM departments d
LEFT JOIN projects p ON d.dept_id = p.dept_id
WHERE p.project_id IS NULL;
```

- - -

## Aggregation

```sql
-- Average salary by department
SELECT 
    department,
    ROUND(AVG(salary), 2) as avg_salary
FROM employees
GROUP BY department;

-- Department with highest total salary
SELECT 
    department,
    SUM(salary) as total_salary
FROM employees
GROUP BY department
ORDER BY total_salary DESC
LIMIT 1;

-- Projects per department
SELECT 
    d.dept_name,
    COUNT(p.project_id) as project_count
FROM departments d
LEFT JOIN projects p ON d.dept_id = p.dept_id
GROUP BY d.dept_name;

-- Total hours per project
SELECT 
    p.project_name,
    SUM(ep.hours_allocated) as total_hours
FROM projects p
LEFT JOIN employee_projects ep ON p.project_id = ep.project_id
GROUP BY p.project_name;

-- Departments with high average salary
SELECT 
    department,
    ROUND(AVG(salary), 2) as avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 70000;
```

- - -

## Views

```sql
-- Employee details view
CREATE VIEW employee_details AS
SELECT 
    e.name,
    e.salary,
    e.hire_date,
    d.dept_name,
    d.location
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;

-- Department salary statistics
CREATE VIEW department_stats AS
SELECT 
    d.dept_name,
    COUNT(e.id) as emp_count,
    ROUND(AVG(e.salary), 2) as avg_salary,
    MAX(e.salary) as max_salary,
    MIN(e.salary) as min_salary
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_name;

-- Active employees view
CREATE VIEW active_employees AS
SELECT * FROM employees WHERE is_active = true;

-- Project assignments view
CREATE VIEW project_assignments AS
SELECT 
    p.project_name,
    d.dept_name,
    COUNT(ep.employee_id) as assigned_employees,
    SUM(ep.hours_allocated) as total_hours
FROM projects p
JOIN departments d ON p.dept_id = d.dept_id
LEFT JOIN employee_projects ep ON p.project_id = ep.project_id
GROUP BY p.project_name, d.dept_name;
```

- - -

## Data Manipulation

```sql
-- Insert with 3 decimal places
INSERT INTO employees (
    name, 
    department, 
    salary, 
    hire_date, 
    is_active
)
VALUES (
    'New Employee', 
    'IT', 
    65000.123, 
    '2023-01-01', 
    true
);

-- Update department budgets
UPDATE departments
SET budget = budget * 1.1;

-- Delete inactive employees
DELETE FROM employees 
WHERE is_active = false;

-- Transfer employees
UPDATE employees
SET 
    department = 'Finance',
    dept_id = (SELECT dept_id FROM departments WHERE dept_name = 'Finance')
WHERE department = 'IT';

-- Update project hours
UPDATE employee_projects
SET hours_allocated = 140
WHERE role = 'Developer';
```

- - -


## `LIKE` Operator and Pattern Matching

```sql
-- Find names starting with 'J'
SELECT name 
FROM employees 
WHERE name LIKE 'J%';

-- Find names containing 'son'
SELECT name 
FROM employees 
WHERE name LIKE '%son%';

-- Find departments ending with 'ing'
SELECT dept_name 
FROM departments 
WHERE dept_name LIKE '%ing';

-- Find projects with 'System'
SELECT project_name 
FROM projects 
WHERE project_name LIKE '%System%';

-- Find names starting with vowels
SELECT name 
FROM employees 
WHERE name LIKE 'A%'
   OR name LIKE 'E%'
   OR name LIKE 'I%'
   OR name LIKE 'O%'
   OR name LIKE 'U%';

-- Alternative using regex
SELECT name 
FROM employees 
WHERE name ~* '^[aeiou]';

-- Find departments with exactly 2 chars before 'ing'
SELECT dept_name 
FROM departments 
WHERE dept_name LIKE '__ing';
```

- - - 

## CASCADE Delete and Referential Integrity
[Go to](https://github.com/nitin-singla/practice-postgres-11dec24/blob/main/solutions/cascade-del-referential-in-sol.md)
