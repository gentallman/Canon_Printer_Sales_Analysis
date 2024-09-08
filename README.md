<h2 align="center"> Canon Printer Sales Analysis with SQL [MOCK DATA] </h2>

<p align="center">
  <img src="https://github.com/user-attachments/assets/8c37b72f-a058-467c-b01d-69cf0cb8310d" width="300">
</p>

### Purpose
- The purpose of this work is to practice and demonstrate SQL concepts, including JOINs, Common Table Expressions (CTEs), UNION, HAVING, ORDER BY, GROUP BY, and Data Definition Language (DDL) and Data Manipulation Language (DML) operations. This project will also cover the use of primary and foreign key constraints.

- Additionally, for broader practice for data reporting, another mock sales data will be analyzed in Excel to create pivot charts and dashboards.

### Overview
This work involves a comprehensive analysis of Canon printer and dealer data using SQL. The objective is to assess the performance of various dealers across the years 2023 and 2024. The analysis aims to provide insights into purchasing trends, dealer performance, and printer model popularity based on transactional data.

### Objectives:

1. **Data Preparation and Cleaning:**
2. **Dealer Performance Analysis:**
3. **Printer Model Analysis:**
4. **Top Dealers and Insights:**
5. **Geographic Analysis:**
6. **Active Dealers Analysis:**

### Schema

```sql
DROP TABLE IF EXISTS DealerDB

-- Create the Database
CREATE DATABASE DealerDB;

USE DealerDB;
```

### Data and Table Creation
```sql
-- Create the Dealer Table
CREATE TABLE Dealer (
    DealerID INT PRIMARY KEY NOT NULL,
    DealerName VARCHAR(100),
    Location VARCHAR(100),
    Age INT NULL,
    Gender CHAR(1) NULL,
    ContactNumber VARCHAR(15)
);

-- Insert Sample Data into the Dealer Table
INSERT INTO Dealer (DealerID, DealerName, Location, Age, Gender, ContactNumber)
VALUES
(1, 'John Doe Electronics', 'Toronto', 45, 'M', '416-555-0101'),
(2, 'Jane Smith Gadgets', 'Vancouver', 39, 'F', '604-555-0102'),
(3, 'Techie World', 'Montreal', 36, NULL, '514-555-0103'),
(4, 'Future Tech', 'Calgary', 42, 'F', '403-555-0104'),
(5, 'Innovative Solutions', 'Ottawa', 36, 'M', '613-555-0105'),
(6, 'Smart Choice Electronics', 'Edmonton', 40, 'M', '780-555-0106'),
(7, 'Gadget Galaxy', 'Halifax', 32, 'F', '902-555-0107'),
(8, 'Electro Hub', 'Victoria', NULL, 'M', '250-555-0108'),
(9, 'Digital Dynamics', 'London', 29, 'F', '519-555-0109'),
(10, 'NextGen Technologies', 'Quebec City', NULL, 'M', '418-555-0110');
```

```sql
-- Create the Purchase Table for 2023
CREATE TABLE Purchase_2023 (
    PurchaseID INT PRIMARY KEY,
    DealerID INT,
    PurchaseDate DATE,
    PurchaseAmount DECIMAL(10, 2),
	PrinterModel VARCHAR(100),
    FOREIGN KEY (DealerID) REFERENCES Dealer(DealerID)
);

-- Insert Sample Data into the Purchase_2023 Table
INSERT INTO Purchase_2023 (PurchaseID, DealerID, PurchaseDate, PurchaseAmount, PrinterModel)
VALUES
(1, 1, '2023-01-15', 5000, 'Printer A'),
(2, 1, '2023-05-22', 7000, 'Printer B'),
(3, 2, '2023-03-10', 8000, 'Printer C'),
(4, 2, '2023-08-14', 6000, 'Printer D'),
(5, 3, '2023-07-30', 6500, 'Printer A'),
(6, 3, '2023-11-05', 7200, 'Printer B'),
(7, 4, '2023-09-15', 9000, 'Printer C'),
(8, 4, '2023-12-10', 8500, 'Printer D'),
(9, 5, '2023-04-20', 7500, 'Printer A'),
(10, 5, '2023-10-18', 8000, 'Printer B'),
(11, 6, '2023-02-25', 6200, 'Printer C'),
(12, 7, '2023-06-30', 5800, 'Printer D'),
(13, 8, '2023-07-22', 6900, 'Printer A'),
(14, 9, '2023-09-05', 7300, 'Printer B'),
(15, 10, '2023-11-15', 8200, 'Printer C'),
(16, 2, '2023-09-10', 6400.00, 'Printer A'),
(17, 2, '2023-04-19', 7100.00, 'Printer C');
```

