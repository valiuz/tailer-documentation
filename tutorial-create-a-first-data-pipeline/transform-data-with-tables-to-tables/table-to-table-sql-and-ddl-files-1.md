---
description: >-
  Learn how to create the Data Definition Language (DDL) file corresponding to
  the workflow tasks of a Table to Table data operation.
---

# DDL script file

### :map: Overview

A SQL workflow is a sequence of tasks that feed tables in parallel or sequentially. The DDL file gives instructions to create a destination table.

### ⎨⎬ Creation task

Once the SQL queries are ready, you need to use one or several DDL files to create the destination BigQuery tables that will contain the output data.

### :video\_camera: DDL script video

```
{
	"bq_table_description": "Iowa Liquor aggregation by store for 2019's sales",
	"bq_table_clustering_fields": ["store_number"],
	"bq_table_timepartitioning_field": "isoweek_monday",
	"bq_table_schema": [{
			"name": "isoweek_monday",
			"type": "DATE",
			"description": "Unique number assigned to the store who ordered the liquor."
		},
		{
			"name": "isoweek_number",
			"type": "INTEGER",
			"description": "Name of store who ordered the liquor."
		},
		{
			"name": "store_number",
			"type": "STRING",
			"description": "Unique number assigned to the store who ordered the liquor."
		},
		{
			"name": "store_name",
			"type": "STRING",
			"description": "Name of store who ordered the liquor."
		},
		{
			"name": "sale_dollars",
			"type": "FLOAT",
			"description": "sum of the 2019's sales for the store"
		},
		{
			"name": "volume_sold_liters",
			"type": "FLOAT",
			"description": "sum of the volume sold in 2019 for the store in liters"
		},
		{
			"name": "p_vodka",
			"type": "FLOAT",
			"description": "ratio of the sale for the vodka category"
		},
		{
			"name": "p_whisky",
			"type": "FLOAT",
			"description": "ratio of the sale for the whisky category"
		},
		{
			"name": "p_rum",
			"type": "FLOAT",
			"description": "ratio of the sale for the rum category"
		},
		{
			"name": "p_liqueur",
			"type": "FLOAT",
			"description": "ratio of the sale for the liqueur category"
		},
		{
			"name": "p_tequila",
			"type": "FLOAT",
			"description": "ratio of the sale for the tequila category"
		},
		{
			"name": "p_schnapps",
			"type": "FLOAT",
			"description": "ratio of the sale for the schnapps category"
		},
		{
			"name": "p_gin",
			"type": "FLOAT",
			"description": "ratio of the sale for the gin category"
		},
		{
			"name": "p_cocktail",
			"type": "FLOAT",
			"description": "ratio of the sale for the cocktail category"
		},
		{
			"name": "p_brandy",
			"type": "FLOAT",
			"description": "ratio of the sale for the brandy category"
		},
		{
			"name": "p_spirit",
			"type": "FLOAT",
			"description": "ratio of the sale for the spirit category"
		},
		{
			"name": "p_special",
			"type": "FLOAT",
			"description": "ratio of the sale for the special category"
		}
	]
}
```

### **Parameters**

