# SQL Interview Questions

# 1. Explain ACID Properties

## Answer

**ACID** is a set of properties that ensures database transactions are processed **reliably and consistently**.

### ACID Stands For

| Property | Description |
|----------|-------------|
| **A - Atomicity** | A transaction is completed entirely or not executed at all. |
| **C - Consistency** | Ensures the database remains in a valid state before and after a transaction. |
| **I - Isolation** | Multiple transactions execute independently without interfering with each other. |
| **D - Durability** | Once a transaction is committed, the changes are permanently stored, even after a system failure. |

### Example

```sql
BEGIN TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;

COMMIT;
```

If any statement fails, the transaction is rolled back.

### Key Points

- Ensures data reliability.
- Prevents partial updates.
- Maintains database consistency.


-------------------
------------------


# Q. What are Database Indexes?

## Answer

A **database index** is a data structure that improves the speed of data retrieval operations on a database table. It works like the **index of a book**, allowing the database to quickly locate rows without scanning the entire table.

Without an index, the database performs a **full table scan**, which can be slow for large tables.

---

## Example

Suppose you have an `Employee` table:

| id | name | department |
|----|------|------------|
| 1 | Alice | HR |
| 2 | Bob | IT |
| 3 | Charlie | Finance |

To speed up searches by employee name, create an index:

```sql
CREATE INDEX idx_employee_name
ON Employee(name);
```

Now, searching for an employee by name is much faster.

---

## How an Index Works

Without an index:

```sql
SELECT *
FROM Employee
WHERE name = 'Bob';
```

- The database checks every row (**Full Table Scan**).

With an index:

- The database uses the index to directly locate rows where `name = 'Bob'`.
- This significantly reduces search time.

---

## Advantages of Indexes

- Improves `SELECT` query performance.
- Speeds up searching, filtering, and sorting.
- Enhances the performance of `JOIN` operations.
- Reduces query execution time for large tables.

---

## Disadvantages of Indexes

- Requires additional storage space.
- Slows down `INSERT`, `UPDATE`, and `DELETE` operations because the index must also be updated.
- Too many indexes can reduce overall database performance.

---

## Types of Indexes

- **Primary Index** – Created automatically for a primary key.
- **Unique Index** – Ensures all values in the indexed column are unique.
- **Clustered Index** – Sorts and stores the actual table data.
- **Non-Clustered Index** – Stores pointers to the table data.
- **Composite Index** – Created on two or more columns.

---

## Example of a Composite Index

```sql
CREATE INDEX idx_employee_name_department
ON Employee(name, department);
```

This index improves queries that filter by both `name` and `department`.

---

## When Should You Create an Index?

Create indexes on:

- Columns frequently used in `WHERE` clauses.
- Columns used in `JOIN` conditions.
- Columns used in `ORDER BY` and `GROUP BY`.
- Primary keys and foreign keys.

Avoid indexing:

- Small tables.
- Columns that change frequently.
- Columns with very few unique values (low selectivity).

---

## Key Points

- A **database index** improves data retrieval speed.
- It helps avoid expensive full table scans.
- Indexes improve `SELECT` performance but may slow down `INSERT`, `UPDATE`, and `DELETE`.
- Choose indexes carefully to balance read and write performance.


------------------
------------------

# 3. What are Joins in SQL?

## Answer

**Joins** are used to retrieve and combine data from **multiple tables** based on a related column between them.

Joins help fetch meaningful information when data is stored across different tables in a relational database.

---

## Example Tables

### Employee Table

| emp_id | name | dept_id |
|--------|------|---------|
| 1 | Alice | 101 |
| 2 | Bob | 102 |
| 3 | Charlie | 103 |

### Department Table

| dept_id | department |
|---------|------------|
| 101 | HR |
| 102 | IT |
| 104 | Finance |

---

# Types of Joins

## 1. INNER JOIN

Returns only the rows that have matching values in both tables.

### Syntax

