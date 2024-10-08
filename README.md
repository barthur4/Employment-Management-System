CREATE DATABASE PharmacyDB;

USE PharmacyDB;

DROP TABLE DEPARTMENTS;

CREATE TABLE DEPARTMENTS (
    DEPARTMENTID INT IDENTITY(1,1) NOT NULL,
    DEPARTMENTNAME VARCHAR(100) NOT NULL,
	LOCATION VARCHAR(30) NOT NULL
	PRIMARY KEY(DEPARTMENTID)
);

DROP TABLE EMPLOYEES;

CREATE TABLE EMPLOYEES (
    EMPLOYEEID INT IDENTITY(2,2) NOT NULL,
    FIRSTNAME VARCHAR(20) NOT NULL,
    LASTNAME VARCHAR(20) NOT NULL,
    BIRTHDATE DATE NOT NULL,
    HIREDATE DATE NOT NULL,
	PHONE VARCHAR(30) NOT NULL,
	EMAIL VARCHAR(30) NOT NULL,
	POSITION VARCHAR(30) NOT NULL,
	DEPARTMENTID INT NOT NULL
	PRIMARY KEY(EMPLOYEEID)
);

DROP TABLE PAYROLL;

CREATE TABLE PAYROLL (
     PAYROLLID INT IDENTITY(3,3) NOT NULL,
     PAYDATE DATE NOT NULL,
	 GROSSSALARY DECIMAL(20,2),
	 DEDUCTIONS DECIMAL(20,2),
	 NETSALARY DECIMAL(20,2),
	 EMPLOYEEID INT NOT NULL
	 PRIMARY KEY(PAYROLLID)
);

INSERT INTO DEPARTMENTS VALUES 
('Pharmacy','COLUMBUS'),
('Customer Service','PATASKALA');

INSERT INTO EMPLOYEES VALUES 
('ERIC','OBENG','1980-06-15','2015-01-15','6142096253','ERIC@OBENG.GMAIL.COM','PHARMACIST','1'),
('TOMMY','WANG','1985-07-21','2015-03-22','6147797437','TOMMYWANG@GMAIL.COM','PHARMACY TECHNICIAN','2'),
('ELAINE','BOATENG','1990-08-04','2021-06-10','6149753711','ELAINEBOATENG20@GMAIL.COM','PHARMACIST','3'),
('KOFI','BROWN','1990-09-22','2023-11-05','6148287527','KOFIBROWN@GMAIL.COM','DRIVER','4'),
('BRIAN','ARTHUR','2003-08-19','2022-07-20','6149665822','BRIANARTHUR609@GMAIL.COM','PHARMACY TECHNICIAN TRAINEE','5');

INSERT INTO PAYROLL VALUES
('2024-1-15',250000.00,80000.00,170000.00,'2'),
('2024-03-22',150000.00,43000.00,107000.00,'4'),
('2024-06-10',120000.00,32000.00,88000.00,'6'),
('2024-11-05',50000.00,9000,41000.00,'8'),
('2024-07-20',5000.00,400,4600,'10');

SELECT *FROM DEPARTMENTS;

SELECT *FROM EMPLOYEES;

SELECT *FROM PAYROLL;

SELECT e.FIRSTNAME, e.LASTNAME, e.POSITION, d.DEPARTMENTNAME, d.LOCATION FROM EMPLOYEES e
JOIN DEPARTMENTS d ON d.DEPARTMENTID = e.DEPARTMENTID

SELECT p.GROSSSALARY, p.NETSALARY, p.DEDUCTIONS FROM PAYROLL p
JOIN EMPLOYEES e ON p.EMPLOYEEID = e.EMPLOYEEID

SELECT SUM(NETSALARY) AS TOTALPAYROLLEXPENSE
FROM PAYROLL;

SELECT SUM(DEDUCTIONS) AS TOTALPAYROLLEXPENSE
FROM PAYROLL;

SELECT SUM(GROSSSALARY) AS TOTALPAYROLLEXPENSE
FROM PAYROLL;

DROP PROCEDURE ADDEMPLOYEE;

CREATE PROCEDURE ADDEMPLOYEE
     @DEPARTMENTID INT,
     @FIRSTNAME VARCHAR(30),
     @LASTNAME VARCHAR(30),
     @EMAIL VARCHAR(30),
     @BIRTHDATE DATE,
     @HIREDATE DATE,
     @PHONE VARCHAR(30),
     @POSITION VARCHAR(30)
AS
BEGIN
    INSERT INTO EMPLOYEES VALUES 
	  (@DEPARTMENTID, @FIRSTNAME, @LASTNAME, @EMAIL, @BIRTHDATE, @HIREDATE, @PHONE, @POSITION);
    PRINT 'Employee added successfully.';
END;

DROP PROCEDURE ADDPAYROLL;

CREATE PROCEDURE ADDPAYROLL
    @PAYDATE DATE,
    @GROSSSALARY DECIMAL(20,2),
    @DEDUCTIONS DECIMAL(20,2),
    @EMPLOYEEID INT
AS
BEGIN
    DECLARE @NETSALARY DECIMAL(20,2);
    SET @NETSALARY = @GROSSSALARY - @DEDUCTIONS;
 
    INSERT INTO PAYROLL VALUES
      (@PAYDATE, @GROSSSALARY, @DEDUCTIONS, @NETSALARY, @EMPLOYEEID);
    
    PRINT 'Payroll added successfully.';
END;

SELECT e.FIRSTNAME, 
       e.LASTNAME, 
       e.POSITION, 
       e.EMAIL 
FROM EMPLOYEES e;

SELECT e.FIRSTNAME,
       e.LASTNAME,
       p.PAYDATE,
       p.GROSSSALARY,
       p.DEDUCTIONS,
       p.NETSALARY
FROM PAYROLL p
JOIN EMPLOYEES e ON p.EMPLOYEEID = e.EMPLOYEEID

BACKUP DATABASE PharmacyDB TO 
DISK = '\\LAPTOP-FUD4S9PU\Projects\Phramacy_backup.bak'
with compression,stats=10

RESTORE DATABASE PharmacyDB
FROM DISK = '\\LAPTOP-FUD4S9PU\Projects\Phramacy_backup.bak'
