# snowpipe

A Snowflake Task allows scheduled execution of SQL statements including calling a stored procedure or logic using Snowflake Scripting.

**Important Points to remember**

1.Only one SQL statement we have to include while creating the Snowflake Task.
2.If you have more than one SQL Statement we have to create Stored Procedure and call the stored
procedure in the Task
3.Once you create any Task by Default it will be Suspend State but by using Alter Statement we can
resume or suspend the task if you need.
4.The Schedule Interval time will be minimum 1 Minutes and Maximum will be 7 Days.

5.If you want to execute any task it required, some compute resources so basically we can have defined
in two ways.
- User-managed (Virtual warehouse)
- Snowflake-managed compute model

If you want to execute manually we have to specify Execute Task and specify the Task Name.

The Snowflake task engine has a CRON and NONCRON different two scheduling ways.
means directly we can specify the scheduled time interval like 5, 10 Minutes something
like that.

| Minute | Hour | Day | Month | WeekDay | Description        |
|--------|------|-----|-------|---------|--------------------|
|   *    |  *   |  *  |   *   |    *    | Every Minute        |
|   0    |  2   |  *  |   *   |    *    | Every Two Hours     |
|   0    |  5   |  *  |   *   |   SUN   | Every Sunday 5 AM   |
|  10    |  *   |  *  |   *   |    *    |                     |

 **Snowflake Task Tree**
Consider a scenario where you wanted to trigger another task immediately after an initial task
completes and so on. This is suppdrted in Snowflake by creating a B-Tree-like task structure
which is referred as Task Tree.

```sql
CREATE OR REPLACE TASK my_daily_task
  WAREHOUSE = my_warehouse
  SCHEDULE = 'USING CRON 0 6 * * * UTC' or '2 minutes'
  WHEN SYSTEMSSTREAM_HAS_DATA('STAGING.STREAM_STG_EMPL')
AS
  INSERT INTO my_table (SELECT * FROM my_source_table WHERE date_column = CURRENT_DATE);
  ```

**In this example, the task is scheduled to run every day at 6 AM UTC and insert data from one table into another.**

WHEN'SYSTEMSSTREAM_HAS_DATA('STAGING.STREAM_STG_EMPL'):' This line specifies that the task should only execute when there is new data in the stream STAGING.STREAM_STG_EMPL**

Example of Task Chaining (DAG):

```sql
CREATE OR REPLACE TASK task_1
  WAREHOUSE = my_warehouse
  SCHEDULE = 'USING CRON 0 2 * * * UTC'
AS
  -- SQL query for task 1
  INSERT INTO table1 (SELECT * FROM source_table WHERE condition);

CREATE OR REPLACE TASK task_2
  WAREHOUSE = my_warehouse
  AFTER task_1
AS
  -- SQL query for task 2
  INSERT INTO table2 (SELECT * FROM table1 WHERE some_other_condition);
  ```

Here, task_2 will only execute after task_1 has finished successfully.

**Test the Task:**

Use the **MANUAL** trigger to test the task.
```sql
ALTER TASK daily_report_task RESUME;
CALL SYSTEM$TASK_TEST('daily_report_task');
```
**Verify the DAG:**

Check the status of tasks using:
```sql
SHOW TASKS;
```


**Monitoring Task Execution and Handling Failures**
**Objective:** Set up task monitoring and learn how to handle task failures.

Steps:

Use the **INFORMATION_SCHEMA** views to track task execution.
Write SQL to monitor **task failures, successes, and the last execution time**.
Add retry logic or notification on task failure.
Example query to monitor task execution:

```sql
Copy code
SELECT *
FROM INFORMATION_SCHEMA.TASK_HISTORY
WHERE name = 'MY_TASK_NAME';
```
**Resume or Suspend a task:**
```sql
ALTER TASK my_daily_task RESUME;  -- To resume the task
ALTER TASK my_daily_task SUSPEND; -- To suspend the task
```

# Scd Type-1

**Slowly Changing Dimension (SCD) Type 1** is a method used to manage changes in data where **no history** is maintained. **When a record is updated, the old value is simply overwritten with the new one,** and only the current state of the data is stored. Unlike SCD Type 2, SCD Type 1 does not retain historical data, and it’s typically used for attributes that are not critical to keep track of historically, like a misspelled name or phone number update.

**Key Features of SCD Type 1:**
- Overwrites the existing data.
- No history is maintained.
- It is simpler than SCD Type 2 because you don’t need to track multiple versions of the same record.
**Example Use Case:**
Let’s assume we have a table called **customer_dim,** which contains customer information such as **customer name** and **address.** Whenever a customer’s name or address changes, we simply overwrite the existing data.

