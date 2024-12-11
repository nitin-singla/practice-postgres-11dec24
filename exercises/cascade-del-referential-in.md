## CASCADE Delete and Referential Integrity

First, let's set up a more meaningful scenario:

```sql
-- Create tables with proper relationships
CREATE TABLE departments (
    dept_id SERIAL PRIMARY KEY,
    dept_name VARCHAR(100),
    location VARCHAR(100)
);

CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    salary NUMERIC(10,2),
    dept_id INTEGER REFERENCES departments(dept_id) ON DELETE CASCADE
);

CREATE TABLE equipment (
    equip_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    dept_id INTEGER REFERENCES departments(dept_id) ON DELETE CASCADE
);

CREATE TABLE employee_credentials (
    credential_id SERIAL PRIMARY KEY,
    emp_id INTEGER REFERENCES employees(emp_id) ON DELETE CASCADE,
    email VARCHAR(100),
    system_access_key VARCHAR(100)
);

-- Insert sample data
INSERT INTO departments (dept_name, location) VALUES
    ('IT', 'New York'),
    ('HR', 'Chicago'),
    ('Finance', 'Boston');

INSERT INTO employees (name, salary, dept_id) VALUES
    ('John Doe', 75000, 1),
    ('Jane Smith', 65000, 1),
    ('Bob Wilson', 70000, 2);

INSERT INTO equipment (name, dept_id) VALUES
    ('Laptop HP', 1),
    ('Printer Canon', 1),
    ('Scanner', 2);

INSERT INTO employee_credentials (emp_id, email, system_access_key) VALUES
    (1, 'john.doe@company.com', 'KEY123'),
    (2, 'jane.smith@company.com', 'KEY456'),
    (3, 'bob.wilson@company.com', 'KEY789');
```

## Exercises:

1. Examine the Cascade Effect
   - Check all equipment and employees in the IT department
   - Delete the IT department
   - Verify what happened to:
     - The employees who were in IT
     - The equipment assigned to IT
     - The credentials of IT employees

2. Test Referential Integrity
   - Try to add an employee with a non-existent department ID
   - Try to add equipment to a department that doesn't exist
   - Try to add credentials for a non-existent employee

3. Multiple Cascade Levels
   - Add a new department with employees and equipment
   - Add credentials for these new employees
   - Delete the department and track all cascading deletions

4. Partial Deletions
   - Delete a single employee
   - Check what happens to their credentials
   - Verify that other employees and department data remain intact

5. Department Merger Scenario
   - Create a new department "IT Operations"
   - Move all IT equipment to the new department
   - Delete the old IT department
   - Verify the state of all related data
