## Data Definition Language (DDL) Statements

DDL (Data Definition Language) statements are a subset of SQL commands used to define, modify, and manage database schema and structure. DDL statements create, alter, drop, and manage database objects such as tables, indexes, and views. Here are the key DDL statements and their purposes:

## 1. `CREATE`
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

## 2. `ALTER`
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

## 3. `DROP`
   - **Purpose**: Deletes database objects, such as tables, views, or indexes.
   - **Syntax Example**:
     ```sql
     DROP TABLE employees;
     ```
   - **Explanation**: Removes the `employees` table from the database along with all its data and structure. **Note**: This operation is irreversible.

## 4. `TRUNCATE`
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

---

### INSERT
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

### UPDATE

The UPDATE command is used to modify existing data in a table. The SET keyword specifies the columns to update.

**Use cases:**

Update one or more records based on a condition.
Update the entire table.
**Syntax:**
```sql
DELETE FROM table_name;

DELETE FROM table_name
WHERE condition;
```

### DELETE
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
### Interview Question

**What is the difference between DELETE and TRUNCATE?**

**DELETE:** This command can delete one, more, or all records from a table based on conditions. Deleted data can be rolled back if necessary. DELETE is a DML statement.
**TRUNCATE:** This command deletes all records from a table without the possibility of rollback. TRUNCATE is a DDL statement.
___
