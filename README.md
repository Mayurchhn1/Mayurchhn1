👋 Hey there, I'm @Mayurchhn1!
- 👀 Dive into my intriguing interests...
- 🌱 Currently optimizing my skills in...
- 💞️ Ready for eye-catching collaborations on...
- 📫 Reach me effortlessly at...
<!---
Mayurchhn1/Mayurchhn1 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.

```sql
-- 1. Create database
create database SaleOrder

-- 2. Use the database
use SaleOrder

-- 3. Create tables
create table dbo.customer (
    CustomerID int NOT null primary key,
    CustomerFirstName varchar(50) NOT null,
    CustomerLastName varchar(50) NOT null,
    CustomerAddress varchar(50) NOT null,
    CustomerSuburb varchar(50) null,
    CustomerCity varchar(50) NOT null,
    CustomerPostCode char(4) null,
    CustomerPhoneNumber char(12) null
);

create table dbo.inventory (
    InventoryID tinyint NOT null primary key,
    InventoryName varchar(50) NOT null,
    InventoryDescription varchar(255) null
);

create table dbo.employee (
    EmployeeID tinyint NOT null primary key,
    EmployeeFirstName varchar(50) NOT null,
    EmployeeLastName varchar(50) NOT null,
    EmployeeExtension char(4) null
);

create table dbo.sale (
    SaleID tinyint not null primary key,
    CustomerID int not null references customer(CustomerID),
    InventoryID tinyint not null references Inventory(InventoryID),
    EmployeeID tinyint not null references Employee(EmployeeID),
    SaleDate date not null,
    SaleQuantity int not null,
    SaleUnitPrice smallmoney not null
);

-- 4. Check what tables are inside
select * from information_schema.tables

-- 5. View specific rows
-- Top: show only the first two
select top 2 * from customer
-- Top 40 percent: also means show the first two
select top 40 percent * from customer

-- 6. View specific columns
-- Sort result (by default is ascending)
select customerfirstname, customerlastname from customer order by customerlastname desc
select customerfirstname, customerlastname from customer order by 4, 2, 3 desc  -- Order By Based on column no. without typing column name
-- Distinct: only show unique values
select distinct customerlastname from customer order by customerlastname

-- 7. Save table to another table
-- Into file_name: save result in another table (BASE TABLE)
select distinct customerlastname into temp from customer order by customerlastname
select * from temp -- see the table (data type will remain)

-- 8. Like (search something)
-- (Underscore sign) _ is only specific for one character only
-- (Percent sign) % represents zero, one, or multiple characters
select * from customer where customerlastname like '_r%'

-- 9. In (search something)
-- Search multiple items
select * from customer where customerlastname in ('Brown', 'Michael', 'Jim')

-- 10. > (search something)
select * from customer where customerlastname > 'Brown' or customerlastname > 'Cross'

-- 11. <> (Not Equal)
select * from customer where customerlastname <> 'Brown'

-- 12. IS NULL: check null values
select * from customer where customerlastname IS NULL

-- 13. IS NOT NULL
select * from customer where customerlastname IS NOT NULL

-- 14. Between
select * from sale where saleunitprice between 5 and 10  -- not include 5 & 10

-- 15. Count
-- Returns the number of rows in a table
-- AS means aliasing, temporarily giving name to a column/table
select count(*) as [Number of Records] from customer where customerfirstname like 'B%'

-- 16. Sum
select
    sale.employeeid,
    EmployeeFirstName,
    EmployeeLastName,
    count(*) as [Number of Orders],
    sum(salequantity) as [Total Quantity]
from sale, employee
where sale.employeeid = employee.employeeid
group by sale.employeeid, EmployeeFirstName, EmployeeLastName

-- 17. Count Month
select
    month(saledate) as [Month],
    count(*) as [Number of Sales],
    sum(salequantity * saleunitprice) as [Total Amount]
from sale
group by month(saledate)

-- 18. Max
SELECT MAX(Salary) FROM EmployeeSalary

-- 19. Min
SELECT MIN(Salary) FROM EmployeeSalary

-- 20. Average
SELECT AVG(Salary) FROM EmployeeSalary

-- 21. Having
SELECT JobTitle, COUNT(JobTitle)
FROM EmployeeDemographics ED
JOIN EmployeeSalary ES ON ED.EmployeeID = ES.EmployeeID
GROUP BY JobTitle
HAVING COUNT(JobTitle) > 1

SELECT JobTitle, AVG(Salary)
FROM EmployeeDemographics ED
JOIN EmployeeSalary ES ON ED.EmployeeID = ES.EmployeeID
GROUP BY JobTitle
HAVING AVG(Salary) > 45000
ORDER BY AVG(Salary)

-- 22. Change data type temporarily for use
-- CAST(expression AS datatype(length))
SELECT CAST('2017-08-25 00:00:00.000' AS date)

-- CONVERT(data_type(length), expression, style)
SELECT CONVERT(date, '2017-08-25 00:00:00.000')

-- 23. CASE Statement
SELECT
    FirstName,
    LastName,
    Age,
    CASE
        WHEN Age > 30 THEN 'Old'
        WHEN Age BETWEEN 27 AND 30 THEN 'Young'
        ELSE 'Baby'
    END
FROM EmployeeDemographics ED
WHERE Age IS NOT NULL
ORDER BY Age

SELECT
    FirstName,
    LastName,
    JobTitle,
    Salary,
    CASE
        WHEN JobTitle = 'Salesman' THEN Salary + (Salary * 0.10)
        WHEN JobTitle = 'Accountant' THEN Salary + (Salary * 0.05)
        WHEN JobTitle = 'HR' THEN Salary + (Salary * 0.000001)
        ELSE Salary + (Salary * 0.03)
    END AS SalaryAfterRaise
FROM EmployeeDemographics ED
JOIN EmployeeSalary ES ON ED.EmployeeID = ES.EmployeeID

-- 24. Partition By
-- Returns a single value for each row
SELECT
    FirstName,
    LastName,
    Gender,
    Salary,
    COUNT(Gender) OVER (PARTITION BY Gender) AS TotalGender
FROM EmployeeDemographics ED
JOIN EmployeeSalary ES ON ED.EmployeeID = ES.EmployeeID

-- 25. String Functions
-- Remove space
SELECT EmployeeID, TRIM(EmployeeID) AS IDTRIM FROM EmployeeErrors
SELECT EmployeeID, RTRIM(EmployeeID) as IDRTRIM FROM EmployeeErrors
SELECT EmployeeID, LTRIM(EmployeeID) as IDLTRIM FROM EmployeeErrors

-- Replace
SELECT LastName, REPLACE(LastName, '- Fired', '') as LastNameFixed FROM EmployeeErrors

-- Substring
SELECT
    Substring(err.FirstName, 1, 3),
    Substring(dem.FirstName, 1, 3),
    Substring(err.LastName, 1, 3),
    Substring(dem.LastName, 1, 3)
FROM EmployeeErrors err
JOIN EmployeeDemographics dem ON Substring(err.FirstName, 1, 3) = Substring(dem.FirstName, 1, 3)
    AND Substring(err.LastName, 1, 3) = Substring(dem.LastName, 1, 3)

-- UPPER and LOWER CASE
SELECT firstname, LOWER(firstname) from EmployeeErrors
SELECT Firstname, UPPER(FirstName) from EmployeeErrors

-- 