

```markdown
# TCL (Transaction Control Language)

**Definition:**  
TCL is used to **control transactions** in a database.  
A transaction is a sequence of operations that are executed as a single logical unit.  
Transactions help ensure **data consistency and integrity**.

**Common TCL commands:**
- `BEGIN TRANSACTION` → Start a new transaction.  
- `COMMIT` → Save all changes made in the transaction.  
- `ROLLBACK` → Undo changes made in the transaction.  
- `SAVE TRANSACTION` → Set a savepoint within a transaction (so you can rollback partially).  

**Examples:**

```sql
-- Begin a transaction
BEGIN TRANSACTION;

-- Insert new data
INSERT INTO Employees (EmployeeID, FirstName, LastName, Salary, Department)
VALUES (3, 'Alice', 'Brown', 7000.00, 'Finance');

-- Commit the transaction (make changes permanent)
COMMIT;

-- Rollback example
BEGIN TRANSACTION;

UPDATE Employees
SET Salary = 10000.00
WHERE Department = 'HR';

-- Oops, something went wrong! Rollback
ROLLBACK;

-- Savepoint example
BEGIN TRANSACTION;

UPDATE Employees
SET Salary = 8000.00
WHERE EmployeeID = 2;

SAVE TRANSACTION SavePoint1;

UPDATE Employees
SET Salary = 9000.00
WHERE EmployeeID = 3;

-- Rollback to savepoint (undo second update only)
ROLLBACK TRANSACTION SavePoint1;

COMMIT;
