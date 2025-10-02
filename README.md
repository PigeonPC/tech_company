# Tech company
_____________________________________________________________________________________________
## SINGLE TABLE ASSIGNMENTS
**1. Find the employee number for employees named MARTIN.**
```sql
SELECT employee_number
FROM employees
WHERE employee_name = 'MARTIN';
```

**2. Find the employee(s) with a salary greater than 1500.**
```sql
SELECT employee_name
FROM employees
WHERE salary > 1500;
```

**3. List the names of salesmen that earn more than 1300.**
```sql
SELECT employee_name
FROM employees
WHERE job_title = 'SALESMAN'
  AND salary > 1300;
```

**4. List the names of employees that are not salesmen.**
```sql
SELECT employee_name
FROM employees
WHERE job_title <> 'SALESMAN';
```

**5. List the names of all clerks together with their salary with a deduction of 10%.**
```sql
SELECT employee_name, salary * 0.90 AS salary_minus_10pct
FROM employees
WHERE job_title = 'CLERK';
```

**6. Find the name of employees hired before May 1981.**
```sql
SELECT employee_name
FROM employees
WHERE hire_date > '1981-05-01';
```

**7. List employees sorted by salary in descending order (i.e. highest salary first).**
```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC, employee_name ASC;
```

**8. List departments sorted by location.**
```sql
SELECT department_name, office_location
FROM departments
ORDER BY office_location ASC;
```

**9. Find name of the department located in New York.**
```sql
SELECT department_name
FROM departments
WHERE office_location = 'NEW YORK';
```

**10. You have proven your worth at the company. Your colleague comes to you trying to remember what's-his-name. It starts with a J and ends with S. Can you help her?**
```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE 'J%S'
ORDER BY employee_name;
```

**11. Maybe that wasn't helpful. "Oh yeah, I remember now!" they say and tell you that he is a manager.**
```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE 'J%S'
  AND job_title = 'MANAGER';
```

**12. How many employees are there in each department?**
*Okay wow, det var virkelig svært..*
```sql
SELECT departments.department_name,
       COUNT(employees.employee_number) AS employee_count
FROM departments
         LEFT JOIN employees
                   ON employees.department_number = departments.department_number
GROUP BY departments.department_number, departments.department_name
ORDER BY departments.department_number;
```

_____________________________________________________________________________________________
## AGGREGATE FUNCTIONS
**2. List the number of employees.**
```sql
SELECT COUNT(employee_number) AS employee_count
FROM employees;
```

**3. List the sum of all salaries (excluding commission).**
```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

**4. List the average salary for employees in department 20.**
```sql
SELECT AVG(salary) AS average_salary
FROM employees
WHERE department_number = 20;
```

**5. List the unique job titles in the company.**
```sql
SELECT DISTINCT job_title
FROM employees
ORDER BY job_title;
```

**6. List the number of employees in each department.**
```sql
SELECT departments.department_name,
       COUNT(employees.employee_number) AS employee_count
FROM departments
         LEFT JOIN employees
                   USING (department_number)
GROUP BY department_number, department_name
ORDER BY department_name;
```

**7. List in decreasing order the maximum salary in each department together with the department number.**
```sql
SELECT departments.department_number,
       MAX(employees.salary) AS max_salary
FROM departments
         LEFT JOIN employees USING (department_number)
GROUP BY departments.department_number
ORDER BY max_salary DESC, department_number;
```

**8. List total sum of salary and commission for all employees.**
```sql
SELECT SUM(salary) + SUM(commission) AS salary_commission_sum
FROM employees;
```

_____________________________________________________________________________________________
## JOIN ASSIGNMENTS
**1. Create an INNER JOIN between employees and departments to get the department name for each employee. Show all columns.**
```sql
SELECT employees.*, departments.department_name
FROM employees
         INNER JOIN departments
                    ON employees.department_number = departments.department_number;
```

**2. Continue from the last task. Show two columns. The employee_name and their corresponding department_name. Oh, and can you sort them alphabetically (A-Z)?**
```sql
SELECT employees.employee_name, departments.department_name
FROM employees
         INNER JOIN departments
                    ON employees.department_number = departments.department_number
ORDER BY employee_name;
```

**3. Now is the time to make a LEFT JOIN. Let's look at employee_name and department_name only. There is one more person this time who didn't show in the previous query. Who is it and why?**
```sql
SELECT employees.employee_name, departments.department_name
FROM employees
         LEFT JOIN departments
                   ON employees.department_number = departments.department_number
