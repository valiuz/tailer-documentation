---
description: >-
  Learn how to create the SQL and DDL files corresponding to the workflow tasks
  of a Table to Table data operation.
---

# Table to Table SQL and DDL files

## 🗺 Overview

A SQL workflow is a sequence of tasks that feed tables in parallel or sequentially.

A workflow can be composed of the following task types:

* SQL task \("sql"\): instructions to load, merge and reorganize data.
* Table creation task \("create\_gbq\_table"\): instructions to create a destination table.
* Table copy task \("copy\_gbq\_table"\): instructions to duplicate a table \(provided in the [data operation configuration file](tables-to-tables-configuration-file.md)\).

## 🛢 SQL tasks

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
"bq_table_description": "List of collections and every related produit_id for every consumption zone.",
 "bq_table_schema": [
                {
                    "name": "x_date",
                    "type": "DATE",
                    "description": "date we calculate this indicators"
                },
                {
                    "name": "master_customer_id",
                    "type": "STRING",
                    "description": "master_customer_id"
                },
                {
                    "name": "nb_visites_web",
                    "type": "INTEGER",
                    "description": " number of visits on the website"
                }
            ],
            "bq_table_timepartitioning_field": "x_date"
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
          <li>&lt;b&gt;&lt;/b&gt;<a href="table-to-table-sql-and-ddl-files.md#data-types"><b>type</b></a>&lt;b&gt;&lt;/b&gt;</li>
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

### Data types

Tailer Platform supports the following data types.

#### Numeric types

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>int64</code>
      </td>
      <td style="text-align:left">
        <p>Integers are numeric values that do not have fractional components.</p>
        <p>They range from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>float64</code>
      </td>
      <td style="text-align:left">Floating point values are approximate numeric values with fractional components.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>numeric</code>
      </td>
      <td style="text-align:left">
        <p>This data type represents decimal values with 38 decimal digits of precision
          and 9 decimal digits of scale. (Precision is the number of digits that
          the number contains. Scale is how many of these digits appear after the
          decimal point.)</p>
        <p>It is particularly useful for financial calculations.</p>
      </td>
    </tr>
  </tbody>
</table>

#### Boolean type

| Name | Description |
| :--- | :--- |
| `boolean` |  This data type supports the `true`, `false`, and `null` values. It can perform some basic conversions, such as `'true'`, `'True'`, `True`, or 1 becoming `true`. |

#### String type

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>Variable-length character data.</p>
        <p>When converting data from string to a different data type, makes sure
          to use <code>safe_cast</code> when you&apos;re unsure about the data quality.</p>
      </td>
    </tr>
  </tbody>
</table>

#### Bytes type

| Name | Description |
| :--- | :--- |
| `bytes` | Variable-length binary data. This data type is rarely used but can be useful for characters with unusual encoding. |

#### Time types

{% hint style="info" %}
Only the `date`, `datetime` and `timestamp` data types \(not `time`\) allow table partitioning.
{% endhint %}

{% hint style="info" %}
Time zone management being difficult with BigQuery, prefer the UTC format.
{% endhint %}

| Name | Description |
| :--- | :--- |
| `date` | This data type represents a calendar date. It includes the year, month, and day. |
| `time` | This data type represents a time, as might be displayed on a watch, independent of a specific date. It includes the hour, minute, second, and subsecond. |
| `datetime` | This data type represents a date and time, as they might be displayed on a calendar or clock. It includes the year, month, day, hour, minute, second, and subsecond. |
| `timestamp` | This data type represents an absolute point in time, with microsecond precision. |

## ♒ Table copy tasks

If necessary, you can duplicate an existing table using a table copy task, for example to share the contents of a table with a partner inside their own dataset. Although it would be possible to use an SQL script with **SELECT \***, a copy task is more efficient. The parameters set in the JSON configuration file are sufficient, no specific file is required for this type of task.

