
# DQL (Data Query Language)

**Definition:**  
DQL is used to **query and retrieve data** from the database.  
It helps in reading data without modifying it.

**Main DQL command:**
- `SELECT` → Fetch data from one or more tables.  

**Examples:**

```sql
-- Select all columns
SELECT * FROM Employees;

-- Select specific columns
SELECT FirstName, LastName, Salary FROM Employees;

-- Select with condition
SELECT * FROM Employees
WHERE Department = 'IT';

-- Select with sorting
SELECT * FROM Employees
ORDER BY Salary DESC;

-- Select with aggregation
SELECT Department, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY Department;

-- Select with filtering after aggregation
SELECT Department, COUNT(*) AS NumEmployees
FROM Employees
GROUP BY Department
HAVING COUNT(*) > 5;
