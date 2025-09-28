# شرح شامل لأنواع أوامر SQL (Transact-SQL)

## 1. DDL (Data Definition Language) - لغة تعريف البيانات
**الغرض**: إنشاء، تعديل، حذف هيكل قاعدة البيانات (الجداول، الفهارس، المعارض، إلخ).

### أوامر DDL الرئيسية:

#### CREATE - إنشاء كائن جديد
```sql
-- إنشاء قاعدة بيانات
CREATE DATABASE SchoolDB;

-- استخدام قاعدة البيانات
USE SchoolDB;

-- إنشاء جدول
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    FirstName NVARCHAR(50) NOT NULL,
    LastName NVARCHAR(50) NOT NULL,
    BirthDate DATE,
    Email NVARCHAR(100)
);

-- إنشاء جدول آخر مرتبط
CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseName NVARCHAR(100) NOT NULL,
    Credits INT
);
```

#### ALTER - تعديل كائن موجود
```sql
-- إضافة عمود جديد
ALTER TABLE Students ADD PhoneNumber NVARCHAR(15);

-- تعديل نوع العمود
ALTER TABLE Students ALTER COLUMN Email NVARCHAR(150);

-- إضافة constraint
ALTER TABLE Students ADD CONSTRAINT CHK_BirthDate 
CHECK (BirthDate < GETDATE());

-- إضافة مفتاح خارجي
CREATE TABLE Enrollments (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    CourseID INT,
    EnrollmentDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);
```

#### DROP - حذف كائن
```sql
-- حذف جدول
DROP TABLE Enrollments;

-- حذق قاعدة بيانات
DROP DATABASE SchoolDB;
```

#### TRUNCATE - حذف جميع البيانات من جدول مع الحفاظ على الهيكل
```sql
TRUNCATE TABLE Students;
```

## 2. DML (Data Manipulation Language) - لغة معالجة البيانات
**الغرض**: إدراج، تحديث، حذف، واسترجاع البيانات من الجداول.

### أوامر DML الرئيسية:

#### INSERT - إدراج بيانات جديدة
```sql
-- إدراج بيانات في جدول الطلاب
INSERT INTO Students (StudentID, FirstName, LastName, BirthDate, Email)
VALUES 
(1, 'أحمد', 'محمد', '2000-05-15', 'ahmed@email.com'),
(2, 'فاطمة', 'علي', '2001-08-22', 'fatima@email.com'),
(3, 'يوسف', 'حسن', '1999-12-10', 'youssef@email.com');

-- إدراج بيانات في جدول المواد
INSERT INTO Courses (CourseID, CourseName, Credits)
VALUES 
(101, 'قواعد البيانات', 3),
(102, 'البرمجة', 4),
(103, 'الرياضيات', 3);
```

#### UPDATE - تحديث بيانات موجودة
```sql
-- تحديث بريد إلكتروني لطالب محدد
UPDATE Students 
SET Email = 'ahmed.new@email.com'
WHERE StudentID = 1;

-- تحديث عدة حقول مرة واحدة
UPDATE Students 
SET PhoneNumber = '0123456789', 
    LastName = 'السيد'
WHERE StudentID = 2;
```

#### DELETE - حذف بيانات
```sql
-- حذف طالب محدد
DELETE FROM Students 
WHERE StudentID = 3;

-- حذف جميع الطلاب الذين ولدوا قبل سنة 2000
DELETE FROM Students 
WHERE YEAR(BirthDate) < 2000;
```

## 3. DQL (Data Query Language) - لغة استعلام البيانات
**الغرض**: استرجاع البيانات من قاعدة البيانات.

### أمر DQL الرئيسي: SELECT

#### استعلامات أساسية:
```sql
-- استعراض جميع البيانات
SELECT * FROM Students;

-- استعراض أعمدة محددة
SELECT StudentID, FirstName, LastName 
FROM Students;

-- مع شرط WHERE
SELECT * FROM Students 
WHERE YEAR(BirthDate) > 2000;

-- مع ORDER BY للترتيب
SELECT * FROM Students 
ORDER BY LastName, FirstName;

-- مع GROUP BY للتجميع
SELECT YEAR(BirthDate) AS BirthYear, COUNT(*) AS StudentCount
FROM Students
GROUP BY YEAR(BirthDate);
```

