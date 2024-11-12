## Data types
 ## 1. CHAR(size)
- **Description**: A fixed-length string that can contain letters, numbers, and special characters.
- **Size Parameter**: Specifies the exact length of the string in characters.
- **Usage**: Often used for data with a known, fixed length, like country codes or postal codes.

**Example:**
 ```sql
 CREATE TABLE example_table (
    country_code CHAR(2)
);
```
 ## 2. VARCHAR(size)
 - **Description:** A variable-length string that can contain letters, numbers, and special characters.
 - **Size Parameter:** Specifies the maximum length of the string in characters.
 - **Usage:** Used for data with variable length, like names or addresses.

**Example:**
```sql
CREATE TABLE example_table (
    name VARCHAR(50)
);
```
 ## 3. INT
 - **Description:** An integer data type with a range from -2147483648 to 2147483647.
 - **Usage:** Used for storing whole numbers within the specified range.

**Example:**
```sql
CREATE TABLE example_table (
    age INT
);
```
 ## 4. FLOAT(size, d)
  - **Description:** A floating-point number with precision defined by the size (total digits) and d (digits after the decimal).
  - **Usage:** Suitable for scientific calculations where precision is required.

**Example:**
```sql
CREATE TABLE example_table (
    rating FLOAT(5, 2) -- e.g., rating up to 99.99
);
```
 ## 5. DOUBLE(size, d)
- **Description:** Similar to FLOAT but allows for greater precision.
- **Usage:** Used when higher precision is required for real numbers.

**Example:**

```sql
CREATE TABLE example_table (
    measurement DOUBLE(10, 4) -- e.g., measurement up to 999999.9999
);
```
 ## 6. NUMBER(size, d)
 - **Description:** Used for both integers and decimals. size defines the total number of digits, and d defines the digits after the decimal point.
- **Usage:** General purpose numeric field, used for flexible numeric requirements.

**Example:**
```sql
CREATE TABLE example_table (
    price NUMBER(10, 2) -- e.g., price up to 99999999.99
);
```
 ## 7. DECIMAL(size, d)
- **Description:** Identical to NUMBER, it allows for integers and decimals with specified precision.
- **Usage:** Used for financial or precise decimal calculations.

**Example:**
```sql
CREATE TABLE example_table (
    salary DECIMAL(8, 2) -- e.g., salary up to 999999.99
);
```
 ## 8. DATE
 - **Description**: Stores dates in the format `YYYY-MM-DD`.
 - **Usage**: Used for storing date values without time.

 **Example**:
```sql
CREATE TABLE example_table (
    birth_date DATE
);
```
 ## 9. TIME
- **Description:** Stores time in the format HH:MI:SS (12-hour) or HH24:MI:SS (24-hour).
- **Usage:** Used to store time values without a date.

**Example:**
```sql
CREATE TABLE example_table (
    login_time TIME
);
```
 ## 10. DATETIME
- **Description:** Stores both date and time in the format YYYY-MM-DD HH:MI:SS.
- **Range:** Supports dates from 1000-01-01 to 9999-12-31.
- **Usage:** Commonly used when both date and time are needed, such as in logging events.

**Example:**
```sql
CREATE TABLE example_table (
    created_at DATETIME
);
```
 ## 11. TIMESTAMP
- **Description:** Stores date and time with timezone information, typically representing a - - - specific moment in time.
- **Range:** Supports dates from 1970-01-01 UTC to 2038-01-09 UTC.
- **Usage:** Useful for logging events with time zone specificity.

**Example:**
```sql
CREATE TABLE example_table (
    updated_at TIMESTAMP
);
```
 ## 12. BOOLEAN
- **Description:** Represents true or false values.
- **Usage:** Commonly used for flags or status fields, where 0 represents FALSE and any non-zero value represents TRUE.

**Example:**
```sql
CREATE TABLE example_table (
    is_active BOOLEAN
);
```
 ## 13.Others(BLOB,CLOB,BINARY,VARBINARY)
- **Description:** Used to store multimedia or binary data.
- **BLOB (Binary Large Object):** Stores binary data such as images or files.
- **CLOB (Character Large Object):** Stores large text data.
- **BINARY and VARBINARY:** Store binary data with fixed or variable lengths.

