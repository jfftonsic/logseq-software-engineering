sources:: https://www.geeksforgeeks.org/cte-in-sql

- A recursive CTE is one that references itself within that CTE.
- Useful when working with **hierarchical data** (e.g. a list of employees) as the CTE continues to execute until the query returns the entire hierarchy.
- If a CTE is created incorrectly it can enter an **infinite loop**.
	- To prevent this, the MAXRECURSION hint can be added in the OPTION clause of the primary SELECT, INSERT, UPDATE, DELETE, or MERGE statement.
- SQL example:
	- ```sql
	  CREATE TABLE Employees
	  (
	    EmployeeID int NOT NULL PRIMARY KEY,
	    FirstName varchar(50) NOT NULL,
	    LastName varchar(50) NOT NULL,
	    ManagerID int NULL
	  )
	  
	  INSERT INTO Employees VALUES (1, 'Ken', 'Thompson', NULL)
	  INSERT INTO Employees VALUES (2, 'Terri', 'Ryan', 1)
	  INSERT INTO Employees VALUES (3, 'Robert', 'Durello', 1)
	  INSERT INTO Employees VALUES (4, 'Rob', 'Bailey', 2)
	  INSERT INTO Employees VALUES (5, 'Kent', 'Erickson', 2)
	  INSERT INTO Employees VALUES (6, 'Bill', 'Goldberg', 3)
	  INSERT INTO Employees VALUES (7, 'Ryan', 'Miller', 3)
	  INSERT INTO Employees VALUES (8, 'Dane', 'Mark', 5)
	  INSERT INTO Employees VALUES (9, 'Charles', 'Matthew', 6)
	  INSERT INTO Employees VALUES (10, 'Michael', 'Jhonson', 6) 
	  
	  WITH
	    cteReports (EmpID, FirstName, LastName, MgrID, EmpLevel)
	    AS
	    (
	      SELECT EmployeeID, FirstName, LastName, ManagerID, 1
	      FROM Employees
	      WHERE ManagerID IS NULL
	      UNION ALL
	      SELECT e.EmployeeID, e.FirstName, e.LastName, e.ManagerID, 
	        r.EmpLevel + 1
	      FROM Employees e
	        INNER JOIN cteReports r
	          ON e.ManagerID = r.EmpID
	    )
	  SELECT
	    FirstName + ' ' + LastName AS FullName, 
	    EmpLevel,
	    (SELECT FirstName + ' ' + LastName FROM Employees 
	      WHERE EmployeeID = cteReports.MgrID) AS Manager
	  FROM cteReports 
	  ORDER BY EmpLevel, MgrID 
	  
	  ```