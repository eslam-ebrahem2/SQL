

# شرح  لأنواع أوامر SQL (Transact-SQL)

## 📋 فهرس المحتويات
1. [مقدمة عن SQL](#مقدمة-عن-sql)
2. [DDL - لغة تعريف البيانات](#ddl---لغة-تعريف-البيانات)
3. [DML - لغة معالجة البيانات](#dml---لغة-معالجة-البيانات)
4. [DQL - لغة استعلام البيانات](#dql---لغة-استعلام-البيانات)
5. [TCL - لغة تحكم المعاملات](#tcl---لغة-تحكم-المعاملات)
6. [ملخص الاختلافات](#ملخص-الاختلافات)

## 🚀 مقدمة عن SQL

**SQL (Structured Query Language)** هي لغة قياسية للتعامل مع قواعد البيانات العلائقية. تنقسم أوامر SQL إلى عدة أنواع حسب الوظيفة.

---

## 🏗️ DDL - لغة تعريف البيانات
**الغرض**: إنشاء، تعديل، حذف هيكل قاعدة البيانات (الجداول، الفهارس، المعارض، إلخ)

### ✨ أوامر DDL الرئيسية

### CREATE - إنشاء كائن جديد
<div dir="ltr">

```sql
-- إنشاء قاعدة بيانات
CREATE DATABASE SchoolDB;

-- استخدام قاعدة البيانات
USE SchoolDB;

-- إنشاء جدول الطلاب
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    FirstName NVARCHAR(50) NOT NULL,
    LastName NVARCHAR(50) NOT NULL,
    BirthDate DATE,
    Email NVARCHAR(100)
);

-- إنشاء جدول المواد
CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseName NVARCHAR(100) NOT NULL,
    Credits INT
);
```
</div>

### ALTER - تعديل كائن موجود
<div dir="ltr">

```sql
-- إضافة عمود جديد
ALTER TABLE Students ADD PhoneNumber NVARCHAR(15);

-- تعديل نوع العمود
ALTER TABLE Students ALTER COLUMN Email NVARCHAR(150);

-- إضافة constraint
ALTER TABLE Students ADD CONSTRAINT CHK_BirthDate 
CHECK (BirthDate < GETDATE());

-- إنشاء جدول التسجيلات مع المفاتيح الخارجية
CREATE TABLE Enrollments (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    CourseID INT,
    EnrollmentDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);
```
</div>

### DROP - حذف كائن
<div dir="ltr">

```sql
-- حذف جدول
DROP TABLE Enrollments;

-- حذف قاعدة بيانات
DROP DATABASE SchoolDB;
```
</div>

### TRUNCATE - حذف جميع البيانات
<div dir="ltr">

```sql
TRUNCATE TABLE Students;
```
</div>

---

## 💾 DML - لغة معالجة البيانات
**الغرض**: إدراج، تحديث، حذف، واسترجاع البيانات من الجداول

### ✨ أوامر DML الرئيسية

### INSERT - إدراج بيانات جديدة
<div dir="ltr">

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
</div>

### UPDATE - تحديث بيانات موجودة
<div dir="ltr">

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
</div>

### DELETE - حذف بيانات
<div dir="ltr">

```sql
-- حذف طالب محدد
DELETE FROM Students 
WHERE StudentID = 3;

-- حذف جميع الطلاب الذين ولدوا قبل سنة 2000
DELETE FROM Students 
WHERE YEAR(BirthDate) < 2000;
```
</div>

---

## 🔍 DQL - لغة استعلام البيانات
**الغرض**: استرجاع البيانات من قاعدة البيانات

### ✨ أمر DQL الرئيسي: SELECT

### استعلامات أساسية
<div dir="ltr">

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
</div>

### استعلامات متقدمة مع JOIN
<div dir="ltr">

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
</div>

### دوال تجميعية
<div dir="ltr">

```sql
SELECT 
    COUNT(*) AS TotalStudents,
    AVG(YEAR(GETDATE()) - YEAR(BirthDate)) AS AvgAge,
    MIN(BirthDate) AS OldestStudent,
    MAX(BirthDate) AS YoungestStudent
FROM Students;
```
</div>

---

## ⚡ TCL - لغة تحكم المعاملات
**الغرض**: التحكم في المعاملات لضمان تكامل البيانات

### ✨ أوامر TCL الرئيسية

### BEGIN TRANSACTION - بدء معاملة
<div dir="ltr">

```sql
BEGIN TRANSACTION;
```
</div>

### COMMIT - تأكيد التغييرات
<div dir="ltr">

```sql
BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 1000 
WHERE AccountID = 1;

UPDATE Accounts SET Balance = Balance + 1000 
WHERE AccountID = 2;

COMMIT TRANSACTION;
```
</div>

### ROLLBACK - تراجع عن التغييرات
<div dir="ltr">

```sql
BEGIN TRANSACTION;

DELETE FROM Students 
WHERE StudentID = 1;

-- إذا أردنا التراجع عن الحذف
ROLLBACK TRANSACTION;
```
</div>

### SAVE TRANSACTION - نقطة حفظ
<div dir="ltr">

```sql
BEGIN TRANSACTION;

INSERT INTO Students VALUES (4, 'مريم', 'خالد', '2002-03-15', 'mariam@email.com');
SAVE TRANSACTION SavePoint1;

UPDATE Students SET Email = 'test@email.com' WHERE StudentID = 2;
-- يمكن الرجوع لنقطة الحفظ
ROLLBACK TRANSACTION SavePoint1;

COMMIT TRANSACTION;
```
</div>

---

## 🎯 مثال شامل يدمج جميع الأنواع
<div dir="ltr">

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
</div>

---

## 📊 ملخص الاختلافات

| النوع | الاختصار | الغرض | الأوامر الرئيسية |
|------|---------|------|-----------------|
| **DDL** | Data Definition Language | تعريف هيكل البيانات | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| **DML** | Data Manipulation Language | معالجة البيانات | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** | Data Query Language | استعلام البيانات | `SELECT` |
| **TCL** | Transaction Control Language | التحكم في المعاملات | `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVE` |




---

