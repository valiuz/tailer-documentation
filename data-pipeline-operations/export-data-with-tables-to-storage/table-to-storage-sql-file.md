---
description: >-
  This is the description of the SQL file used for a Table to Storage data
  operation.
---

# Table to Storage SQL file

To run a Table to Storage data operation, you first need to prepare a SQL query that will extract the data to export.

The SQL file must contains a BigQuery Standard SQL query. You can write it directly in the query editor of [BigQuery](https://console.cloud.google.com/bigquery) and then save it to  a .sql file.

Example:

```text
SELECT * FROM 'fd-tailer-datalake.rmp152_datalake.coupon' LIMIT 100000
```

This query will be executed when the data operation is deployed, and the result will display in the CSV export file and the table copy if specified.

