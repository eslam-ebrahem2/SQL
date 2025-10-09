
# SQL Server Commands

## 📚 Table of Contents
- [DDL (Data Definition Language)](#ddl-data-definition-language)
- [DML (Data Manipulation Language)](#dml-data-manipulation-language)
- [DQL (Data-Query Language)](#dql-data-query-language)
- [DCL (Data Control Language)](#dcl-data-control-language)
- [TCL (Transaction Control Language)](#tcl-transaction-control-language)
- [Administrative Commands](#administrative-commands)
- [Common Built-in Functions](#common-built-in-functions)

---

## 🧱 DDL (Data Definition Language)

### CREATE DATABASE
```sql
CREATE DATABASE MyDatabase;
```

### ALTER DATABASE
```sql
ALTER DATABASE MyDatabase SET RECOVERY SIMPLE;
```

### DROP DATABASE
```sql
DROP DATABASE MyDatabase;
```

### CREATE TABLE
```sql
CREATE TABLE Students (
    ID INT PRIMARY KEY,
    Name NVARCHAR(50),
    Age INT
);
```

### ALTER TABLE
```sql
ALTER TABLE Students ADD Email NVARCHAR(100);
```

### DROP TABLE
```sql
DROP TABLE Students;
```

### TRUNCATE TABLE
```sql
TRUNCATE TABLE Students;
```

### CREATE INDEX
```sql
CREATE INDEX IX_Students_Name ON Students(Name);
```

### ALTER INDEX
```sql
ALTER INDEX IX_Students_Name ON Students REBUILD;
```

### DROP INDEX
```sql
DROP INDEX IX_Students_Name ON Students;
```

### CREATE VIEW
```sql
CREATE VIEW ActiveStudents AS
SELECT Name, Age FROM Students WHERE Age >= 18;
```

### ALTER VIEW
```sql
ALTER VIEW ActiveStudents AS
SELECT Name, Age, Email FROM Students WHERE Age >= 18;
```

### DROP VIEW
```sql
DROP VIEW ActiveStudents;
```

### CREATE PROCEDURE
```sql
CREATE PROCEDURE GetAllStudents
AS
SELECT * FROM Students;
```

### ALTER PROCEDURE
```sql
ALTER PROCEDURE GetAllStudents
AS
SELECT Name, Age FROM Students;
```

### DROP PROCEDURE
```sql
DROP PROCEDURE GetAllStudents;
```

### CREATE FUNCTION
```sql
CREATE FUNCTION GetStudentCount()
RETURNS INT
AS
BEGIN
    RETURN (SELECT COUNT(*) FROM Students);
END;
```

### DROP FUNCTION
```sql
DROP FUNCTION GetStudentCount;
```

### CREATE TRIGGER
```sql
CREATE TRIGGER trgAfterInsert
ON Students
AFTER INSERT
AS
BEGIN
    PRINT 'A new student has been added.';
END;
```

### DROP TRIGGER
```sql
DROP TRIGGER trgAfterInsert;
```

---

## 🧮 DML (Data Manipulation Language)

### INSERT INTO
```sql
INSERT INTO Students (ID, Name, Age)
VALUES (1, 'Ali', 22);
```

### UPDATE
```sql
UPDATE Students
SET Age = 23
WHERE ID = 1;
```

### DELETE
```sql
DELETE FROM Students
WHERE ID = 1;
```

### MERGE
```sql
MERGE INTO Students AS Target
USING NewStudents AS Source
ON Target.ID = Source.ID
WHEN MATCHED THEN
    UPDATE SET Target.Name = Source.Name
WHEN NOT MATCHED THEN
    INSERT (ID, Name) VALUES (Source.ID, Source.Name);
```

---

## 🔍 DQL (Data Query Language)

### SELECT
```sql
SELECT * FROM Students;
```

### SELECT DISTINCT
```sql
SELECT DISTINCT Name FROM Students;
```

### WHERE
```sql
SELECT * FROM Students WHERE Age > 20;
```

### ORDER BY
```sql
SELECT * FROM Students ORDER BY Name ASC;
```

### GROUP BY
```sql
SELECT Age, COUNT(*) FROM Students GROUP BY Age;
```

### HAVING
```sql
SELECT Age, COUNT(*) FROM Students GROUP BY Age HAVING COUNT(*) > 2;
```

### JOIN
```sql
SELECT s.Name, c.CourseName
FROM Students s
INNER JOIN Courses c ON s.ID = c.StudentID;
```

### UNION
```sql
SELECT Name FROM Students
UNION
SELECT TeacherName FROM Teachers;
```

### LIKE
```sql
SELECT * FROM Students WHERE Name LIKE 'A%';
```

### BETWEEN
```sql
SELECT * FROM Students WHERE Age BETWEEN 18 AND 25;
```

### IN
```sql
SELECT * FROM Students WHERE Age IN (18, 20, 22);
```

---

## 🔐 DCL (Data Control Language)

### GRANT
```sql
GRANT SELECT, INSERT ON Students TO User1;
```

### REVOKE
```sql
REVOKE INSERT ON Students FROM User1;
```

### DENY
```sql
DENY DELETE ON Students TO User1;
```

---

## 💾 TCL (Transaction Control Language)

### BEGIN TRANSACTION
```sql
BEGIN TRANSACTION;
INSERT INTO Students VALUES (2, 'Omar', 21);
```

### COMMIT
```sql
COMMIT TRANSACTION;
```

### ROLLBACK
```sql
ROLLBACK TRANSACTION;
```

### SAVE TRANSACTION
```sql
SAVE TRANSACTION SavePoint1;
```

---

## ⚙️ Administrative Commands

### USE
```sql
USE MyDatabase;
```

### EXEC
```sql
EXEC GetAllStudents;
```

### sp_help
```sql
EXEC sp_help 'Students';
```

### sp_rename
```sql
EXEC sp_rename 'Students.Name', 'FullName', 'COLUMN';
```

### DBCC CHECKDB
```sql
DBCC CHECKDB(MyDatabase);
```

### BACKUP DATABASE
```sql
BACKUP DATABASE MyDatabase TO DISK = 'C:\Backups\MyDatabase.bak';
```

### RESTORE DATABASE
```sql
RESTORE DATABASE MyDatabase FROM DISK = 'C:\Backups\MyDatabase.bak';
```

### SET
```sql
SET NOCOUNT ON;
```

### PRINT
```sql
PRINT 'Backup completed successfully.';
```

### GO
```sql
GO
```

### @@VERSION
```sql
SELECT @@VERSION;
```

---

## 🧮 Common Built-in Functions

### GETDATE
```sql
SELECT GETDATE();
```

### LEN
```sql
SELECT LEN('SQL Server');
```

### COUNT
```sql
SELECT COUNT(*) FROM Students;
```

### SUM
```sql
SELECT SUM(Age) FROM Students;
```

### AVG
```sql
SELECT AVG(Age) FROM Students;
```

### MIN
```sql
SELECT MIN(Age) FROM Students;
```

### MAX
```sql
SELECT MAX(Age) FROM Students;
```

### ISNULL
```sql
SELECT ISNULL(Email, 'No Email') FROM Students;
```

### CAST
```sql
SELECT CAST(Age AS VARCHAR(10)) FROM Students;
```

### CONVERT
```sql
SELECT CONVERT(VARCHAR(10), GETDATE(), 103);
```

### SUBSTRING
```sql
SELECT SUBSTRING(Name, 1, 3) FROM Students;
```

### UPPER / LOWER
```sql
SELECT UPPER(Name), LOWER(Name) FROM Students;
```

### ROUND
```sql
SELECT ROUND(123.4567, 2);
```

### COALESCE
```sql
SELECT COALESCE(Email, 'N/A') FROM Students;
```