```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

### Example

```sql
SELECT e.name, d.department
FROM Employee e
INNER JOIN Department d
ON e.dept_id = d.dept_id;
```

### Result

| name | department |
|------|------------|
| Alice | HR |
| Bob | IT |

Only matching records are returned.

---

## 2. LEFT JOIN (LEFT OUTER JOIN)

Returns **all records from the left table** and matching records from the right table.

If there is no match, NULL values are returned for the right table columns.

### Example

```sql
SELECT e.name, d.department
FROM Employee e
LEFT JOIN Department d
ON e.dept_id = d.dept_id;
```

### Result

| name | department |
|------|------------|
| Alice | HR |
| Bob | IT |
| Charlie | NULL |

---

## 3. RIGHT JOIN (RIGHT OUTER JOIN)

Returns **all records from the right table** and matching records from the left table.

### Example

```sql
SELECT e.name, d.department
FROM Employee e
RIGHT JOIN Department d
ON e.dept_id = d.dept_id;
```

### Result

| name | department |
|------|------------|
| Alice | HR |
| Bob | IT |
| NULL | Finance |

---

## 4. FULL JOIN (FULL OUTER JOIN)

Returns all records from both tables.

- Matching records are combined.
- Non-matching records contain NULL values.

### Example

```sql
SELECT e.name, d.department
FROM Employee e
FULL OUTER JOIN Department d
ON e.dept_id = d.dept_id;
```

### Result

| name | department |
|------|------------|
| Alice | HR |
| Bob | IT |
| Charlie | NULL |
| NULL | Finance |

---

## 5. CROSS JOIN

Returns the **Cartesian product** of two tables.

Every row from the first table is combined with every row from the second table.

### Example

```sql
SELECT e.name, d.department
FROM Employee e
CROSS JOIN Department d;
```

If:

- Employee table has 3 rows
- Department table has 4 rows

Result:

```text
3 × 4 = 12 rows
```

---

## Join Comparison Table

| Join Type | Result |
|-----------|--------|
| INNER JOIN | Only matching records from both tables |
| LEFT JOIN | All records from left table + matching right records |
| RIGHT JOIN | All records from right table + matching left records |
| FULL JOIN | All records from both tables |
| CROSS JOIN | Every possible combination of rows |

---

## Key Points

- Joins combine data from multiple tables.
- `INNER JOIN` is the most commonly used join.
- `LEFT JOIN` keeps all records from the left table.
- `RIGHT JOIN` keeps all records from the right table.
- `FULL JOIN` returns all records from both tables.
- `CROSS JOIN` creates every possible combination of rows.


-------------
---------------


# 4. Difference Between `WHERE` and `HAVING` in SQL

## Answer

Both `WHERE` and `HAVING` are used to filter data in SQL, but they work at different stages of query execution.

- **`WHERE`** filters individual rows before grouping.
- **`HAVING`** filters groups after `GROUP BY` is applied.

---

## Comparison Table

| Feature | WHERE | HAVING |
|---------|-------|--------|
| Purpose | Filters rows | Filters groups |
| Applied | Before `GROUP BY` | After `GROUP BY` |
| Works On | Individual records | Aggregated results |
| Aggregate Functions | ❌ Cannot use directly | ✅ Can use |
| Used With | `SELECT`, `UPDATE`, `DELETE` | Mainly used with `GROUP BY` |

---

# 1. WHERE Clause

The `WHERE` clause filters rows before any grouping or aggregation happens.

### Example

```sql
SELECT *
FROM employee
WHERE department = 'IT';
```

### Explanation

Only employees from the `IT` department are selected before any grouping occurs.

---

## Using WHERE with Aggregate Function

```sql
SELECT *
FROM employee
WHERE COUNT(*) > 5;
```

### Result

```text
Error
```

### Reason

`WHERE` cannot be used with aggregate functions like:

- `COUNT()`
- `SUM()`
- `AVG()`
- `MAX()`
- `MIN()`

---

# 2. HAVING Clause

The `HAVING` clause filters grouped data after the `GROUP BY` operation.

### Example

```sql
SELECT department, COUNT(*)
FROM employee
GROUP BY department
HAVING COUNT(*) > 5;
```

### Explanation

1. Rows are grouped by `department`.
2. `COUNT(*)` calculates the number of employees in each department.
3. Only departments with more than 5 employees are returned.

---

## Query Execution Order

The general execution order is:

```text
FROM
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
ORDER BY
```

---

## Example Difference

### Using WHERE

```sql
SELECT department, COUNT(*)
FROM employee
WHERE salary > 50000
GROUP BY department;
```

- First filters employees with salary greater than 50000.
- Then groups the remaining employees.

---

### Using HAVING

```sql
SELECT department, COUNT(*)
FROM employee
GROUP BY department
HAVING COUNT(*) > 5;
```

- First groups employees by department.
- Then filters departments having more than 5 employees.

---

## Key Points

- Use **`WHERE`** to filter rows before aggregation.
- Use **`HAVING`** to filter groups after aggregation.
- `WHERE` cannot use aggregate functions directly.
- `HAVING` is commonly used with `GROUP BY`.

------------
-----------


# 5. Difference Between `DELETE`, `TRUNCATE`, and `DROP` in SQL

## Answer

`DELETE`, `TRUNCATE`, and `DROP` are SQL commands used to remove data, but they differ in terms of **what they remove**, **rollback support**, and **command type**.

---

## Comparison Table

| Feature | DELETE | TRUNCATE | DROP |
|---------|--------|----------|------|
| Purpose | Removes selected rows | Removes all rows | Removes entire table |
| Removes Data | ✅ Yes | ✅ Yes | ✅ Yes |
| Removes Table Structure | ❌ No | ❌ No | ✅ Yes |
| WHERE Clause | ✅ Can use | ❌ Cannot use | ❌ Not applicable |
| Rollback Support | ✅ Can rollback within a transaction (depends on database) | Usually cannot rollback in many databases | Usually cannot rollback easily |
| Speed | Slower for large data | Faster | Fast |
| Command Type | DML | DDL | DDL |

---

# 1. DELETE Command

The `DELETE` command removes specific rows from a table.

### Syntax

```sql
DELETE FROM employee
WHERE id = 101;
```

### Explanation

- Removes only rows matching the condition.
- The table structure remains unchanged.
- Can delete selected records using `WHERE`.

### Delete All Rows

```sql
DELETE FROM employee;
```

---

# 2. TRUNCATE Command

The `TRUNCATE` command removes all rows from a table but keeps the table structure.

### Syntax

```sql
TRUNCATE TABLE employee;
```

### Explanation

- Removes all records.
- Cannot use a `WHERE` condition.
- Faster than `DELETE` because it removes data in bulk.
- Resets identity/auto-increment values in many databases.

---

# 3. DROP Command

The `DROP` command removes the entire table, including its structure and data.

### Syntax

```sql
DROP TABLE employee;
```

### Explanation

- Deletes the table completely.
- Removes:
  - Data
  - Table structure
  - Indexes
  - Constraints

After dropping, the table no longer exists.

---

## Example

Assume an `Employee` table:

### DELETE

```sql
DELETE FROM Employee
WHERE department = 'HR';
```

Result:

- HR employee records are removed.
- Table remains.

---

### TRUNCATE

```sql
TRUNCATE TABLE Employee;
```

Result:

- All employee records are removed.
- Table remains.

---

### DROP

```sql
DROP TABLE Employee;
```

Result:

- Employee table is completely removed.

---

## Key Points

- **DELETE** → Removes selected rows and supports `WHERE`.
- **TRUNCATE** → Removes all rows but keeps the table structure.
- **DROP** → Removes the complete table.
- Use `DELETE` when you need selective removal.
- Use `TRUNCATE` when you need to quickly clear a table.
- Use `DROP` when the table is no longer required.



-------------------
-------------------

# 6. Difference Between `UNION` and `UNION ALL` in SQL

## Answer

Both `UNION` and `UNION ALL` are used to combine the results of two or more `SELECT` queries, but they differ in how they handle duplicate records.

---

## Comparison Table

| Feature | UNION | UNION ALL |
|---------|-------|-----------|
| Duplicate Records | Removes duplicates | Keeps duplicates |
| Performance | Slower due to duplicate removal | Faster |
| Sorting | Performs duplicate checking and may sort results | No duplicate checking |
| Result Size | Returns only unique records | Returns all records |

---

# 1. UNION

The `UNION` operator combines results from multiple queries and removes duplicate rows.

### Syntax

```sql
SELECT column_name
FROM table1

