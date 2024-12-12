## CASCADE Delete and Referential Integrity - Solutions

Note: You may ignore the `BEGIN` & `COMMIT` to avoid any confusions.

```sql
-- 1. Examine the Cascade Effect
-- Check initial state
SELECT * FROM departments WHERE dept_name = 'IT';
SELECT * FROM employees WHERE dept_id = 1;
SELECT * FROM equipment WHERE dept_id = 1;
SELECT ec.* 
FROM employee_credentials ec
JOIN employees e ON ec.emp_id = e.emp_id
WHERE e.dept_id = 1;

-- Delete IT department
DELETE FROM departments WHERE dept_name = 'IT';

-- Verify cascading effects
SELECT * FROM employees WHERE dept_id = 1;  -- Should return no rows
SELECT * FROM equipment WHERE dept_id = 1;  -- Should return no rows
SELECT * FROM employee_credentials 
WHERE emp_id IN (1, 2);  -- Should return no rows

-- 2. Test Referential Integrity
-- Try invalid department (should fail)
INSERT INTO employees (name, salary, dept_id)
VALUES ('Test Employee', 60000, 999);

-- Try invalid equipment assignment (should fail)
INSERT INTO equipment (name, dept_id)
VALUES ('New Printer', 999);

-- Try invalid employee credentials (should fail)
INSERT INTO employee_credentials (emp_id, email, system_access_key)
VALUES (999, 'test@company.com', 'KEY999');

-- 3. Multiple Cascade Levels
-- Add new department with related data
BEGIN;
INSERT INTO departments (dept_name, location)
VALUES ('Research', 'Seattle')
RETURNING dept_id;  -- Let's say this returns 4

INSERT INTO employees (name, salary, dept_id)
VALUES 
    ('Sarah Connor', 72000, 4),
    ('Kyle Reese', 71000, 4);

INSERT INTO equipment (name, dept_id)
VALUES 
    ('Research Lab Equipment', 4),
    ('Testing Devices', 4);

INSERT INTO employee_credentials 
    (emp_id, email, system_access_key)
VALUES 
    ((SELECT emp_id FROM employees WHERE name = 'Sarah Connor'),
     'sarah.connor@company.com', 'KEY321'),
    ((SELECT emp_id FROM employees WHERE name = 'Kyle Reese'),
     'kyle.reese@company.com', 'KEY654');

-- Delete department and verify cascade
DELETE FROM departments WHERE dept_name = 'Research';

-- Verify all related data is deleted
SELECT * FROM employees WHERE dept_id = 4;
SELECT * FROM equipment WHERE dept_id = 4;
SELECT * FROM employee_credentials 
WHERE emp_id IN (
    SELECT emp_id FROM employees WHERE dept_id = 4
);
COMMIT;

-- 4. Partial Deletions
-- Delete single employee
DELETE FROM employees 
WHERE name = 'Bob Wilson';

-- Check credentials (should be deleted)
SELECT * FROM employee_credentials 
WHERE emp_id = 3;

-- Verify other data remains
SELECT * FROM departments WHERE dept_id = 2;
SELECT * FROM employees WHERE dept_id = 2;
SELECT * FROM equipment WHERE dept_id = 2;

-- 5. Department Merger Scenario
BEGIN;
-- Create new department
INSERT INTO departments (dept_name, location)
VALUES ('IT Operations', 'New York')
RETURNING dept_id;  -- Let's say this returns 5

-- Move equipment to new department
UPDATE equipment 
SET dept_id = 5
WHERE dept_id = 1;

-- Delete old department
DELETE FROM departments WHERE dept_name = 'IT';

-- Verify final state
SELECT * FROM departments;
SELECT * FROM equipment WHERE dept_id = 5;
SELECT * FROM employees WHERE dept_id = 5;
COMMIT;
```