**Example:**
```sql
CREATE TABLE example_table (
    profile_picture BLOB,
    document CLOB
);
```
## Operators in SQL

  ## SQL Arithmetic Operators
SQL provides arithmetic operators for performing mathematical calculations.

| Operator | Description         | Example                       |
|----------|---------------------|-------------------------------|
| `+`      | Addition            | `SELECT 10 + 5;`             |
| `-`      | Subtraction         | `SELECT 10 - 5;`             |
| `*`      | Multiplication      | `SELECT 10 * 5;`             |
| `/`      | Division            | `SELECT 10 / 5;`             |
| `%`      | Modulo (remainder)  | `SELECT 10 % 3;`             |

---

 ## SQL Comparison Operators
Comparison operators are used to compare values in SQL queries.

| Operator | Description               | Example                          |
|----------|---------------------------|----------------------------------|
| `=`      | Equal to                  | `SELECT * FROM table WHERE age = 30;` |
| `>`      | Greater than              | `SELECT * FROM table WHERE age > 30;` |
| `<`      | Less than                 | `SELECT * FROM table WHERE age < 30;` |
| `>=`     | Greater than or equal to  | `SELECT * FROM table WHERE age >= 30;` |
| `<=`     | Less than or equal to     | `SELECT * FROM table WHERE age <= 30;` |
| `<>` or `!=` | Not equal to         | `SELECT * FROM table WHERE age <> 30;` |

These operators are commonly used in `WHERE` clauses to filter data based on conditions.

 ## SQL Logical Operators

SQL logical operators are used to combine, negate, or test conditions in SQL queries.

| Operator | Description                                                                            | Example |
|----------|----------------------------------------------------------------------------------------|---------|
| `AND`    | Returns `TRUE` if **all** conditions separated by `AND` are `TRUE`.                   | ``` SELECT * FROM employees WHERE age > 25 AND city = 'New York';``` |
| `OR`     | Returns `TRUE` if **any** of the conditions separated by `OR` is `TRUE`.              | ``` SELECT * FROM employees WHERE age > 25 OR city = 'New York';``` |
| `NOT`    | Returns `TRUE` if the condition is **not true**.                                      | ``` SELECT * FROM employees WHERE NOT (age > 30);``` |
| `ALL`    | Returns `TRUE` if **all** values in a subquery meet the condition.                    | ``` SELECT * FROM employees WHERE salary > ALL (SELECT salary FROM departments WHERE dept_id = 10);``` |
| `ANY`    | Returns `TRUE` if **any** of the values in a subquery meet the condition.             | ```SELECT * FROM employees WHERE salary > ANY (SELECT salary FROM departments WHERE dept_id = 10);``` |
| `IN`     | Returns `TRUE` if the operand is **equal to one** of a list of expressions.           | ``` SELECT * FROM employees WHERE city IN ('New York', 'Chicago', 'San Francisco');``` |
| `LIKE`   | Returns `TRUE` if the operand matches a specified **pattern**.                        | ```SELECT * FROM employees WHERE name LIKE 'A%';``` |
| `BETWEEN`| Returns `TRUE` if the operand is **within the specified range** of comparisons.       | ``` SELECT * FROM employees WHERE age BETWEEN 20 AND 30;``` |
| `EXISTS` | Returns `TRUE` if a **subquery returns one or more records**.                         | ``` SELECT * FROM employees WHERE EXISTS (SELECT * FROM departments WHERE dept_id = employees.dept_id);``` |

---

 ## Detailed Examples

1. **AND**: Finds employees older than 25 who live in New York.
    ```sql
    SELECT * FROM employees 
    WHERE age > 25 AND city = 'New York';
    ```

2. **OR**: Finds employees older than 25 or those who live in New York.
    ```sql
    SELECT * FROM employees 
    WHERE age > 25 OR city = 'New York';
    ```

3. **NOT**: Finds employees who are **not** older than 30.
    ```sql
    SELECT * FROM employees 
    WHERE NOT (age > 30);
    ```

