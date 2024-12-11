# PostgreSQL Practice Exercises

First, run this setup code in your PostgreSQL database:

```sql
-- Create and populate employees table
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(100),
    salary NUMERIC(10,2),
    hire_date DATE,
    is_active BOOLEAN
);

INSERT INTO employees (name, department, salary, hire_date, is_active) VALUES
    ('John Doe', 'IT', 75000.50, '2020-01-15', true),
    ('Jane Smith', 'HR', 65000.75, '2019-03-20', true),
    ('Bob Johnson', 'Finance', 80000.25, '2018-11-05', true),
    ('Alice Brown', 'IT', 72000.80, '2020-08-30', true),
    ('Charlie Wilson', 'Marketing', 68000.90, '2021-04-12', true),
    ('Diana Miller', 'HR', 67000.45, '2019-07-25', false),
    ('Edward Davis', 'Finance', 85000.20, '2017-12-01', true),
    ('Fiona Clark', 'Marketing', 71000.60, '2020-09-15', true),
    ('George Harris', 'IT', 78000.30, '2018-06-30', true),
    ('Helen Martinez', 'HR', 69000.70, '2021-02-28', true),,
    ('James Anderson', 'IT', 71000.50, '2021-03-15', true),
    ('Jessica Wilson', 'Marketing', 69000.75, '2021-04-20', true),
    ('Johnson Miller', 'HR', 62000.25, '2021-05-05', true),
    ('Oliver Smith', 'IT', 73000.80, '2021-06-30', true),
    ('Emily Jackson', 'Marketing', 67000.90, '2021-07-12', true);;

-- Create and populate departments table with cascade delete
CREATE TABLE departments (
    dept_id SERIAL PRIMARY KEY,
    dept_name VARCHAR(100),
    location VARCHAR(100),
    budget NUMERIC(12,2)
);

INSERT INTO departments (dept_name, location, budget) VALUES
    ('IT', 'New York', 500000.00),
    ('HR', 'Chicago', 300000.00),
    ('Finance', 'Boston', 450000.00),
    ('Marketing', 'Los Angeles', 350000.00);

-- Create and populate projects table
CREATE TABLE projects (
    project_id SERIAL PRIMARY KEY,
    project_name VARCHAR(100),
    start_date DATE,
    end_date DATE,
    budget NUMERIC(12,2),
    dept_id INTEGER REFERENCES departments(dept_id)
);

INSERT INTO projects (project_name, start_date, end_date, budget, dept_id) VALUES
    ('Website Redesign', '2023-01-01', '2023-06-30', 100000.00, 1),
    ('HR System Update', '2023-02-15', '2023-12-31', 80000.00, 2),
    ('Financial Planning', '2023-03-01', '2024-02-28', 150000.00, 3),
    ('Marketing Campaign', '2023-04-01', '2023-09-30', 120000.00, 4),
    ('Database Migration', '2023-05-15', '2023-11-30', 90000.00, 1),
    ('CRM System 2.0', '2023-06-01', '2023-12-31', 95000.00, 1),
    ('Marketing System Pro', '2023-07-15', '2024-01-31', 85000.00, 4),
    ('Training System 365', '2023-08-01', '2024-03-31', 75000.00, 2);

-- Create and populate employee_projects table
CREATE TABLE employee_projects (
    employee_id INTEGER REFERENCES employees(id),
    project_id INTEGER REFERENCES projects(project_id),
    role VARCHAR(100),
    hours_allocated INTEGER,
    PRIMARY KEY (employee_id, project_id)
);

INSERT INTO employee_projects (employee_id, project_id, role, hours_allocated) VALUES
    (1, 1, 'Lead Developer', 160),
    (4, 1, 'Developer', 120),
    (9, 1, 'Developer', 120),
    (2, 2, 'Project Manager', 80),
    (6, 2, 'Analyst', 100),
    (3, 3, 'Financial Analyst', 140),
    (7, 3, 'Project Lead', 160),
    (5, 4, 'Marketing Specialist', 100),
    (8, 4, 'Content Creator', 80);

-- Add foreign key with cascade delete
ALTER TABLE employees 
ADD COLUMN dept_id INTEGER REFERENCES departments(dept_id) ON DELETE CASCADE;

-- Update employees with department IDs
UPDATE employees SET dept_id = 1 WHERE department = 'IT';
UPDATE employees SET dept_id = 2 WHERE department = 'HR';
UPDATE employees SET dept_id = 3 WHERE department = 'Finance';
UPDATE employees SET dept_id = 4 WHERE department = 'Marketing';
```

## Exercises
### Basic Queries
1. List all employees in the IT department
2. Find employees with salaries greater than 70000
3. List all active employees ordered by hire date
4. Find all employees hired between 2019 and 2020


### Logical Expressions
1. Find all employees who are either in IT department OR earn more than 70000
2. List employees who are both active AND hired after 2019
3. Find employees who are NOT in the Marketing department
4. (Advanced) Create a column showing TRUE if employee salary is above department average, FALSE otherwise
