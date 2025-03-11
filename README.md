# Databricks Certification Prep

## Section 1

* Describe the relationship between the data lakehouse and the data warehouse.
    * Lakehouse = warehouse (structured data) + data lake (unstructured data)
    * Supports ACID transactions
* Identify the improvement in data quality in the data lakehouse over the data lake.
    * Schema Enforcement
    * Strong data governance (ACID)
    * Ensured data consistency
    * Optimizations (indexing and caching)
    * Supports BI tools and ML natively
* Compare and contrast silver and gold tables, which workloads will use a bronze table as a source, which workloads will use a gold table as a source.
    * Bronze table: raw data, ingestion layer
    * Silver table: where data is cleaned, structured, transformations applied 
    * Gold table: Enterprise-level, business-ready, aggregated data
    * Usage:
        * Bronze: ETL workloads, data validation
        * Silver: Analytical queries, reports, and ML models
        * Gold: BI dashboards, advanced analytics
* Identify elements of the Databricks Platform Architecture, such as what is located in the data plane versus the control plane and what resides in the customer’s cloud account
    * Data plane: 
        * Cluster VMs
        * Storage (DBFS)
    * Control plane: 
        * Web Application
        * Compute Orchestration (Cluster Management)
        * Unity Catalog
        * Queries and Code
        * Workflows
        * Notebooks
* Differentiate between all-purpose clusters and jobs clusters.
    * Job cluster:
        * Automatically created when a job is scheduled
        * Automatically terminated when job completes
        * Not scalable
        * For scheduled jobs, batch processing
    * All-Purpose Cluster:
        * Manually created
        * Manually terminated
        * Manual scaling
        * For interactive analysis, collaboration
* Identify how cluster software is versioned using the Databricks Runtime.
    * Databricks Runtime (DBR) versions control cluster configurations
    * Upgrading DBR ensures performance improvements and compatibility with newer features
* Identify how clusters can be filtered to view those that are accessible by the user
    * Cluster Permissions: View/Edit permissions based on role-based access control (RBAC)
    * Owner and Creator Tags: Lists clusters owned by specific users
* Describe how clusters are terminated and the impact of terminating a cluster.
    * Clusters are removed asynchronously when terminating
    * Cluster termination can be:
        * Manual:
            * Dbx UI
            * Dbx CLI
            * Dbx REST API
        * Automatic
            * Auto-termination (config cluster to terminate after specified period of inactivity)
    * Impact of terminating a cluster:
        * Cost savings
        * Stops all active processes
        * Data stored in memory is lost
        * Users must restart the cluster to continue work
* Identify a scenario in which restarting the cluster will be useful.
    * Refresh environment variables
    * Apply updates after a library installation
    * Clear memory and fix performance issues
* Describe how to use multiple languages within the same notebook.
    * Using Magic commands (i.e. %<language> before code):\
        `%python print("Python")`\
        `%sql SELECT * FROM table`
    * Databricks supports Scala, Python, SQL, and R
* Identify how to run one notebook from within another notebook.
    * Use the %run magic command:\
        `%run /path/to/child_notebook`
* Identify how notebooks can be shared with others.
    * Permissions Management: Share with users/groups
    * Share via URL: Read/write access based on user role
    * Export as HTML, IPython, or PDF
* Describe how Databricks Repos enables CI/CD workflows in Databricks.
    * Repos enable version control for notebooks, workflows, and configurations
    * Integration with GitHub, Azure DevOps, Bitbucket
    * Enables CI/CD by syncing notebooks with external repositories
    * Databricks Repos can commit or push code changes to trigger CI/CD processes
* Identify Git operations available via Databricks Repos.
    * Commit, Push, Pull changes to remote repositories
    * Branching & Merging for version control
    * Conflict Resolution tools for collaboration
* Identify limitations in Databricks Notebooks version control functionality relative to Repos.
    * Limited history tracking
    * Shared in Databricks only
    * Branching not supported

## Section 2

* Extract data from a single file and from a directory of files
    * Apache Spark: `spark.read.format(“<format>”).load(“/path/to/file”)`
    * Spark/Dbx SQL: `SELECT * FROM 'path/to/file_or_directory’`
* Identify the prefix included after the FROM keyword as the data type.
    * Indicates data source type\
        `SELECT * FROM csv.'/path/to/file.csv'`\
        `SELECT * FROM json.'/path/to/file.json'`
