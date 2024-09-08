<h2 align="center"> Canon Printer Sales Analysis with SQL [MOCK DATA] </h2>

<p align="center">
  <img src="https://github.com/user-attachments/assets/8c37b72f-a058-467c-b01d-69cf0cb8310d" width="300">
</p>

### Overview
This project involves a comprehensive analysis of Canon printer and dealer data using SQL. The objective is to assess the performance of various dealers across the years 2023 and 2024. The analysis aims to provide insights into purchasing trends, dealer performance, and printer model popularity based on transactional data.

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

```sql

```

```sql

```

```sql

```

```sql

```

```sql

```

```sql

```

```sql

```

```sql

```

```sql

```

```sql

```

```sql

```

```sql

```
