# DATABASES IN MySql FROM SCRATCH

## Difference between Relational Databases (SQL) and Non-relational DBs (NonSql)

SQL (relationals) uses tables with fixed schemes and defined relations, priorizing consistency (ACID) and structured data, perfect to complex transactions, while NotSql gives flexibility with dynamic schemes and diverse models (documents, key-values) prioritizing horizontal scalabilty and availability for big not structured data or semistructured  volumes, although with eventual consistency.

**ACID**
|Acronym  | Definition |
|---------|------------|
|**A (Atomicity):** | A transaction it executes completely or this is not executed. |
| **C (Consistency):** | It is not allowed that data being corruped, ensuring that each rule and restriction be fulfilled. |
| **I (Isolation):** | It ensures that each transaction does not retrieves dirty data, due to these are executed one by one.|
| **D (Duration):** | Data persistence. Keeps data.|


### Examples of each type of DB.

**Relationals (SQL):**
- MySql.
- Sql Server.
- Postgress.
- MariaDB
- etc.

**Non-relational databases(Non-Sql):**
- Mongo DB.
- Cassandra.
- Redis.


## MAIN COMMANDS IN MySql

### 0. To Open MySql:
```
mysql -u root -p  
```


 ### 1. It creates a new database:
```
CREATE DATABASE Riwi;  
```

### 2. Shows all databases.  
```
SHOW DATABASES; 
```

### 3. Puts a database into use
```
USE Riwi;  
```


### 4. Deletes a DB. 
```
DROP DATABASE Riwi; 
```

### 5. It creates a new table into our DB.
```
CREATE TABLE documentTypes(
    Id_docType INT AUTO_INCREMENT PRIMARY KEY,
    DocType VARCHAR (20) NOT NULL UNIQUE,
    Active TINYINT(1) NOT NULL,
    Price DECIMAL(10,2),
    CreatedAt DATETIME NOT NULL
);
```

### 6. See all properties of a table.
```
DESCRIBE documentTypes;

-- or 

DESC documentTypes;
```

### 7. Modify an existing table.

#### 7.1 Add a new column.
```
ALTER TABLE documentTypes
ADD COLUMN UpdatedAt INT NOT NULL;
```

#### 7.2 Change data type of a column.
```
ALTER TABLE documentTypes
MODIFY COLUMN UpdatedAt DATETIME;
```

#### 7.3 Rename a column.
```
ALTER TABLE documentTypes
RENAME COLUMN Price
TO pagar;
```

#### 7.4 Rename a table.
```
RENAME TABLE documentTypes
TO DocTypesCol;

-- rename to the original name:

RENAME TABLE DocTypesCol
TO documentTypes;
```

#### 7.5 Delete a field of a table.
```
ALTER TABLE documentTypes
DROP COLUMN Pagar;
```


### 8. See all tables.
```
SHOW TABLES;
```

### 9. Enter new records in a table.
```
INSERT INTO documentTypes VALUES 
(1, 'Cédula de ciudadanía', 1, '2025-12-18', '2025-12-25');

-- Another way:

INSERT INTO documentTypes 
(DocType, Active, CreatedAt, UpdatedAt) 
VALUES 
('Pasaporte', 1, '1990-12-23', '1991-02-23'), 
('Identity Card', 1, '2001-01-12', '2002-04-11'), 
('Nit', 1, '2011-03-02', '2015-04-22');

```

### 10. Show all registers of a table.
```
SELECT * FROM documentTypes;
```


### 11. Modify an register.
```
UPDATE documentTypes
SET DocType = 'Tarjeta de Identidad'
WHERE Id_docType =3;

-- or

UPDATE documentTypes
SET Active = 0
WHERE Id_docType = 4;

```

