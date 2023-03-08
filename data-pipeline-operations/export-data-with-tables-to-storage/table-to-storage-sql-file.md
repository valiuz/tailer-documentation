---
description: >-
  This is the description of the SQL file used for a Table to Storage data
  operation.
---

# Table to Storage SQL file

To run a Table to Storage data operation, you first need to prepare a SQL query that will extract the data to export.

The SQL file must contains a BigQuery Standard SQL query. You can write it directly in the query editor of [BigQuery](https://console.cloud.google.com/bigquery) and then save it to a .sql file.

Example:

```sql
SELECT * FROM 'myproject.datalake.sales'
```

This query will be executed when the data operation is deployed, and the result will display in the CSV export file and the table copy if specified.

{% hint style="info" %}
You can now use constants with context but **only if** you stated version :"3" in the global parameters of the configuration **and if** your tailer SDK version is 1.3.9 or later. See [here](https://docs.tailer.ai/data-pipeline-operations/set-constants-with-context) for the constants with context. For example:

```
SELECT * FROM {{SOURCE_DATASET}}.sales
```
{% endhint %}