```sql
-- Create the Purchase Table for 2024
CREATE TABLE Purchase_2024 (
    PurchaseID INT PRIMARY KEY,
    DealerID INT,
    PurchaseDate DATE,
    PurchaseAmount DECIMAL(10, 2),
	PrinterModel VARCHAR(100),
    FOREIGN KEY (DealerID) REFERENCES Dealer(DealerID)
);

-- Insert Sample Data into the Purchase_2024 Table
INSERT INTO Purchase_2024 (PurchaseID, DealerID, PurchaseDate, PurchaseAmount, PrinterModel)
VALUES
(1, 1, '2024-01-20', 6000.00, 'Printer A'),
(2, 1, '2024-06-25', 7500.00, 'Printer B'),
(3, 2, '2024-04-05', 8500.00, 'Printer C'),
(4, 2, '2024-08-10', 6700.00, 'Printer D'),
(5, 3, '2024-03-15', 7000.00, 'Printer A'),
(6, 3, '2024-09-20', 7600.00, 'Printer B'),
(7, 4, '2024-05-10', 9500.00, 'Printer C'),
(8, 4, '2024-11-25', 8700.00, 'Printer D'),
(9, 5, '2024-07-30', 8000.00, 'Printer A'),
(10, 5, '2024-12-15', 8200.00, 'Printer B'),
(11, 6, '2024-01-15', 6300.00, 'Printer C'),
(12, 7, '2024-05-20', 5900.00, 'Printer D'),
(13, 8, '2024-08-30', 7000.00, 'Printer A'),
(14, 9, '2024-10-15', 7400.00, 'Printer B'),
(15, 10, '2024-12-05', 8300.00, 'Printer C'),
(16, 3, '2024-11-20', 4600.00, 'Printer B'),
(17, 3, '2024-06-12', 9200.00, 'Printer C');
```

### Data Cleaning
```sql
-- Find records with NULL:
SELECT *
FROM Dealer
WHERE Age IS NULL OR Gender IS NULL;
```

```sql
-- Replace NULL values in Age with the average age 
UPDATE Dealer
SET Age = (SELECT AVG(Age) FROM Dealer WHERE Age IS NOT NULL)
WHERE Age IS NULL;
```

```sql
-- Replace NULL values in Gender with the most frequent gender (e.g., 'M')
UPDATE Dealer
SET Gender = (SELECT TOP 1 Gender FROM Dealer GROUP BY Gender ORDER BY COUNT(*) DESC) WHERE Gender IS NULL;
```

## Business Problems and Solutions

### 1. Total Purchase Amount for Each Dealer
```sql
SELECT 
	d.DealerName, 
	sum(d23.PurchaseAmount) + sum(d24.PurchaseAmount) as Total_Purchase 
from 
	Dealer as d
	JOIN Purchase_2023 as d23
	ON d.DealerID = d23.DealerID
	
	JOIN Purchase_2024 as d24
	on d.DealerID = d24.DealerID
GROUP BY 
	d.DealerName
ORDER BY 
	Total_Purchase
```
| DealerName                  | Total_Purchase |
|-----------------------------|----------------|
| Gadget Galaxy               | 11,700.00      |
| Smart Choice Electronics    | 12,500.00      |
| Electro Hub                 | 13,900.00      |
| Digital Dynamics            | 14,700.00      |
| NextGen Technologies        | 16,500.00      |
| John Doe Electronics        | 51,000.00      |
| Innovative Solutions        | 63,400.00      |
| Future Tech                 | 71,400.00      |
| Techie World                | 111,600.00     |
| Jane Smith Gadgets          | 115,800.00     |

### 2. Average Purchase Amount for Each Printer Model 
```sql
SELECT
    PrinterModel,
    ROUND(AVG(PurchaseAmount), 2) AS AvgPurchase
FROM
    (
        SELECT PrinterModel, PurchaseAmount FROM Purchase_2023
        UNION ALL
        SELECT PrinterModel, PurchaseAmount FROM Purchase_2024
    ) AS TotalPurchase23_24
GROUP BY
    PrinterModel
ORDER BY
    AvgPurchase DESC;  
```
| PrinterModel | AvgPurchase |
|--------------|-------------|
| Printer C    | 8,030.00    |
| Printer B    | 7,200.00    |
| Printer D    | 6,933.33    |
| Printer A    | 6,700.00    |