4. **ALL**: Finds employees whose salary is higher than all salaries in department 10.
    ```sql
    SELECT * FROM employees 
    WHERE salary > ALL (SELECT salary FROM departments WHERE dept_id = 10);
    ```

5. **ANY**: Finds employees with a salary higher than any salary in department 10.
    ```sql
    SELECT * FROM employees 
    WHERE salary > ANY (SELECT salary FROM departments WHERE dept_id = 10);
    ```

6. **IN**: Finds employees in specific cities.
    ```sql
    SELECT * FROM employees 
    WHERE city IN ('New York', 'Chicago', 'San Francisco');
    ```

7. **LIKE**: Finds employees whose names start with "A".
    ```sql
    SELECT * FROM employees 
    WHERE name LIKE 'A%';
    ```

8. **BETWEEN**: Finds employees aged between 20 and 30.
    ```sql
    SELECT * FROM employees 
    WHERE age BETWEEN 20 AND 30;
    ```

9. **EXISTS**: Finds employees who belong to departments that exist in the departments table.
    ```sql
    SELECT * FROM employees 
    WHERE EXISTS (SELECT * FROM departments WHERE dept_id = employees.dept_id);
    ```

These logical operators are essential for creating complex filtering conditions in SQL `WHERE` clauses.

## Data Definition Language (DDL) Statements
DDL (Data Definition Language) statements are a subset of SQL commands used to define, modify, and manage database schema and structure. DDL statements create, alter, drop, and manage database objects such as tables, indexes, and views. Here are the key DDL statements and their purposes:

 ## CREATE
   - **Purpose**: Creates new database objects, such as tables, indexes, views, databases, or schemas.
   - **Syntax Example**: 
     ```sql
     CREATE TABLE employees (
         id INT PRIMARY KEY,
         name VARCHAR(50),
         department VARCHAR(50),
         salary DECIMAL(10, 2)
     );
     ```
      - **Explanation**: This command creates a table named `employees` with columns for `id`, `name`, `department`, and `salary`.

 ## ALTER
   - **Purpose**: Modifies existing database objects, allowing you to add, delete, or modify columns, change data types, rename columns, etc.
   - **Syntax Example**:
     ```sql
     -- Rename the table from 'employee' to 'employees'
     ALTER TABLE public.employee 
     RENAME TO public.employees;

     -- Add a new column 'location'
     ALTER TABLE public.employees 
     ADD COLUMN location VARCHAR(30);

     -- Increase the length of 'first_name' from 20 to 25
     ALTER TABLE public.employees 
     ALTER COLUMN first_name TYPE VARCHAR(25);

     -- Drop the existing column 'manager_id'
     ALTER TABLE public.employees 
     DROP COLUMN manager_id;

     -- Describe the table structure
     DESC TABLE public.employees;

     -- Drop the 'employees' table completely
     DROP TABLE public.employees;
     ```
     
 ## DROP
   - **Purpose**: Deletes database objects, such as tables, views, or indexes.
   - **Syntax Example**:
     ```sql
     DROP TABLE employees;
     ```
   - **Explanation**: Removes the `employees` table from the database along with all its data and structure. **Note**: This operation is irreversible.
 ## TRUNCATE
   - **Purpose**: Removes all rows from a table but keeps the structure intact for future use.
   - **Syntax Example**:
     ```sql
     TRUNCATE TABLE employees;
     ```
   - **Explanation**: Empties the `employees` table of all its data without deleting the table itself. It’s faster than a `DELETE` command without a `WHERE` clause, as it doesn't log individual row deletions.
___

## DML (Data Manipulation Language)

**DML** statements are used to add, modify, and delete data within tables.

 ## DML Commands
 1. **INSERT**
 2. **UPDATE**
 3. **DELETE**
    

___
 ## INSERT
The `INSERT` command is used to add data to a table.

 **Use cases:**
1. Insert a single record manually.
2. Insert multiple records manually.
3. Insert bulk data from other processes.
4. Insert data from one table into another table.


**Syntax:**
```sql
INSERT INTO table_name (columns)
VALUES (...);

INSERT INTO table_a (columns)
SELECT columns FROM table_b;
```
 ## UPDATE

