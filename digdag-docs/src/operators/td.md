# td>: Treasure Data queries

**td>** operator runs a Hive or Presto query on Treasure Data.

    _export:
      td:
        database: www_access

    +simple_query:
      td>: queries/simple_query.sql

    +create_new_table_using_result_of_select:
      td>: queries/select_sql.sql
      create_table: mytable_${session_date_compact}

    +insert_result_of_select_into_a_table:
      td>: queries/select_sql.sql
      insert_into: mytable

    +result_with_connection:
      td>: queries/select_sql.sql
      result_connection: connection_created_on_console

    +result_with_connection_with_settings:
      td>: queries/select_sql.sql
      result_connection: my_s3_connection
      result_settings:
        bucket: my_bucket
        path: /logs/

## Examples

  * [Examples](https://github.com/treasure-data/workflow-examples/tree/master/td)

## Secrets

* **td.apikey**: API_KEY

  The Treasure Data API key to use when running Treasure Data queries.

## Options

* **td>**: FILE.sql

  Path to a query template file. This file can contain `${...}` syntax to embed variables.

  Examples:

  ```
  td>: queries/step1.sql
  ```

* **create_table**: NAME

  Name of a table to create from the results. This option deletes the table if it already exists.

  This option adds DROP TABLE IF EXISTS; CREATE TABLE AS (Presto) or INSERT OVERWRITE (Hive) commands before the SELECT statement. If the query includes a `-- DIGDAG_INSERT_LINE` line, the commands are inserted there.

  Examples:

  ```
  create_table: my_table
  ```

* **insert_into**: NAME

  Name of a table to append results into. The table is created if it does not already exist.

  This option adds INSERT INTO (Presto) or INSERT INTO TABLE (Hive) command at the beginning of SELECT statement. If the query includes `-- DIGDAG_INSERT_LINE` line, the command is inserted to the line.

  Examples:

  ```
  insert_into: my_table
  ```

* **download_file**: NAME

  Saves query result as a local CSV file.

  Examples:

  ```
  download_file: output.csv
  ```

* **store_last_results**: BOOLEAN

  Stores the first 1 row of the query results to `${td.last_results}` variable (default: false).
  td.last_results is a map of column name and a value. To access to a single value, you can use `${td.last_results.my_count}` syntax.

  Examples:

  ```
  store_last_results: true
  ```

* **preview**: BOOLEAN

  Tries to show some query results to confirm the results of a query.

  Examples:

  ```
  preview: true
  ```

* **result_url**: NAME
  Output the query results to the URL:

  Examples:

  ```
  result_url: tableau://username:password@my.tableauserver.com/?mode=replace
  ```

* **database**: NAME

  Name of a database.

  Examples:

  ```
  database: my_db
  ```

* **endpoint**: ADDRESS

  API endpoint (default: api.treasuredata.com).

* **use_ssl**: BOOLEAN

  Enable SSL (https) to access to the endpoint (default: true).

* **engine**: presto

  Query engine (`presto` or `hive`).

  Examples:

  ```
  engine: hive
  ```

  ```
  engine: presto
  ```

* **priority**: 0

  Set Priority (From `-2` (VERY LOW) to `2` (VERY HIGH) , default: 0 (NORMAL)).

* **result_connection**: NAME

  Use a connection to write the query results to an external system.

  You can create a connection using the web console.

  Examples:

  ```
  reuslt_connection: my_s3_connection
  ```

* **result_settings**: MAP

  Add additional settings to the result connection.

  This option is valid only if `result_connection` option is set.

  Examples:

  ```
  reuslt_connection: my_s3_connection
  result_settings:
    bucket: my_s3_bucket
    path: /logs/
  ```

  ```
  reuslt_connection: my_http
  result_settings:
    path: /endpoint
  ```

* **presto_pool_name**: NAME

  Name of a resource pool to run the query in.
  Applicable only when ``engine`` is ``presto``.

  Examples:

  ```
  presto_pool_name: poc
  ```

* **hive_pool_name**: NAME

  Name of a resource pool to run the query in.
  Applicable only when ``engine`` is ``hive``.

  Examples:

  ```
  engine: hive
  hive_pool_name: poc
  ```

## Output parameters

* **td.last_job_id**

  The job id this task executed.

  Examples:

  ```
  52036074
  ```

* **td.last_results**

  The first 1 row of the query results as a map. This is available only when `store_last_results: true` is set.

  Examples:

  ```
  {"path":"/index.html","count":1}
  ```