* Create a view, a temporary view, and a CTE as a reference to a file
    * View\
		`CREATE VIEW my_view AS`\
		`SELECT * FROM my_table`
    * Temporary view\
		```
        CREATE TEMP VIEW my_view AS
        SELECT * FROM my_table
        ```
    * CTE (Common Table Expression)\
		```
        WITH my_cte AS (
		   SELECT * FROM my_table WHERE category = ‘category’
		)
		SELECT * FROM my_cte
        ```
* Identify that tables from external sources are not Delta Lake tables.
    * Use SHOW TABLES and check the provider column
* Create a table from a JDBC connection and from an external CSV file
    * JDBC
    ```
		CREATE TABLE jdbc_table
		USING jdbc
		OPTIONS (
		   url “url”,
		   dbtable "my_table",
		   user "username",
		   password "password"
		)
    ```
    * CSV
    ```
		CREATE TABLE csv_table
		USING csv
		OPTIONS (path “/path”/to/file, header "true", inferSchema "true")
    ```
* Identify how the count_if function and the count where x is null can be used
    ```
    COUNT_IF(condition) - counts only rows meeting condition
		SELECT COUNT_IF(status = 'active') FROM users;
    ```
    ```
    COUNT(column) - skips NULL values
		SELECT COUNT(email) FROM users; -- Ignores NULL emails
    ```
* Identify how the count(row) skips NULL values.
* Deduplicate rows from an existing Delta Lake table.
    ```
		CREATE OR REPLACE TABLE dedup_table AS
		SELECT DISTINCT * FROM original_table;
    ```
* Create a new table from an existing table while removing duplicate rows.
* Deduplicate a row based on specific columns.
* Validate that the primary key is unique across all rows
    ```
		SELECT id, COUNT(*)
		FROM my_table
		GROUP BY id
		HAVING COUNT(*) > 1; -- Identifies duplicates
    ```
* Validate that a field is associated with just one unique value in another field.
    ```
		SELECT field1, COUNT(DISTINCT field2)
		FROM my_table
		GROUP BY field1
		HAVING COUNT(DISTINCT field2) > 1;
    ```
* Validate that a value is not present in a specific field.
    ```
		SELECT *
		FROM my_table
		WHERE field NOT IN ('value1', 'value2');
    ```
* Cast a column to a timestamp.
    ```
		SELECT CAST(date_str AS TIMESTAMP) FROM my_table;
    ```
* Extract calendar data from a timestamp.
    ```
		SELECT YEAR(timestamp_col), MONTH(timestamp_col), DAY(timestamp_col) 
		FROM my_table;
    ```
* Extract a specific pattern from an existing string column.
    ```
        SELECT REGEXP_EXTRACT(column_name, ‘<regex>’) FROM my_table;
    ```
* Utilize the dot syntax to extract nested data fields.
    ```
        SELECT json_col.field_name FROM my_table;
    ```
* Identify the benefits of using array functions.
    * Efficiently work with collections inside a single column
    * Perform operations like filtering, transformation, and aggregation
* Parse JSON strings into structs.
    ```
        SELECT FROM_JSON(json_column, ‘<schema>’) FROM my_table;
    ```
* Identify which result will be returned based on a join query.
    * INNER JOIN:
        * Returns values matching values in both tables
    * OUTER JOIN:
        * LEFT JOIN:
            * Returns all records from left table and matched values from right
        * RIGHT JOIN:
            * Returns all records from right table and matched values from left
        * FULL OUTER JOIN
            * Returns all records where there is a match in right or left table
* Identify a scenario to use the explode function versus the flatten function
    * EXPLODE(): Converts array elements into multiple rows
        * want to split data into multiple rows
    * FLATTEN(): Merges nested arrays into a single array
        * have nested lists (arrays) and you want to merge them into a single-level list
* Identify the PIVOT clause as a way to convert data from a long format to a wide format.
    ```
		SELECT * FROM (
		  SELECT category, year, sales FROM sales_data
		) PIVOT (
		  SUM(sales) FOR year IN (2022, 2023, 2024)
		);
    ```
* Define a SQL UDF.
    * User Defined Function
    ```
		CREATE FUNCTION my_udf(x INT) RETURNS INT
		RETURN x * 2;
    ```