UNION

SELECT column_name
FROM table2;
```

### Example

```sql
SELECT name FROM employee1

UNION

SELECT name FROM employee2;
```

### Result Example

**employee1**

| name |
|------|
| Alice |
| Bob |

**employee2**

| name |
|------|
| Bob |
| Charlie |

**UNION Result**

| name |
|------|
| Alice |
| Bob |
| Charlie |

The duplicate `Bob` record is removed.

---

# 2. UNION ALL

The `UNION ALL` operator combines results from multiple queries and keeps duplicate rows.

### Syntax

```sql
SELECT column_name
FROM table1

UNION ALL

SELECT column_name
FROM table2;
```

### Example

```sql
SELECT name FROM employee1

UNION ALL

SELECT name FROM employee2;
```

### Result Example

| name |
|------|
| Alice |
| Bob |
| Bob |
| Charlie |

The duplicate `Bob` record is retained.

---

## Important Rules

For both `UNION` and `UNION ALL`:

- The number of columns in both queries must be the same.
- Corresponding columns must have compatible data types.
- Column names are taken from the first `SELECT` statement.

---

## Key Points

- **UNION** → Combines results and removes duplicates.
- **UNION ALL** → Combines results and keeps duplicates.
- `UNION ALL` is faster because it does not perform duplicate elimination.
- Use `UNION` when unique results are required.
- Use `UNION ALL` when all records are needed, including duplicates.


-----------------
------------------


# 23. What is SQL Injection?

## Answer

**SQL Injection** is a security attack where an attacker inserts **malicious SQL statements** into application input fields to manipulate database queries.

It occurs when user input is directly combined with SQL queries without proper validation or parameterization.

---

## Example of Vulnerable Code

```java
String query = "SELECT * FROM users WHERE username = '" 
               + username + "'";