### Implementation of SCD Type 1 in Snowflake Using Streams and SQL `MERGE` Statement

**Snowflake Tools:**
- **Streams**: Snowflake streams allow you to track changes (inserts, updates, deletes) made to a table. Streams capture the delta of changes between two points in time, making them useful for identifying which rows need to be updated.
  
- **MERGE**: The `MERGE` SQL statement allows you to conditionally update, insert, or delete data from a table based on changes from a source table.


1. Schema and Table Creation:
```sql
-- Create or replace the target schema (stores dimension tables)
CREATE OR REPLACE SCHEMA target;

-- Create or replace the source schema (stores staging/source data)
CREATE OR REPLACE SCHEMA source;

-- Create the source table (customer data)
CREATE OR REPLACE TABLE source.customer_source (
  customer_id INTEGER,
  customer_name STRING,
  customer_address STRING,
  last_updated DATE
);

-- Create the dimension table (holds current customer data)
CREATE OR REPLACE TABLE target.customer_dim (
  customer_id INTEGER PRIMARY KEY,
  customer_name STRING,
  customer_address STRING
);
Use code with caution.
```
2. Sample Data Insertion:

```SQL
-- Insert initial data into the dimension table
INSERT INTO target.customer_dim (customer_id, customer_name, customer_address)
VALUES
  (1, 'Alice Johnson', '123 Main St'),
  (2, 'Bob Smith', '456 Oak St'),
  (3, 'Charlie Brown', '789 Pine St');

-- Insert sample data into the source table (including an update)
INSERT INTO source.customer_source (customer_id, customer_name, customer_address, last_updated)
VALUES
  (1, 'Alice Johnson', '987 Elm St', CURRENT_DATE),  -- Address changed
  (2, 'Bob Smith', '456 Oak St', CURRENT_DATE);    -- No change
Use code with caution.
```
3. Stream Creation and SCD Type 1 Logic:

```SQL

--Create a stream to capture changes made to the source table
CREATE OR REPLACE STREAM customer_source_stream ON TABLE source.customer_source;

-- Use MERGE statement for SCD Type 1 logic (overwrite existing data)
-- Procedure to update existing records

CREATE PROCEDURE update_dim_customers()
RETURNS VARCHAR
LANGUAGE SQL
AS
$$
BEGIN
MERGE INTO target.customer_dim AS target
USING customer_source_stream AS source
ON target.customer_id = source.customer_id

-- Update existing records with new values (SCD Type 1 behavior)
WHEN MATCHED THEN
  UPDATE SET
    target.customer_name = source.customer_name,
    target.customer_address = source.customer_address

-- Insert new records (if any)
WHEN NOT MATCHED THEN
  INSERT (customer_id, customer_name, customer_address)
  VALUES (source.customer_id, source.customer_name, source.customer_address);
  END;
$$;
```

# DataLoading

```sql
--creating a storage integration object
CREATE OR REPLACE STORAGE INTEGRATION my_s3_integration
  TYPE = 'EXTERNAL_STAGE'
  STORAGE_PROVIDER = 'S3'
  ENABLED = TRUE
  STORAGE_AWS_IAM_USER_ARN = 'arn:aws:iam::YOUR_AWS_ACCOUNT_ID:user/YOUR_IAM_USER'
  STORAGE_ALLOWED_LOCATIONS = ('s3://your-bucket-name/path','s3://your-bucket-name/path')
  COMMENT = 'Integration for accessing S3 bucket';

--Get external_id and update it in IAM(Trust polices)
DESC integration s3_int; --copy(AWS-IAM-user-ARN)

  --s3(simple storage service)
  --ARN(Amazon Resources names)
  --IAM(Integrated account mangement)


--Alter your storage integration
alter storage integration s3_int
set STORAGE_ALLOWED_LOCATIONS = ('s3://awss3bucketjana/csv/', 's3://awss3bucketjana/json/','s3://awss3bucketjana/pipes/csv/');
```

**file format**
```sql
CREATE OR REPLACE FILE FORMAT customer_data_csv_format
TYPE = 'CSV'
SKIP_HEADER = 1
FIELD_DELIMITER = ','
empty_field_as_null = True;
```