* Identify the location of a function.
* Describe the security model for sharing SQL UDFs.
* Use CASE/WHEN in SQL code.
    ```
		CASE
		    WHEN condition1 THEN result1
		    WHEN condition2 THEN result2
		    WHEN conditionN THEN resultN
		    ELSE result
		END;
    ```
* Leverage CASE/WHEN for custom control flow.


## Section 3

* https://docs.databricks.com/aws/en/lakehouse/acid 
* ACID principles
    * Atomicity: all transactions either succeed or fail completely
    * Consistency: data remains valid and follows integrity constraints
    * Isolation: concurrent transactions do not interfere with each other
    * Durability: committed changes are permanent
* Identify where Delta Lake provides ACID transactions
    * Delta Lake achieves ACID transactions through Optimistic Concurrency Control (OCC) (multiple users can read/write data safely)
* Identify the benefits of ACID transactions.
    * Data Integrity: Ensures correctness even with concurrent operations.
    * Concurrent Writes: Multiple users can update the table safely.
    * Data Consistency: Prevents corrupted/incomplete data.
    * Time Travel & Versioning: Enables rollback and querying historical data.
* Identify whether a transaction is ACID-compliant.
    * Transaction is ACID-compliant if:
        * It is logged in the Delta transaction log (_delta_log directory)
        * The commit protocol ensures the transaction is fully applied
        * Isolation levels prevent partial updates or conflicting writes
* Compare and contrast data and metadata.
    * Data: collection of raw, unorganized facts that can be used in calculating, reasoning or planning
    * Metadata: information about the data (structure, schema, partitioning, versions etc.)
* Compare and contrast managed and external tables.
    * Managed: manages the data and metadata (schema), default table type in Spark
    * External: only manages metadata, acts as a pointer
* Identify a scenario to use an external table.
    * When data is shared across different Spark applications
    * When you want to leverage existing data stored in external locations without moving or copying it
    * When you need to ensure data is not deleted when dropping the table