### 3. Top 3 Dealers with the Highest Total Purchase Amount
```sql
SELECT TOP 3
    d.DealerName, 
    SUM(d23.PurchaseAmount) + SUM(d24.PurchaseAmount) AS Total_Purchase 
FROM 
    Dealer AS d
    LEFT JOIN Purchase_2023 AS d23 ON d.DealerID = d23.DealerID
    LEFT JOIN Purchase_2024 AS d24 ON d.DealerID = d24.DealerID
GROUP BY 
    d.DealerName
ORDER BY 
    Total_Purchase DESC;
```
| DealerName            | Total_Purchase |
|-----------------------|----------------|
| Jane Smith Gadgets    | 115,800.00     |
| Techie World          | 111,600.00     |
| Future Tech           | 71,400.00      |

### 4. Dealer(s) with the Highest Single Purchase Amount in 2024
```sql

SELECT
    d.DealerID,
    d.DealerName,
    p.PurchaseAmount
FROM
    Purchase_2024 p
    JOIN Dealer d ON p.DealerID = d.DealerID
WHERE
    p.PurchaseAmount = (SELECT MAX(PurchaseAmount) FROM Purchase_2024);
```
| DealerID | DealerName | PurchaseAmount |
|----------|------------|----------------|
| 4        | Future Tech | 9500.00        |


### 5. Number of Purchases by Each Dealer
```sql
SELECT
    d.DealerID,
    d.DealerName,
    COUNT(*) AS NumberOfPurchases
FROM 
    Dealer d
    JOIN (
        SELECT DealerID FROM Purchase_2023
        UNION ALL
        SELECT DealerID FROM Purchase_2024
    ) p ON d.DealerID = p.DealerID
GROUP BY
    d.DealerID, d.DealerName
ORDER BY 
    NumberOfPurchases DESC;
```
| DealerID | DealerName                | NumberOfPurchases |
|----------|---------------------------|-------------------|
| 2        | Jane Smith Gadgets        | 6                 |
| 3        | Techie World              | 6                 |
| 4        | Future Tech               | 4                 |
| 5        | Innovative Solutions      | 4                 |
| 1        | John Doe Electronics      | 4                 |
| 6        | Smart Choice Electronics  | 2                 |
| 7        | Gadget Galaxy             | 2                 |
| 8        | Electro Hub               | 2                 |
| 9        | Digital Dynamics          | 2                 |
| 10       | NextGen Technologies      | 2                 |

### 6. Location with the Highest Single Purchase Amount in 2023
```sql
SELECT
    d.Location,
    p.PurchaseAmount
FROM 
    Dealer d
    JOIN Purchase_2023 p ON d.DealerID = p.DealerID
WHERE 
    p.PurchaseAmount = (SELECT MAX(PurchaseAmount) FROM Purchase_2023);
```
| Location | PurchaseAmount |
|----------|----------------|
| Calgary  | 9,000.00       |

### 7. Location with the Highest Total Purchase Amount in 2023
```sql
-- Calculate the total purchase amount per location in 2023
WITH TotalPurchasePerLocation AS (
    SELECT
        d.Location,
        SUM(p.PurchaseAmount) AS TotalPurchaseAmount
    FROM 
        Dealer d
        JOIN Purchase_2023 p ON d.DealerID = p.DealerID
    GROUP BY
        d.Location
)
-- Select the location with the highest total purchase amount
SELECT 
    Location,
    TotalPurchaseAmount
FROM
    TotalPurchasePerLocation
WHERE 
    TotalPurchaseAmount = (SELECT MAX(TotalPurchaseAmount) FROM TotalPurchasePerLocation);
```
| Location  | TotalPurchaseAmount |
|-----------|---------------------|
| Vancouver | 27,500.00           |

### 8. Printer Models with Total Purchase Amount Exceeding $60,000
```sql
SELECT 
    PrinterModel,
    SUM(PurchaseAmount) AS TotalPurchaseAmount
FROM
    (
        SELECT PrinterModel, PurchaseAmount FROM Purchase_2023
        UNION ALL
        SELECT PrinterModel, PurchaseAmount FROM Purchase_2024
    ) AS CombinedPurchase
GROUP BY
    PrinterModel
HAVING
    SUM(PurchaseAmount) > 60000;
```
| PrinterModel | TotalPurchaseAmount |
|--------------|----------------------|
| Printer A    | 64,600.00            |
| Printer C    | 77,000.00            |

### 9.  Dealers with More Than 3 Purchases in 2024
```sql
SELECT 
    d.DealerName,
    COUNT(*) AS NumberOfPurchases
FROM 
    Dealer d
    JOIN Purchase_2024 p ON d.DealerID = p.DealerID
GROUP BY 
    d.DealerName
HAVING
    COUNT(*) > 3;
```
| DealerName   | NumberOfPurchases |
|--------------|-------------------|
| Techie World | 4                 |

