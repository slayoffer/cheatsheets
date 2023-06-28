The way to check the size of your database on disk will depend on the database management system you're using. Here are some ways to do it for the most popular systems:

1. **MySQL**:
   You can use the following SQL query, which will give you the size of each table in each database:

    ```sql
   SELECT 
       table_schema as `Database`,
       table_name AS `Table`,
       round(((data_length + index_length) / 1024 / 1024), 2) `Size in MB` 
   FROM 
       information_schema.TABLES 
   ORDER BY 
       (data_length + index_length) DESC;
    ```

    If you want to check the size of a particular database, replace `'database_name'` with the name of your database in the following query:

    ```sql
   SELECT 
       table_schema AS `Database`, 
       SUM(data_length + index_length) / 1024 / 1024 AS `Size in MB`
   FROM 
       information_schema.TABLES 
   WHERE 
       table_schema = 'database_name'
   GROUP BY 
       table_schema;
    ```

2. **PostgreSQL**:
   You can get the size of all databases using the following SQL query:

    ```sql
   SELECT 
       pg_size_pretty(pg_database_size(pg_database.datname)) AS size,
       pg_database.datname AS name
   FROM 
       pg_database
   ORDER BY 
       pg_database_size(pg_database.datname) DESC;
    ```

    If you want to check the size of a particular database, replace `'database_name'` with the name of your database in the following query:

    ```sql
   SELECT 
       pg_size_pretty(pg_database_size('database_name'));
    ```

3. **SQL Server**:
   You can use the following SQL query to determine the size of your database:

    ```sql
   USE database_name;
   EXEC sp_spaceused;
    ```

    This will return the size of the database, along with other information like the unallocated space.

4. **SQLite**:
   SQLite doesn't provide a built-in method to check the size of a database. However, because an SQLite database is a single file, you can check the file size in your operating system. In Linux or MacOS, you can use the following command in the terminal:

    ```bash
   du -h /path/to/database.db
    ```

    In Windows, you can check the file size in the file properties from the Windows Explorer.

Please replace 'database_name' and '/path/to/database.db' with your actual database name and path.