* Create an external table. (https://docs.databricks.com/aws/en/tables/external)
    ```
		CREATE TABLE <catalog>.<schema>.<table-name>
		(<column-specification>)
		LOCATION 's3://<bucket-path>/<table-directory>';
    ```
* Create a managed table. (https://docs.databricks.com/aws/en/tables/managed)
    ```
		CREATE TABLE <catalog-name>.<schema-name>.<table-name>
		(<column-specification>);
    ```
* Identify the location of a table.\
    `DESCRIBE EXTENDED your_table_name`\
    `DESCRIBE DETAIL your_table_name`
* Inspect the directory structure of Delta Lake files.
    * /table_name/
        * _delta_log/
            * Transaction log files
        * .parquet data files
* Identify who has written previous versions of a table.
    * DESCRIBE HISTORY your_table_name;
    * Add “LIMIT <n>” to get the last n operations only
* Review a history of table transactions.
    * https://docs.databricks.com/gcp/en/delta/history might be helpful here
    ```
        SELECT * FROM delta.`dbfs:/user/hive/warehouse/my_table/` VERSION AS OF 5;
    ```
* Roll back a table to a previous version.
    ```
        RESTORE TABLE your_table TO VERSION AS OF 1
    ```
* Identify that a table can be rolled back to a previous version.
    * If it is logged in _delta_log
* Query a specific version of a table.
    ```
        SELECT * FROM table VERSION AS OF 123;
    ```

    ```
        SELECT * FROM table TIMESTAMP AS OF '2018-10-18T22:15:12.013Z’;
    ```
* Identify why Z-ordering is beneficial to Delta Lake tables.
    * improves data skipping efficiency by physically colocating related data in storage
    * especially useful for queries that filter on multiple columns
    * improves read performance in Delta Lake
* Identify how vacuum commits deletes.
    * removes data files no longer referenced by a Delta table that are older than the retention threshold
    * deletes are soft-deleted initially (they remain in the transaction history for some time (default: 7 days)
* Identify the kind of files Optimize compacts.
    * Default: bin-packing optimization is performed
    * compacts small files into larger Parquet files to improve read efficiency
* Identify CTAS as a solution.
    * CREATE TABLE AS SELECT
    * parallel operation that creates a new table based on the output of a SELECT statement
    * useful for data transformation, materialization, or creating partitioned tables
    ```
		CREATE TABLE new_table AS
		SELECT customer_id, sum(amount) as total_spent
		FROM transactions
		GROUP BY customer_id;
    ```
* Create a generated column.
    ```
		CREATE TABLE default.people (
		  id INT,
		  firstName STRING,
		  lastName STRING,
		  birthDate TIMESTAMP,
		  dateOfBirth DATE GENERATED ALWAYS AS (CAST(birthDate AS DATE))
		)
    ```
* Add a table comment.
    ```
        COMMENT ON TABLE my_table IS ‘This ‘is my comment;
    ```
* Use CREATE OR REPLACE TABLE and INSERT OVERWRITE
    * CREATE OR REPLACE TABLE:
    ```
		CREATE OR REPLACE TABLE sales (
		    id INT,
		    amount DOUBLE
		);
    ```
    * INSERT OVERWRITE:
    ```
		INSERT OVERWRITE sales
		SELECT * FROM new_sales;
    ```
* Compare and contrast CREATE OR REPLACE TABLE and INSERT OVERWRITE
    * CREATE OR REPLACE TABLE recreates a table, removing all existing data
    * INSERT OVERWRITE replaces the data but keeps the schema intact
* Identify a scenario in which MERGE should be used.
    * Merging new records while updating existing records
    * Used for complex operations like:
        * deduplicating data,
        * upserting change data,
        * applying SCD Type 2 operations
* Identify MERGE as a command to deduplicate data upon writing.
* Describe the benefits of the MERGE command.
    * Atomic Upsert: Combines insert, update, and delete in one step
    * Performance: Efficient compared to multiple independent queries
    * Data Integrity: Avoids duplicate data in target tables
* Identify why a COPY INTO statement is not duplicating data in the target table.
    * COPY INTO only loads new files from an external source
    * Keeps track of already loaded files in metadata
    * If the same file is reloaded, it is ignored unless explicitly forced
* Identify a scenario in which COPY INTO should be used.
    * Example: Loading files incrementally from a cloud storage
    * Used for bulk ingestion
* Use COPY INTO to insert data.
    ```
		COPY INTO my_table
		FROM 's3://my-bucket/data/'
		FILEFORMAT = CSV
		FORMAT_OPTIONS ('header' = 'true');
    ```
* Identify the components necessary to create a new DLT pipeline.
    * Source Data: cloud storage, Kafka, or relational databases
    * Notebook or Python Script
    * Pipeline Configuration:
        * Target storage location (Delta Lake table or Unity Catalog)
        * Compute cluster settings
        * Pipeline mode (Triggered vs. Continuous)
    * Libraries: Required dependencies
    * Auto Loader (Optional): For streaming data
* Identify the purpose of the target and of the notebook libraries in creating a pipeline.
    * Target: where the transformed data will be stored (generally Delta Lake location or Unity Catalog)
    * Notebook Libraries: dependencies for the pipeline (pyspark, dlt, delta)
* Compare and contrast triggered and continuous pipelines in terms of cost and latency
    * Triggered: 
        * Cost: lower - runs when triggered or on schedule
        * Latency: higher - data processed in batches
    * Continuous: 
        * Cost: higher - always running
        * Latency: lower - data processed almost in real time
* Identify which source location is utilizing Auto Loader.
    * The presence of "cloudFiles" format indicates Auto Loader is being used
    * Check:
    ```
		df = spark.readStream.format("cloudFiles") 
		    .option("cloudFiles.format", "json") 
		    .load("s3://your-bucket/path")
    ```
* Identify a scenario in which Auto Loader is beneficial.
    * Handling large-scale incremental file ingestion
    * Ingesting data from cloud storage
    * Working with streaming data
    * Managing schema evolution dynamically
* Identify why Auto Loader has inferred all data to be STRING from a JSON source
    * Auto Loader might infer all fields as STRING if:
        * The JSON schema is not explicitly defined.
        * There are inconsistent data types across JSON files.
        * The schema inference option is set incorrectly.
    * To enforce proper data types, specify the schema explicitly\
        `.schema("id INT, name STRING, timestamp TIMESTAMP")`
* Identify the default behavior of a constraint violation
    * Delta Live Tables enforce constraints but do not drop or fail when a constraint violation occurs
    * Instead, the row is marked as invalid and logged in the event log
* Identify the impact of ON VIOLATION DROP ROW and ON VIOLATION FAIL UPDATE for a constraint violation
    * ON VIOLATION DROP ROW: The violating row is removed from the final dataset
    * ON VIOLATION FAIL UPDATE: The entire update fails if any constraint violation occurs
* Explain change data capture and the behavior of APPLY CHANGES INTO
    * Change data capture (CDC) refers to the process of identifying and capturing changes made to data in a database and then delivering those changes in real-time to a downstream process or system
    * APPLY CHANGES INTO automates CDC
        * Detects changes in a source table
        * Applies them to the target table
        * Handles deduplication and merging automatically
* Query the events log to get metrics, perform audit logging, examine lineage.
    * Event log
        ```
            SELECT * FROM event_log WHERE pipeline_id = "your_pipeline_id";
        ```
    * Lineage Details
        ```
            SELECT event_type, details FROM event_log WHERE event_type = "flow_progress";
        ```
* Troubleshoot DLT syntax: Identify which notebook in a DLT pipeline produced an error, identify the need for LIVE in create statement, identify the need for STREAM in from clause.


## Section 4

* Identify the benefits of using multiple tasks in Jobs.
    * Modularity and maintenance: breaking a large job into multiple tasks makes it easier to manage (or troubleshoot).
    * Parallelism 
    * Orchestration: better tracking of data flow, ensuring downstream tasks only run when upstream tasks have succeeded (https://docs.databricks.com/aws/en/jobs/run-if)
* Set up a predecessor task in Jobs.
    1. Create multiple tasks within the same job
    2. For the task that must run after another, go to edit task and select from the drop-down menu “Depends on”
* Identify a scenario in which a predecessor task should be set up.
    * common example: data ingestion followed by data transformation
        * You have a notebook or workflow that ingests raw data from external sources. Only after completion, should the data transformation task begin
* Review a task's execution history.
    1. Go to a job
    2. Select Runs tab
    3. Click on a particular run (and select List view from the top) 
    4. To see detailed information about each task, click on a particular task name of your choosing. There, you can also access run events from the right side of the page
* Identify CRON as a scheduling opportunity.
    * https://docs.databricks.com/aws/en/jobs/scheduled
    * Cron is designed to schedule recurring tasks at regular intervals.
    * In the Schedules & Triggers section (Trigger type set to Scheduled) in the Jobs UI, you can choose to Show cron syntax and then provide a standard CRON expression.
    * Examples:
        * If you want to run a job every day for up to 30 days, you could use the cron expression "0 0 * * *" to run the job every day at midnight. However, you would need to implement additional logic outside of the cron schedule to stop the job after 30 days (https://community.databricks.com/t5/get-started-discussions/how-to-schedule-job-in-workflow-for-every-30-days/td-p/36889)
        * You could specify a CRON schedule to run at 2 AM on weekdays or to run at 6 PM on the last day of the month, etc.
* Debug a failed task.
    * Check the Run Logs: In the task’s run details, review the stdout/stderr logs, the Databricks driver logs, and any error messages reported.
    * If it’s a notebook task, open the notebook output. If it’s a Python or Spark submit task, look at the logs from the driver and executors.
    * Check cluster event logs (look for possible out of memory, worker failures etc.)
    * Ensure that the data needed by your task was successfully created by any predecessor tasks
    * Rerun with additional logging & debugging statements (if necessary)
* Set up a retry policy in case of failure.
    * Tasks settings (Edit task button) -> Retries -> Add
    * You can specify how many times a task should automatically retry upon failure and optionally add a delay between retries
* Create an alert in the case of a failed task.
    * Task View -> Notifications -> Add
    * Provide a destination (email, Slack, webhooks etc.) For all these to be available, you’ll have to navigate to Settings-> Notifications (under Workspace admin section) -> Manage
* Identify that an alert can be sent via email.
    * Task View -> Notifications -> Add. Here you should have the “Email address” option in the “Enter destination” drop-down menu
    * If not, navigate to Settings-> Notifications (under Workspace admin section) -> Manage -> Add destination -> Email from the “Type” drop-down menu



## Section 5

* Identify one of the four areas of data governance.
* Compare and contrast meta-stores and catalogs.
* Identify Unity Catalog securables.
* Define a service principal.
* Identify the cluster security modes compatible with Unity Catalog.
* Create a UC-enabled all-purpose cluster.
* Create a DBSQL warehouse.
* Identify how to query a three-layer namespace.
* Implement data object access control
* Identify colocating meta-stores with a workspace as best practice.
* Identify using service principals for connections as best practice.
* Identify the segregation of business units across catalog as best practice.