### Creating another table
```
CREATE TABLE users(
    Id_user INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(30) NOT NULL,
    Lastname VARCHAR(30),
    DocId INT NOT NULL,
    NumbDoc VARCHAR(12),
    NumKids INT NOT NULL,
    Salary DECIMAL(10,2),
    Active TINYINT(1),
    CreatedAt DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### 12. Creating relations between two tables.
```
ALTER TABLE users
ADD CONSTRAINT fk_users_documentTypes
FOREIGN KEY (DocId)
REFERENCES documentTypes(Id_docType);
```


### 13. Disable a relation
```
SET FOREIGN_KEY_CHECKS = 0;
```


### 14. Enable a relation
```
SET FOREIGN_KEY_CHECKS = 1;
```


### 15. Show all properties of a table, including relations.
```
SHOW CREATE TABLE users;

-- and

SHOW CREATE TABLE documentTypes;
```


### Insert new registers
```
INSERT INTO users (Name, Lastname, DocId, NumbDoc, NumKids, Salary, Active)
VALUES
('Juan', 'Pérez', 1, '1023456789', 2, 2850000, 1),
('María', 'Gómez', 2, '1034567890', 1, 3400000, 1),
('Carlos', 'Rodríguez', 1, '1045678901', 3, 1950000, 0),
('Ana', 'Martínez', 1, '1056789012', 0, 4200000, 1),
('Luis', 'Fernández', 3, '1067890123', 2, 3100000, 1),
('Sofía', 'López', 1, '1078901234', 1, 3650000, 1),
('Pedro', 'Sánchez', 1, '1089012345', 4, 1600000, 0),
('Valentina', 'Torres', 2, '1090123456', 0, 5100000, 1),
('Andrés', 'Ramírez', 1, '1101234567', 2, 2750000, 1),
('Camila', 'Herrera', 1, '1112345678', 1, 3350000, 1),
('Miguel', 'Castro', 3, '1123456789', 3, 2250000, 0),
('Laura', 'Morales', 1, '1134567890', 2, 3000000, 1);
```



## 16. Agregation Functions

### 16.1 To COUNT the number of records in a column or field.
```
SELECT COUNT(*) 
FROM documentTypes;
```

### 16.2 Sumarize (SUM) values of a column or field.
```
SELECT SUM(NumKids) AS promedio
FROM users;
```


### 16.3 Maximum (MAX) value of a column or field.
```
SELECT MAX(Salary) AS promedio
FROM users;
```


### 16.4 Minimum (MIN) value of a column or field.
```
SELECT MIN(Salary) AS promedio
FROM users;
```


### 16.5 Average (AVG) of a column or field.
```
SELECT AVG(Salary) AS promedio
FROM users;
```


### 17. Show unique values (without duplicates)
```
SELECT DISTINCT NumKids
FROM users;
```

### 18. Limit to n amount or records (LIMIT).
```
SELECT * 
FROM users
LIMIT 3;
```


### 19. Order by an specific field or column in ascendent order (ORDER BY - ASC).
```
SELECT *
FROM users
ORDER BY NumKids ASC;
```


### 20. Order by an specific field or column in descendent order (ORDER BY - DESC).
```
SELECT *
FROM users
ORDER BY NumKids DESC
LIMIT 5;
```


### 21. Select only specific fields or columns.
```
SELECT Name, Lastname, Salary AS 'Payment Month'
FROM users;
```


### 22. Select only specific fields or columns ordered desc.
```
SELECT Name, Lastname, Salary AS 'Payment Month'
FROM users
ORDER BY Salary DESC
LIMIT 6;
```


### 23. Concatenate columns and alias (CONCAT() - AS)
```
SELECT CONCAT(Name, ' ', Lastname) AS 'Full Name', 
Salary AS 'Payment'
FROM users;
```


### 24. Filters with where condition (WHERE).
```
SELECT * 
FROM users
WHERE NumKids = 1;
```


### 25.  Filters with where condition, limits, specific fields and alias (WHERE, LIMIT, AS, CONCAT and OPERATORS).
```
SELECT 
Id_user,
CONCAT(Name, ' ', Lastname) AS Full_Name,
Salary
FROM users
WHERE Salary >= 2000000 AND Salary <= 4000000
ORDER BY Salary DESC
LIMIT 3;
```



### 26. Select records between a range (BETWEEN).
```
SELECT Name, Salary
FROM users
WHERE Salary BETWEEN 1950000 AND 4199999
ORDER BY Salary;
```



### 
```

```