```sql
-- Create a stage for S3 CSV files using the integration and file format
CREATE OR REPLACE STAGE UniversalMusicGroup.External_Stages.aws_s3_csv
  URL = 's3://customerdata-snowflake-umg-prod/customer_data'
  STORAGE_INTEGRATION = s3_int
  FILE_FORMAT = UniversalMusicGroup.Public.customer_data_csv_format;
```
```sql
-- Copy data from the external stage into the customer_data table
COPY INTO UniversalMusicGroup.Source.customer_data
FROM @UniversalMusicGroup.External_Stages.aws_s3_csv
FILE_FORMAT = (FORMAT_NAME = UniversalMusicGroup.Public.customer_data_csv_format)
ON_ERROR = 'CONTINUE';  -- Skip any rows with errors; other options are 'ABORT_STATEMENT' or 'SKIP_FILE'
```

### LAB SESSION

```sql
-- Create the database if it doesn't exist
CREATE DATABASE IF NOT EXISTS UniversalMusicGroup;

-- Create a storage integration object for S3 access
CREATE OR REPLACE STORAGE INTEGRATION s3_int
  TYPE = EXTERNAL_STAGE
  STORAGE_PROVIDER = S3
  ENABLED = TRUE
  STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::010526271403:role/Snowflake_s3_integration'
  STORAGE_ALLOWED_LOCATIONS = ('s3://customerdata-snowflake-umg-prod/customer_data')
  COMMENT = 'Integration with AWS S3 bucket for customer data';

-- Verify storage integration details
DESC INTEGRATION s3_int;

-- Create a file format for CSV data
CREATE OR REPLACE FILE FORMAT customer_data_csv_format
  TYPE = 'CSV'
  FIELD_OPTIONALLY_ENCLOSED_BY = '"'
  SKIP_HEADER = 1
  NULL_IF = ('NULL', 'null', '')
  FIELD_DELIMITER = ','
  EMPTY_FIELD_AS_NULL = TRUE;

-- Create schema for external stages if it doesn't exist
CREATE OR REPLACE SCHEMA UniversalMusicGroup.External_Stages;

-- Create a stage for S3 CSV files using the integration and file format
CREATE OR REPLACE STAGE UniversalMusicGroup.External_Stages.aws_s3_csv
  URL = 's3://customerdata-snowflake-umg-prod/customer_data'
  STORAGE_INTEGRATION = s3_int
  FILE_FORMAT = UniversalMusicGroup.Public.customer_data_csv_format;

-- List files in the stage (optional verification)
LIST @UniversalMusicGroup.External_Stages.aws_s3_csv;

-- Create a schema for source data if it doesn't exist
CREATE SCHEMA IF NOT EXISTS UniversalMusicGroup.Source;

-- Create the customer data table in the public schema
CREATE OR REPLACE TABLE UniversalMusicGroup.Source.customer_data (
    CustomerID INT NOT NULL,
    Name STRING,
    Email STRING,
    Phone STRING,
    Address STRING,
    City STRING,
    State STRING,
    ZipCode STRING,
    RegistrationDate DATE
);

-- Optional: Select all records from the customer data table to verify creation
SELECT * FROM UniversalMusicGroup.Source.customer_data;


-- Copy data from the external stage into the customer_data table
COPY INTO UniversalMusicGroup.Source.customer_data
FROM @UniversalMusicGroup.External_Stages.aws_s3_csv
FILE_FORMAT = (FORMAT_NAME = UniversalMusicGroup.Public.customer_data_csv_format)
ON_ERROR = 'CONTINUE';  -- Skip any rows with errors; other options are 'ABORT_STATEMENT' or 'SKIP_FILE'


--verify the data 
select * from UNIVERSALMUSICGROUP.SOURCE.CUSTOMER_DATA
```
# Dynamic table


A dynamic table in Snowflake is a new feature that automates the process of keeping data transformations up-to-date in near real-time. It is designed to automatically maintain the data in a table based on a specified query or transformation. Snowflake monitors changes in the source tables, and when any change is detected, it automatically triggers the dynamic table to update, ensuring that data remains current without manual intervention.

```sql
CREATE OR REPLACE DYNAMIC TABLE sales_summary
  WAREHOUSE = 'MY_WAREHOUSE'
  LAG = '5 MINUTE'  -- Specifies a lag tolerance to balance refresh rate and cost
AS
SELECT 
    customer_id,
    SUM(total_amount) AS total_sales,
    COUNT(order_id) AS num_orders
FROM sales_data
GROUP BY customer_id;

```

### Example Explanation for `sales_summary` Dynamic Table

### Automatic Updates
The **sales_summary** table will stay updated as new records are added or modified in the **sales_data** table. Snowflake automatically tracks changes in the source data and applies updates to the dynamic table.

### LAG Setting
The LAG setting of 5 MINUTE` means that Snowflake will refresh the table approximately every five minutes, balancing real-time data needs with performance and cost considerations.









