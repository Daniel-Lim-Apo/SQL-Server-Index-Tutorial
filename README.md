# SQL Server Indexes Example

Indexes in SQL Server can greatly enhance the performance of queries, especially on large datasets. Indexes are similar to the index in a book – they help locate data faster. In SQL Server, there are different types of indexes, such as clustered, non-clustered, unique, and full-text indexes.

Indexes are a crucial part of SQL Server optimization. However, it's important to note that while indexes can speed up read operations, they can slow down insert, update, and delete operations, as these actions require updating the indexes as well.


## 1. Clustered Index

A **Clustered Index** defines the order in which data is physically stored in a table. A table can have only one clustered index because the data rows themselves can be sorted in only one order.

### Example:

```sql
-- Create a table with a primary key constraint which by default creates a clustered index
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY, -- Clustered index created by default
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50),
    HireDate DATE
);
```

In this example, a clustered index is automatically created on the `EmployeeID` column since it's the primary key. This ensures the rows are stored in the order of `EmployeeID`.

## 2. Non-Clustered Index

A **Non-Clustered Index** is an index that doesn’t change the physical order of the rows in the table. It provides a pointer to the data. A table can have multiple non-clustered indexes.

### Example:

```sql
-- Create a non-clustered index on the LastName column
CREATE NONCLUSTERED INDEX IX_LastName
ON Employees (LastName);
```

This non-clustered index will make it faster to search for employees by their last name without altering the physical order of data in the table.

## 3. Unique Index

A **Unique Index** ensures that the values in the index key columns are unique. This is often used to enforce the uniqueness of data in columns that aren’t primary keys.

### Example:

```sql
-- Create a unique index on the Email column
CREATE UNIQUE INDEX IX_Email
ON Employees (Email);
```

This unique index ensures that no two employees can have the same email address.

## 4. Composite Index

A **Composite Index** is an index on two or more columns. These are useful when queries often filter or sort data based on multiple columns.

### Example:

```sql
-- Create a composite non-clustered index on LastName and FirstName
CREATE NONCLUSTERED INDEX IX_LastName_FirstName
ON Employees (LastName, FirstName);
```

This composite index will optimize queries that search for employees by both their last and first names.

## 5. Full-Text Index

A **Full-Text Index** is used for performing complex queries on textual data. It supports word-based searches in character string columns such as `VARCHAR` or `TEXT`.

### Example:

```sql
-- Step 1: Enable Full-Text Indexing on the database
EXEC sp_fulltext_database 'enable';

-- Step 2: Create a full-text catalog (if not already created)
CREATE FULLTEXT CATALOG EmployeeCatalog AS DEFAULT;

-- Step 3: Create a full-text index on the Employees table for the LastName column
CREATE FULLTEXT INDEX ON Employees(LastName)
    KEY INDEX PK_Employees
    ON EmployeeCatalog;
```

In this example, a full-text index is created on the `LastName` column, allowing for more advanced search queries like finding all employees with a last name that contains a specific word or phrase.

## 6. Covering Index

A **Covering Index** is a special type of non-clustered index that includes all the columns needed by a query, reducing the need to access the actual table data (also known as a "lookup").

### Example:

```sql
-- Create a covering index that covers a specific query
CREATE NONCLUSTERED INDEX IX_CoveringIndex
ON Employees (LastName)
INCLUDE (FirstName, HireDate);
```

This index will cover queries that filter by `LastName` and need `FirstName` and `HireDate` in the result set, improving performance by avoiding extra lookups.

## 7. Dropping an Index

To remove an index, you can use the `DROP INDEX` statement.

### Example:

```sql
-- Drop the non-clustered index on LastName
DROP INDEX IX_LastName ON Employees;
```

This statement removes the `IX_LastName` non-clustered index from the `Employees` table.

---
