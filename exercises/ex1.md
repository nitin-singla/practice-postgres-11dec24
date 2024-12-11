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

- - - 
  
### Logical Expressions
1. Find all employees who are either in IT department OR earn more than 70000
2. List employees who are both active AND hired after 2019
3. Find employees who are NOT in the Marketing department
4. (Advanced) Create a column showing TRUE if employee salary is above department average, FALSE otherwise

- - - 

### Joins
> Before doing the below section on Joins, go through [this tutorial](https://neon.tech/postgresql/postgresql-tutorial/postgresql-joins).

1. List all employees with their department details
2. Show all departments and the number of employees in each
3. Find all projects and their assigned employees
4. List employees who haven't been assigned to any project
5. Show departments that have no projects

- - - 
  
### Aggregation
> Before doing the below section on Aggregation, go through [this tutorial](https://neon.tech/postgresql/postgresql-aggregate-functions).

1. Calculate the average salary by department
2. Find the department with the highest total salary
3. Count the number of projects per department
4. Calculate the total hours allocated per project
5. Find departments where the average salary is above 70000

- - - 
  
### Views
1. Create a view showing employee details along with their department information
2. Create a view showing department-wise salary statistics
3. Create a view for active employees only
4. (Advanced) Create a view showing project assignments and total hours

- - - 
  
### Data Manipulation
1. Insert a new employee with salary having 3 decimal places
2. Update all department budgets by adding 10%
3. Delete all inactive employees
4. Transfer all employees from IT to Finance department
5. Update project hours for all developers to 140

- - - 
  
### `LIKE` Operator and Pattern Matching
1. Find all employees whose names start with 'J'
2. Find all employees whose names contain 'son'
3. Find departments that end with 'ing'
4. Find projects that have the word 'System' anywhere in the name
5. Find employees whose names start with any vowel
6. Find departments that have exactly 2 characters before 'ing'
7. Find projects where names contain numbers

- - - 
  
### CASCADE Delete and Referential Integrity
[Go to](https://github.com/nitin-singla/practice-postgres-11dec24/blob/main/exercises/cascade-del-referential-in.md)