```

If a user enters:

```text
admin' OR '1'='1
```

The generated query becomes:

```sql
SELECT * FROM users 
WHERE username = 'admin' OR '1'='1';
```

The condition always evaluates to true, which may allow unauthorized access.

---

## Risks of SQL Injection

- Unauthorized access to sensitive data.
- Data modification or deletion.
- Bypassing authentication.
- Exposure of confidential information.
- Possible database compromise.

---

## How to Prevent SQL Injection?

### 1. Use Prepared Statements

Prepared statements separate SQL code from user input.

### Example (Java)

```java
String query = "SELECT * FROM users WHERE username = ?";

PreparedStatement ps = connection.prepareStatement(query);

ps.setString(1, username);
```

---

### 2. Validate User Input

- Check input format.
- Restrict unexpected characters.
- Apply proper validation rules.

---

### 3. Use Stored Procedures Carefully

Stored procedures can reduce risk when implemented securely and avoid dynamic SQL construction.

---

### 4. Apply Least Privilege

- Give database users only the permissions they need.
- Avoid using administrator accounts for applications.

---

## Key Points

- SQL Injection is caused by unsafe handling of user input.
- Never build SQL queries by directly concatenating user input.
- Use **PreparedStatement** or parameterized queries.
- Input validation and proper database permissions improve security.

-------------
-------------

# Frequently Asked SQL Coding Questions for Java Developers

## 1. Find the Nth Highest Salary

### Question

Find the 2nd highest salary from the Employee table.

### Query Using `DENSE_RANK()`

```sql
SELECT salary
FROM (
    SELECT salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) AS rank
    FROM employee
) temp
WHERE rank = 2;
```

### Explanation

- `DENSE_RANK()` assigns ranking based on salary.
- Highest salary gets rank `1`.
- Second highest salary gets rank `2`.

---

## 2. Find Duplicate Records

### Example

Find duplicate employee names.

```sql
SELECT name, COUNT(*)
FROM employee
GROUP BY name
HAVING COUNT(*) > 1;
```

### Explanation

- Groups records by name.
- Returns names appearing more than once.

---

## 3. Remove Duplicate Rows

### Example

Keep one record and remove duplicates.

```sql
DELETE FROM employee
WHERE id NOT IN (
    SELECT MIN(id)
    FROM employee
    GROUP BY name, department, salary
);
```

### Explanation

- Keeps the row with the smallest ID.
- Deletes duplicate records.

> Always verify data before running DELETE queries in production.

---

## 4. Find Employees Without Departments

Assume:

- `employee` table has `department_id`.
- `department` table has `id`.

### Query

```sql
SELECT e.*
FROM employee e
LEFT JOIN department d
ON e.department_id = d.id
WHERE d.id IS NULL;
```

### Explanation

- `LEFT JOIN` keeps all employees.
- `NULL` department means no matching department exists.

---

## 5. Find Department-Wise Employee Count

### Query

```sql
SELECT department_id,
       COUNT(*) AS employee_count
