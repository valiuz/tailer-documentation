---
description: >-
  Learn how to create the SQL and DDL files corresponding to the workflow tasks
  of a Table to Table data operation.
---

# Table to Table SQL and DDL files

## 🗺️ Overview

A SQL workflow is a sequence of tasks that feed tables in parallel or sequentially.

A workflow can be composed of the following task types:

* SQL task \("sql"\): instructions to load, merge and reorganize data.
* Table creation task \("create\_gbq\_table"\): instructions to create a destination table.
* Table copy task \("copy\_gbq\_table"\): instructions to duplicate a table \(provided in the [data operation configuration file](tables-to-tables-configuration-file.md)\).

## 🛢️ SQL tasks

SQL tasks are steps of the workflow. Each SQL task is defined with a .sql file that contains the query. You can write the queries directly in the query editor of [BigQuery](https://console.cloud.google.com/bigquery) and then save them to .sql files.

{% hint style="info" %}
There can be only one SQL query for each task. If a table need several SQL queries to be well loaded, you need to execute a task for each of them.
{% endhint %}

{% hint style="info" %}
The name of the SQL file should be the same as the SQL task.
{% endhint %}

Example:

```text
#standardSQL

SELECT

t1.customer_id,

COUNT(distinct t2.id_family) as nb_families,

SUM(t3.amount) as total_amount

FROM Project_A.Dataset_Y.Tab_Y1 t1

INNER JOIN project_A.Dataset_S.Tab_S1 t2

ON t1.id_product=t2.id_article

INNER JOIN project_A.Dataset_U.Tab_U3 t3

ON t1.id_loyalty_card=t3.id_card
```

## 🆕 Table creation tasks

Once the SQL queries are ready, you need to use one or several DDL files to create the destination BigQuery tables that will contain the output data.

### Example

```text
{
  "bq_table_description": "List of collections and every related produit_id.",
  "bq_table_schema": [
    {
      "name": "PK_collection_plan",
      "type": "STRING",
      "description": "Primary key."
    },
    {
      "name": "reference_id",
      "type": "STRING",
      "description": "Reference ID (R), gathering every SKU of a same reference."
    },
    {
      "name": "code_coloris",
      "type": "STRING",
      "description": "Color code."
    }
  ],
    "bq_table_clustering_fields": [
    "reference_id",
    "code_coloris"
  ],
  "bq_table_timepartitioning_field",
  "bq_table_timepartitioning_expiration_ms",
  "bq_table_timepartitioning_require_partition_filter"
}
```

### **Parameters**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>bq_table_description</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Description of the BigQuery table.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>bq_table_schema</b>
        </p>
        <p>type: array</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>BigQuery table schema. It contains a list of fields corresponding to the
          number of columns it will contain.</p>
        <p>Each field described has three attributes:</p>
        <ul>
          <li><b>name</b>
          </li>
          <li><b>type</b>
          </li>
          <li><b>description</b>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>bq_table_clustering_fields</b>
        </p>
        <p>type: array</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>List of fields used when clustering is enabled.</p>
        <p>The table data will be automatically organized based on the contents of
          the fields you specify. Their order determines the sort order of the data.</p>
        <p>If this parameter is set, time partitioning will be automatically enabled
          on the table. If you don&apos;t set partitioning parameters, default values
          will be used.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>bq_table_timepartitioning_field</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>If this parameter is set, the table will be partitioned by this field.</p>
        <p>If not, the table will be partitioned by pseudo column <b>_PARTITIONTIME</b>.</p>
        <p>The field must be a top-level <b>TIMESTAMP</b> or <b>DATE</b> field. Its mode
          must be <b>NULLABLE</b> or <b>REQUIRED</b>.</p>
        <p>(Refer to <a href="https://googleapis.dev/python/bigquery/latest/generated/google.cloud.bigquery.table.TimePartitioning.html#google.cloud.bigquery.table.TimePartitioning">BigQuery documentation</a> for
          more information.)</p>
        <p>Note: You can set this parameter to a field that equals to <b>DATE(&apos;&apos;)</b>.
          Then, if you relaunch an execution with a partition, and if <b>default_write_disposition</b> is
          set to &quot;WRITE_APPEND&quot; in the JSON configuration file, Tailer
          will check if the corresponding partition already exists in the table:</p>
        <ul>
          <li>If it does, it will delete it, and replace it with the current execution
            data.</li>
          <li>If not, it will add them.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>bq_table_timepartitioning_expiration_ms</b>
        </p>
        <p>type: integer</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Number of milliseconds for which to keep the storage for a partition.</p>
        <p>(Refer to <a href="https://googleapis.dev/python/bigquery/latest/generated/google.cloud.bigquery.table.TimePartitioning.html#google.cloud.bigquery.table.TimePartitioning">BigQuery documentation</a> for
          more information.)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>bq_table_timepartitioning_require_partition_filter</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>If set to true, queries over the partitioned table require a partition
          filter that can be used for partition elimination to be specified.</p>
        <p>(Refer to <a href="https://googleapis.dev/python/bigquery/latest/generated/google.cloud.bigquery.table.Table.html#google.cloud.bigquery.table.Table.require_partition_filter">BigQuery documentation</a> for
          more information.)</p>
      </td>
    </tr>
  </tbody>
</table>

## ♒ Table copy tasks

If necessary, you can duplicate an existing table using a table copy task, for example to share the contents of a table with a partner inside their own dataset. Although it would be possible to use an SQL script with **SELECT \***, a copy task is more efficient. The parameters set in the JSON configuration file are sufficient, no specific file is required for this type of task.

