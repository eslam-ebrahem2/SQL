#function
- scaler function

``` sql
CREATE FUNCTION dbo.GetFullName(@FirstName NVARCHAR(50), @LastName NVARCHAR(50))
RETURNS NVARCHAR(100)
AS
BEGIN
    RETURN (@FirstName + ' ' + @LastName)
END

```

use
``` sql
SELECT dbo.GetFullName('Islam', 'Ahmed');
```


- Inline Table-Valued Function

``` sql
CREATE FUNCTION dbo.GetEmployeesByDept(@DeptId INT)
RETURNS TABLE
AS
RETURN
(
    SELECT EmployeeID, Name 
    FROM Employees 
    WHERE DepartmentID = @DeptId
);
```

use
``` sql
SELECT * FROM dbo.GetEmployeesByDept(1);
``` 

 - Multi-statement Table-Valued Function

 ```sql
CREATE FUNCTION dbo.GetHighSalaryEmployees(@MinSalary DECIMAL(10,2))
RETURNS @Result TABLE (EmpID INT, EmpName NVARCHAR(50), Salary DECIMAL(10,2))
AS
BEGIN
    INSERT INTO @Result
    SELECT EmployeeID, Name, Salary
    FROM Employees
    WHERE Salary > @MinSalary;

    RETURN;
END


 ```
 use 
  scema.fun()


 ``` sql
SELECT * FROM dbo.GetHighSalaryEmployees(5000);

 ```
### Important Notes

Functions cannot perform data modification (INSERT, UPDATE, DELETE) on base tables directly.

##### Functions can be used inside:

SELECT

WHERE

JOIN

#### Very useful for:

Reusable calculations.

Filters.

Encapsulating business logic.

#### Summary

###### Scalar Function  → Returns a single value.

###### Inline TVF → Returns a table with one query.

###### Multi-statement TVF → Returns a table with multiple statements.