FROM employee
GROUP BY department_id;
```

### Output Example

| department_id | employee_count |
|---------------|----------------|
| 101 | 5 |
| 102 | 10 |

---

## 6. Find Employees Who Joined in the Last 30 Days

### Query (MySQL)

```sql
SELECT *
FROM employee
WHERE joining_date >= CURRENT_DATE - INTERVAL 30 DAY;
```

### Query (PostgreSQL)

```sql
SELECT *
FROM employee
WHERE joining_date >= CURRENT_DATE - INTERVAL '30 days';
```

---

## 7. Find Maximum Salary Per Department

### Query

```sql
SELECT department_id,
       MAX(salary) AS max_salary
FROM employee
GROUP BY department_id;
```

---

## 8. Find the Longest Employee Name

### Query

```sql
SELECT name
FROM employee
ORDER BY LENGTH(name) DESC
LIMIT 1;
```

### SQL Server Version

```sql
SELECT TOP 1 name
FROM employee
ORDER BY LEN(name) DESC;
```

---

## 9. Find Common Records Between Two Tables

### Using INNER JOIN

```sql
SELECT a.*
FROM employee1 a
INNER JOIN employee2 b
ON a.id = b.id;
```

### Using INTERSECT

```sql
SELECT id
FROM employee1

INTERSECT

SELECT id
FROM employee2;
```

---

## 10. Write Pagination Query

Pagination is used to fetch records in batches.

### MySQL Example

```sql
SELECT *
FROM employee
ORDER BY id
LIMIT 10 OFFSET 20;
```

### Explanation

- `LIMIT 10` → Fetch 10 records.
- `OFFSET 20` → Skip first 20 records.

Example:

```
Page 1: OFFSET 0
Page 2: OFFSET 10
Page 3: OFFSET 20
```

---

## 11. Explain Indexing Strategy for Slow Queries

### Answer

Indexes improve query performance by allowing faster data lookup.

### Indexing Strategies

### 1. Analyze Slow Queries

Use query execution plans:

```sql
EXPLAIN SELECT *
FROM employee
WHERE email = 'test@example.com';
```

---

### 2. Add Indexes on Frequently Used Columns

Example:

```sql
CREATE INDEX idx_employee_email
ON employee(email);
```

---

### 3. Index Columns Used In:

- `WHERE` conditions
- `JOIN` conditions
- `ORDER BY`
- `GROUP BY`

---

### 4. Use Composite Indexes

Example:

```sql
CREATE INDEX idx_department_salary
ON employee(department_id, salary);
```

Useful for:

```sql
WHERE department_id = 10
AND salary > 50000;
```

---

### 5. Avoid Too Many Indexes

Indexes improve reads but increase:

- Insert cost
- Update cost
- Delete cost

---

## 12. Optimize a Slow SELECT Query

### Answer

Steps to optimize a slow query:

---

### 1. Check Query Execution Plan

```sql
EXPLAIN SELECT *
FROM employee
WHERE department_id = 10;
```

---

### 2. Avoid Selecting Unnecessary Columns

Avoid:

```sql
SELECT *
FROM employee;
```

Prefer:

```sql
SELECT id, name
FROM employee;
```

---

### 3. Add Proper Indexes

Example:

```sql
CREATE INDEX idx_department
ON employee(department_id);
```

---

### 4. Optimize JOINs

Avoid unnecessary joins and ensure joined columns are indexed.

Example:

```sql
SELECT e.name, d.department
FROM employee e
JOIN department d
ON e.department_id = d.id;
```

---

### 5. Limit Result Size

Instead of fetching all records:

```sql
SELECT *
FROM employee;
```

Use:

```sql
SELECT *
FROM employee
LIMIT 100;
```

---

### 6. Avoid Functions on Indexed Columns

Avoid:

```sql
WHERE YEAR(joining_date) = 2026;
```

Prefer:

```sql
WHERE joining_date >= '2026-01-01'
AND joining_date < '2027-01-01';
```

---

## Key Points

- Use window functions like `DENSE_RANK()` for nth highest salary problems.
- Use `GROUP BY` with aggregate functions for counting and maximum values.
- Use joins to find matching or missing records.
- Use indexes carefully based on query patterns.
- Use `EXPLAIN` to identify slow query execution plans.
- Avoid unnecessary data retrieval with `SELECT *`.


----------------
--------------