| Parameter                                                                                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p><strong>bq_table_description</strong></p><p>type: string</p><p>mandatory</p>                               | Description of the BigQuery table.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p><strong>bq_table_schema</strong></p><p>type: array</p><p>mandatory</p>                                     | <p>BigQuery table schema. It contains a list of fields corresponding to the number of columns it will contain.</p><p>Each field described has three attributes:</p><ul><li><strong>name</strong></li><li><a href="table-to-table-sql-and-ddl-files-1.md#data-types"><strong>type</strong></a></li><li><strong>description</strong></li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| <p><strong>bq_table_clustering_fields</strong></p><p>type: array</p><p>optional</p>                           | <p>List of fields used when clustering is enabled.</p><p>The table data will be automatically organized based on the contents of the fields you specify. Their order determines the sort order of the data.</p><p>If this parameter is set, time partitioning will be automatically enabled on the table. If you don't set partitioning parameters, default values will be used.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| <p><strong>bq_table_timepartitioning_field</strong></p><p>type: string</p><p>optional</p>                     | <p>If this parameter is set, the table will be partitioned by this field.</p><p>If not, the table will be partitioned by pseudo column <strong>_PARTITIONTIME</strong>.</p><p>The field must be a top-level <strong>TIMESTAMP</strong> or <strong>DATE</strong> field. Its mode must be <strong>NULLABLE</strong> or <strong>REQUIRED</strong>.</p><p>(Refer to <a href="https://googleapis.dev/python/bigquery/latest/generated/google.cloud.bigquery.table.TimePartitioning.html#google.cloud.bigquery.table.TimePartitioning">BigQuery documentation</a> for more information.)</p><p>Note: You can set this parameter to a field that equals to <strong>DATE('')</strong>. Then, if you relaunch an execution with a partition, and if <strong>default_write_disposition</strong> is set to "WRITE_APPEND" in the JSON configuration file, Tailer will check if the corresponding partition already exists in the table:</p><ul><li>If it does, it will delete it, and replace it with the current execution data.</li><li>If not, it will add them.</li></ul> |
| <p><strong>bq_table_timepartitioning_expiration_ms</strong></p><p>type: integer</p><p>optional</p>            | <p>Number of milliseconds for which to keep the storage for a partition.</p><p>(Refer to <a href="https://googleapis.dev/python/bigquery/latest/generated/google.cloud.bigquery.table.TimePartitioning.html#google.cloud.bigquery.table.TimePartitioning">BigQuery documentation</a> for more information.)</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <p><strong>bq_table_timepartitioning_require_partition_filter</strong></p><p>type: boolean</p><p>optional</p> | <p>If set to true, queries over the partitioned table require a partition filter that can be used for partition elimination to be specified.</p><p>(Refer to <a href="https://googleapis.dev/python/bigquery/latest/generated/google.cloud.bigquery.table.Table.html#google.cloud.bigquery.table.Table.require_partition_filter">BigQuery documentation</a> for more information.)</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

### Data types

Tailer Platform supports the following data types.

#### Numeric types

| Name      | Description                                                                                                                                                                                                                                                                                                    |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `int64`   | <p>Integers are numeric values that do not have fractional components.</p><p>They range from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807.</p>                                                                                                                                                      |
| `float64` | Floating point values are approximate numeric values with fractional components.                                                                                                                                                                                                                               |
| `numeric` | <p>This data type represents decimal values with 38 decimal digits of precision and 9 decimal digits of scale. (Precision is the number of digits that the number contains. Scale is how many of these digits appear after the decimal point.)</p><p>It is particularly useful for financial calculations.</p> |

#### Boolean type

| Name      | Description                                                                                                                                                      |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `boolean` | This data type supports the `true`, `false`, and `null` values. It can perform some basic conversions, such as `'true'`, `'True'`, `True`, or 1 becoming `true`. |

#### String type

| Name     | Description                                                                                                                                                                                 |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `string` | <p>Variable-length character data.</p><p>When converting data from string to a different data type, makes sure to use <code>safe_cast</code> when you're unsure about the data quality.</p> |

#### Bytes type

| Name    | Description                                                                                                        |
| ------- | ------------------------------------------------------------------------------------------------------------------ |
| `bytes` | Variable-length binary data. This data type is rarely used but can be useful for characters with unusual encoding. |

#### Time types

{% hint style="info" %}
Only the `date`, `datetime` and `timestamp` data types (not `time`) allow table partitioning.
{% endhint %}

{% hint style="info" %}
Time zone management being difficult with BigQuery, prefer the UTC format.
{% endhint %}

| Name        | Description                                                                                                                                                          |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `date`      | This data type represents a calendar date. It includes the year, month, and day.                                                                                     |
| `time`      | This data type represents a time, as might be displayed on a watch, independent of a specific date. It includes the hour, minute, second, and subsecond.             |
| `datetime`  | This data type represents a date and time, as they might be displayed on a calendar or clock. It includes the year, month, day, hour, minute, second, and subsecond. |
| `timestamp` | This data type represents an absolute point in time, with microsecond precision.                                                                                     |

