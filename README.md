# Employee-_project.sql
My second SQL project — Employee database schema with departments, jobs, and employees.
# Employee Project (SQL)

This is my second SQL project.  
I designed a database called **employee_project** to manage employees, jobs, and departments.

## Features
- Create database and tables
- Define relationships (Foreign Keys)
- Handle employee statuses (active, laid-off, resigned, retired)

## Technologies
- MySQL

## How to Run
1. Clone the repo
2. Run `employee_project.sql` in MySQL Workbench or any SQL client
-- =========================================
-- SQL Employee Project
-- Author: [Your Name]
-- Description: Employee Management Database
-- =========================================

-- 1. Create the database
CREATE DATABASE employee_project;
USE employee_project;

-- 2. Create Departments table
CREATE TABLE departments (
    department_id INT PRIMARY KEY AUTO_INCREMENT,
    dept_name VARCHAR(100) NOT NULL
);

-- 3. Create Jobs table
CREATE TABLE jobs (
    job_id INT PRIMARY KEY AUTO_INCREMENT,
    job_title VARCHAR(100) NOT NULL,
    min_salary DECIMAL(10,2),
    max_salary DECIMAL(10,2)
);

-- 4. Create Employees table
CREATE TABLE employees (
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    phone_number VARCHAR(20),
    hire_date DATE NOT NULL,
    job_id INT,
    department_id INT,
    salary DECIMAL(10,2),
    manager_id INT NULL,
    status ENUM('active','laid-off','resigned','retired') DEFAULT 'active',
    FOREIGN KEY (job_id) REFERENCES jobs(job_id),
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);

-- 5. Insert sample departments
INSERT INTO departments (dept_name) VALUES
('Human Resources'),
('Finance'),
('Engineering'),
('Marketing'),
('Sales');

-- 6. Insert sample jobs
INSERT INTO jobs (job_title, min_salary, max_salary) VALUES
('HR Manager', 40000, 80000),
('Software Engineer', 50000, 120000),
('Accountant', 35000, 70000),
('Marketing Specialist', 30000, 65000),
('Sales Executive', 28000, 60000);

-- 7. Insert sample employees
INSERT INTO employees (first_name, last_name, email, phone_number, hire_date, job_id, department_id, salary, manager_id, status) VALUES
('John', 'Doe', 'john.doe@company.com', '08012345678', '2020-01-15', 2, 3, 75000, NULL, 'active'),
('Mary', 'Smith', 'mary.smith@company.com', '08023456789', '2019-03-10', 1, 1, 65000, NULL, 'active'),
('James', 'Brown', 'james.brown@company.com', '08034567890', '2021-06-25', 5, 5, 45000, 1, 'active'),
('Linda', 'Johnson', 'linda.johnson@company.com', '08045678901', '2018-11-30', 3, 2, 58000, 2, 'laid-off'),
('David', 'Wilson', 'david.wilson@company.com', '08078901234', '2017-05-14', 4, 4, 40000, NULL, 'resigned');

-- =========================================
-- 8. Example Queries
-- =========================================

-- List all employees with their department and job
SELECT e.first_name, e.last_name, j.job_title, d.dept_name, e.salary, e.status
FROM employees e
JOIN jobs j ON e.job_id = j.job_id
JOIN departments d ON e.department_id = d.department_id;

-- Find employees earning above department average
SELECT e.first_name, e.last_name, e.salary, d.dept_name
FROM employees e
JOIN (
    SELECT department_id, AVG(salary) AS dept_avg
    FROM employees
    GROUP BY department_id
) da ON e.department_id = da.department_id
JOIN departments d ON e.department_id = d.department_id
WHERE e.salary > da.dept_avg;

-- Count employees by status
SELECT status, COUNT(*) AS total
FROM employees
GROUP BY status;   