#### استعلامات متقدمة مع JOIN:
```sql
-- INNER JOIN بين الجداول
SELECT 
    s.FirstName,
    s.LastName,
    c.CourseName,
    e.EnrollmentDate
FROM Students s
INNER JOIN Enrollments e ON s.StudentID = e.StudentID
INNER JOIN Courses c ON e.CourseID = c.CourseID;

-- LEFT JOIN
SELECT 
    s.FirstName,
    s.LastName,
    c.CourseName
FROM Students s
LEFT JOIN Enrollments e ON s.StudentID = e.StudentID
LEFT JOIN Courses c ON e.CourseID = c.CourseID;
```

#### دوال تجميعية:
```sql
SELECT 
    COUNT(*) AS TotalStudents,
    AVG(YEAR(GETDATE()) - YEAR(BirthDate)) AS AvgAge,
    MIN(BirthDate) AS OldestStudent,
    MAX(BirthDate) AS YoungestStudent
FROM Students;
```

## 4. TCL (Transaction Control Language) - لغة تحكم المعاملات
**الغرض**: التحكم في المعاملات لضمان تكامل البيانات.

### أوامر TCL الرئيسية:

#### BEGIN TRANSACTION - بدء معاملة
```sql
BEGIN TRANSACTION;
```

#### COMMIT - تأكيد التغييرات
```sql
BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 1000 
WHERE AccountID = 1;

UPDATE Accounts SET Balance = Balance + 1000 
WHERE AccountID = 2;

COMMIT TRANSACTION;
```

#### ROLLBACK - تراجع عن التغييرات
```sql
BEGIN TRANSACTION;

DELETE FROM Students 
WHERE StudentID = 1;

-- إذا أردنا التراجع عن الحذف
ROLLBACK TRANSACTION;
```

#### SAVE TRANSACTION - نقطة حفظ
```sql
BEGIN TRANSACTION;

INSERT INTO Students VALUES (4, 'مريم', 'خالد', '2002-03-15', 'mariam@email.com');
SAVE TRANSACTION SavePoint1;

UPDATE Students SET Email = 'test@email.com' WHERE StudentID = 2;
-- يمكن الرجوع لنقطة الحفظ
ROLLBACK TRANSACTION SavePoint1;

COMMIT TRANSACTION;
```

## مثال شامل يدمج جميع الأنواع:

```sql
-- DDL: إنشاء قاعدة بيانات وجداول
CREATE DATABASE BankSystem;
USE BankSystem;

CREATE TABLE Accounts (
    AccountID INT PRIMARY KEY,
    AccountHolder NVARCHAR(100),
    Balance DECIMAL(15,2)
);

CREATE TABLE Transactions (
    TransactionID INT PRIMARY KEY IDENTITY(1,1),
    FromAccount INT,
    ToAccount INT,
    Amount DECIMAL(15,2),
    TransactionDate DATETIME DEFAULT GETDATE()
);

-- DML: إدراج بيانات
INSERT INTO Accounts VALUES 
(1, 'أحمد محمد', 5000.00),
(2, 'فاطمة علي', 3000.00);

-- TCL: معاملة تحويل أموال
BEGIN TRANSACTION;

BEGIN TRY
    UPDATE Accounts SET Balance = Balance - 1000 WHERE AccountID = 1;
    UPDATE Accounts SET Balance = Balance + 1000 WHERE AccountID = 2;
    
    INSERT INTO Transactions (FromAccount, ToAccount, Amount)
    VALUES (1, 2, 1000.00);
    
    COMMIT TRANSACTION;
    PRINT 'تمت العملية بنجاح';
END TRY
BEGIN CATCH
    ROLLBACK TRANSACTION;
    PRINT 'فشلت العملية، تم التراجع';
END CATCH;

-- DQL: استعراض النتائج
SELECT * FROM Accounts;
SELECT * FROM Transactions;
```

## ملخص الاختلافات:

| النوع | الغرض | الأوامر |
|------|------|---------|
| DDL | تعريف هيكل البيانات | CREATE, ALTER, DROP, TRUNCATE |
| DML | معالجة البيانات | INSERT, UPDATE, DELETE |
| DQL | استعلام البيانات | SELECT |
| TCL | التحكم في المعاملات | BEGIN, COMMIT, ROLLBACK, SAVE |

هذا الشرغط يغطي جميع جوانب Transact-SQL الأساسية مع أمثلة عملية لكل نوع من الأوامر.