The **UPDATE** command is used to modify existing data in a table. The SET keyword specifies the columns to update.

**Use cases:**

Update one or more records based on a condition.
Update the entire table.
**Syntax:**
```sql
DELETE FROM table_name;

DELETE FROM table_name
WHERE condition;
```

 ## DELETE
The DELETE command removes records from a table.

**Use cases:**

1.Delete one or more records based on a condition.
2.Delete all records from a table.

**Syntax:**
```sql
DELETE FROM table_name;

DELETE FROM table_name
WHERE condition;
```

 ## Interview Question

**What is the difference between DELETE and TRUNCATE?**

**DELETE:** This command can delete one, more, or all records from a table based on conditions. Deleted data can be rolled back if necessary. DELETE is a DML statement.
**TRUNCATE:** This command deletes all records from a table without the possibility of rollback. TRUNCATE is a DDL statement.
___

## SQL Clauses: WHERE and ORDER BY

 ## WHERE Clause

The `WHERE` clause is used to filter data from tables or views based on specified conditions.

- **Purpose**: Filters records that satisfy given conditions.
- **Multiple Conditions**: You can specify one or more conditions as needed.
- **Result**: The `SELECT` statement returns all records that match the conditions in `WHERE`.

**Syntax**:
```sql
SELECT * FROM table/view_name WHERE condition(s);
```

**Note:** SQL keywords are case-insensitive (e.g., SELECT or select), but data is case-sensitive (e.g., 'a' is different from 'A').

 ## ORDER BY Clause
The **ORDER BY** clause is used to sort query results in ascending or descending order.

 - Default Order: Ascending (ASC); use DESC for descending order.
 - Data Types: Works with numeric, string, and date data types.
 - Multiple Columns: You can sort based on multiple columns.
Syntax:

```sql
SELECT * FROM table/view_name ORDER BY column_name;
SELECT * FROM table/view_name ORDER BY column_name DESC;
SELECT column_list FROM table/view_name ORDER BY column1 DESC, column2;
```

**Examples of WHERE Clause Usage**
```sql
---Display only employees from department 20:
SELECT * FROM hrdata.employees WHERE dept_id = 20;

--Display employee_id and first_name for employees with a salary greater than 10,000:
SELECT employee_id, first_name, salary FROM hrdata.employees WHERE salary > 10000;

--Display employees with a yearly income of 200,000:
SELECT employee_id, first_name, salary AS mon_sal, salary * 12 AS year_sal
FROM hrdata.employees WHERE salary * 12 = 200000;

--List employees who joined on or after January 1, 2000:
SELECT employee_id, first_name, hire_date FROM hrdata.employees
WHERE hire_date >= TO_DATE('2000-01-01', 'YYYY-MM-DD');
--List employees from department 50 with a salary greater than 5000:

SELECT employee_id, dept_id, first_name, salary FROM hrdata.employees
WHERE dept_id = 50 AND salary > 5000;
```

 ## BETWEEN Operator
The **BETWEEN** operator is used to filter records within a specific range.

**Example:**
```sql
 --Employees with salaries between 8000 and 10000:
SELECT employee_id, first_name, salary FROM hrdata.employees
WHERE salary BETWEEN 8000 AND 10000;

---Using NOT BETWEEN: Employees with salaries not between 8000 and 10000:

SELECT employee_id, first_name, salary FROM hrdata.employees
WHERE salary NOT BETWEEN 8000 AND 10000;
```

**Examples of ORDER BY Clause Usage**
```sql
--Sort data by first_name in ascending order:
SELECT * FROM hrdata.employees ORDER BY first_name;

--Sort data by salary in descending order:
SELECT emp_id, first_name, salary FROM hrdata.employees ORDER BY salary DESC;

--Sort data by hire_date in ascending order:
SELECT emp_id, first_name, hire_date FROM hrdata.employees ORDER BY hire_date;

--Sort data by dept_id and then by salary in descending order:
SELECT dept_id, first_name, last_name, salary FROM hrdata.employees ORDER BY dept_id, salary DESC;
```