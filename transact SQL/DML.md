


```markdown
# DML (Data Manipulation Language)

**Definition:**  
DML is used to **manipulate and manage data** stored in database objects.  
It allows inserting, updating, and deleting data.

**Common DML commands:**
- `INSERT` → Add new records.  
- `UPDATE` → Modify existing records.  
- `DELETE` → Remove records.  
- `MERGE` → Insert, update, or delete depending on conditions.  

**Examples:**

```sql
-- Insert a new record
INSERT INTO Employees (EmployeeID, FirstName, LastName, Salary, Department)
VALUES (1, 'John', 'Doe', 5000.00, 'IT');

-- Update a record
UPDATE Employees
SET Salary = 6000.00
WHERE EmployeeID = 1;

-- Delete a record
DELETE FROM Employees
WHERE EmployeeID = 1;

-- Merge example (update if exists, insert if not)
MERGE Employees AS target
USING (SELECT 2 AS EmployeeID, 'Jane' AS FirstName, 'Smith' AS LastName, 4500.00 AS Salary, 'HR' AS Department) AS source
ON (target.EmployeeID = source.EmployeeID)
WHEN MATCHED THEN
    UPDATE SET Salary = source.Salary
WHEN NOT MATCHED THEN
    INSERT (EmployeeID, FirstName, LastName, Salary, Department)
    VALUES (source.EmployeeID, source.FirstName, source.LastName, source.Salary, source.Department);