ORDER BY employee_name;
```
*King var ikke med, fordi han ikke er i et department, men er president.
Med INNER JOIN var det kun de employees hvor der var et match mellem tabellerne
Med LEFT JOIN kommer alle rækkerne fra venstre med, også dem, der giver null, også KING der ikke har et department_number*

**4. Consider this query:**
```sql
SELECT departments.department_name, COUNT(employees.employee_number)
FROM employees
         JOIN departments
              ON departments.department_number = employees.department_number
GROUP BY department_name;
```
*Der er ingen employees i OPERATIONS, derfor dukker de ikke op i COUNT*

**5. To get the missing department change the previous query to use a RIGHT JOIN.**
```sql
SELECT departments.department_name, COUNT(employees.employee_number)
FROM employees
         RIGHT JOIN departments
                    ON departments.department_number = employees.department_number
GROUP BY department_name;
```

**6. SCOTT sends you this query and asks you to run it. In order to assess whether it is information that SCOTT is privy to, you must first understand it. Describe in technical terms what this query does:**
```sql
SELECT *
FROM employees employee
         JOIN employees manager
              ON employee.manager_id = manager.employee_number
ORDER BY employee.employee_name;
```
*Query joiner employee manager til de employees de er manager for.
Der laves en join på samme tabel, så al info om eployees bliver vist, samt info om hver deres managers.*

**7. Get two columns: employees and their managers.**
```sql
SELECT employees.employee_name AS employee,
       managers.employee_name  AS manager
FROM employees
         LEFT JOIN employees AS managers
                   ON employees.manager_id = managers.employee_number
ORDER BY employees.employee_name;
```

**8. Use the HAVING keyword (feel free to look it up) to show the departments with more than 3 employees. The as number_of_employees is so that you can reference the value later on in the query:**
```sql
SELECT department_number,
       COUNT(employees.department_number) AS number_of_employees
FROM employees
GROUP BY department_number
HAVING number_of_employees > 3;
```

**9. Subquery time! Select the name and salary of employees whose salary is above average:**
```sql
WHERE salary > (SELECT AVG(salary) FROM employees)
SELECT employee_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

_____________________________________________________________________________________________
# JOIN TABLE (MANY-TO-MANY)
**Create a new table called leaders and insert rows into it.**
*Sorry Anders, det er for svært... Kan ikke finde ud af det, så den sidste del er AI...
Tænker, du nok ikke er super tilfreds med det.. men sådan blev det. Vil heller have en færdig opgave, som jeg kan bruge og referere til senere.*
```sql
DROP TABLE IF EXISTS employees_leaders;
DROP TABLE IF EXISTS leaders;

CREATE TABLE leaders
(
    leader_number INT PRIMARY KEY,
    leader_name   VARCHAR(30) NOT NULL
);

INSERT INTO leaders (leader_number, leader_name)
VALUES (7839, 'KING'),
       (7566, 'JONES'),
       (7698, 'BLAKE'),
       (7782, 'CLARK');

CREATE TABLE employees_leaders
(
    employee_number INT,
    leader_number   INT,
    PRIMARY KEY (employee_number, leader_number),
    FOREIGN KEY (employee_number)
        REFERENCES employees (employee_number),
    FOREIGN KEY (leader_number)
        REFERENCES leaders (leader_number)
);
```

**Link employees til deres leder(e)**
```sql
INSERT INTO employees_leaders (employee_number, leader_number)
VALUES (7369, 7566),
       (7369, 7839), -- SMITH  -> JONES, KING
       (7499, 7698),
       (7499, 7839), -- ALLEN  -> BLAKE, KING
       (7521, 7698),
       (7521, 7839), -- WARD   -> BLAKE, KING
       (7566, 7839), -- JONES  -> KING
       (7654, 7698),
       (7654, 7839), -- MARTIN -> BLAKE, KING
       (7698, 7839), -- BLAKE  -> KING
       (7782, 7839), -- CLARK  -> KING
       (7788, 7566),
       (7788, 7839), -- SCOTT  -> JONES, KING
       -- KING (7839) har ingen leder
       (7844, 7698),
       (7844, 7839), -- TURNER -> BLAKE, KING
       (7876, 7566),
       (7876, 7839), -- ADAMS  -> JONES, KING
       (7900, 7698),
       (7900, 7839), -- JAMES  -> BLAKE, KING
       (7902, 7566),
       (7902, 7839), -- FORD   -> JONES, KING
       (7934, 7782),
       (7934, 7839); -- MILLER -> CLARK, KING
```

**Many-to-many query med to JOINs**
```sql
SELECT employees.employee_name AS employee,
       leaders.leader_name     AS leader
FROM employees
         JOIN employees_leaders
              ON employees.employee_number = employees_leaders.employee_number
         JOIN leaders
              ON employees_leaders.leader_number = leaders.leader_number
ORDER BY employees.employee_name, leaders.leader_name;
```
