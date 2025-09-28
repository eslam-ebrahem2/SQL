# DDL (Data Definition Language)

**Definition:**  
DDL is used to **define and manage database structures** like tables, schemas, views, and indexes.  
It deals with the **structure** of the database, not the data itself.

**Common DDL commands:**
- `CREATE` → Create a new database object (table, database, index, etc.)  
- `ALTER` → Modify an existing object (add/modify columns, constraints, etc.)  
- `DROP` → Delete an object (table, database, index, etc.)  
- `TRUNCATE` → Remove all records from a table but keep the structure.  

**Examples:**

```sql
-- Create a new table
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Salary DECIMAL(10,2)
);

-- Alter the table (add a new column)
ALTER TABLE Employees
ADD Department VARCHAR(50);

-- Drop the table
DROP TABLE Employees;

-- Truncate table (delete all rows, keep structure)
TRUNCATE TABLE Employees;
