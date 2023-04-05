---
description: This is the guide page to Fashion Data Expectation stored procedures.
---

# Asserting Data quality with Expectations

## :map: A short introduction to “Expectations”

What are expectations? A set of SQL Stored Procedures that ease the writing of tests. Too often, writing tests in SQL requires assertions that are complex to write, that are not factorized. In a complex project, those assertions are spread amongst many projects and scripts and this can be a tedious tasks to maintain them. Fashion Data Expectations are a way to solve these problems:

* a set of stored procedures that manage a large number of common assertions (check for primary key, check for integrity constraints, regular expression, etc.)
* one liners that are still part of the SQL ecosystem, so they live with your SQL code and your Tailer configurations and can benefit from classic SQL syntax.
* fast execution with parallel processing.
* full assertion metrics including number of rejected lines, assertion processing time, timestamping, metrics history, etc.

Let’s say that you imported data into a BigQuery project and you want to check for a primary key constraint with a certain threshold. In SQL you would write something like that:

```sql
ASSERT ((
    (select count(distinct PK_products) from `dlk-demo.dlk_demo_pda.products`) 
    - (select count(*) from `dlk-demo.dlk_demo_pda.products`) 
    ) = 0
) as "pk issue with table dlk-demo.dlk_demo_pda.products";
```

With Fashion Data Expectations, you just write:

```sql
CALL `tailer-ai.expect.primarykey_named`('dlk-demo.dlk_demo_pda', 'products', 'PK_products', 0);                     
```

## :airplane\_departure: Getting started

### Launching Expectation in the BigQuery console

You can launch an expectation directly from your BigQuery console and check your test. If the call is properly formed, BigQuery will launch the jobs described in the procedure and you will see the test status and the related metrics in the result of the last job. This eases the developpement of a set of expectations and can also be useful for ensuring adhoc quality of an element.

```sql
-- Expectations have usually the following format
-- CALL `tailer-ai.expect.EXPECTATION`('PROJECT_ID.DATASET_ID', 'TABLE_ID', SOME_PARAMETERS); 
CALL `tailer-ai.expect.table_count_greater`('dlk-demo.dlk_demo_pda', 'products', 10000, 0); 
```

{% hint style="warning" %}
In your expectation call, always specify the name of the project, otherwise the expectation will search for your table in “tailer-ai” (and will fail) instead of wherever your data is.
{% endhint %}

{% hint style="danger" %}
**You need to request access** to the tailer-ai project **from your Tailer Platform administrator** before beeing able to call these expectations.&#x20;
{% endhint %}

### :gear: Creating an Expectation in a Tailer Table to Table configuration

To create an expectation, you need two elements:&#x20;

* a dedicated task in a table-to-table configuration
* a dedicated SQL file

The dedicated task must be of type “expectation”. For example:

```json
{
    "id": "expects_tables",
    "task_type": "expectation",
    "short_description": "Check for data integrity (pk, count, dates,...).",
    "doc_md": "000001_load_PDA_products.md",
    "sql_file": "000001_load_PDA_products_expects_r7.sql",
    "criticality": "warning"
}
```

In your SQL file, you can add as much expectations as you want:

```sql
-- assert count greater than 0 
CALL `tailer-ai.expect.table_count_greater`('dlk-demo.dlk_demo_pda', 'products', 100000, 0); 
-- assert primary key is ok 
CALL `tailer-ai.expect.primarykey`('dlk-demo.dlk_demo_pda', 'products', 0); 
-- assert freshness on the final table (we want to have at least 10k products for today iteration) 
CALL `tailer-ai.expect.values_to_contain`('dlk-demo.dlk_demo_pda', 'products', 'max_importdate', cast(current_date() as string), 10000, 0); 
-- assert freshness on the psa table (we want to have at least 10k product for today psa)
 CALL `tailer-ai.expect.table_count_greater`('dlk-demo.dlk_demo_psa', concat('products_', replace(cast(current_date() as string), '-', '')), 100000, 0); 
 -- assert freshness on the psa table for yesterday(we want to have at least 10k product for yesterday psa) 
 CALL `tailer-ai.expect.table_count_greater`('dlk-demo.dlk_demo_psa', concat('products_', replace(cast(date_sub(current_date(), interval 1 day) as string), '-', '')), 100000, 0);
```

Your call to a stored procedure will be treated as a SQL instruction. This allows writing great expectations with powerful features. For example, doing “CONCAT” or using “DATE\_SUB” or “CURRENT\_DATE” enable counting with a sliding window on a specific table:

```sql
-- Check line count for the table products_YYYYMMDD where YYYYMMDD is yesterday's date
CALL `tailer-ai.expect.table_count_greater`(
     'dlk-demo.dlk_demo_psa', 
     concat('products_', replace(cast(date_sub(current_date(), interval 1 day) as string), '-', '')),
     100000, 
     0
 );
```

{% hint style="info" %}
In your SQL expectation file, only expectations will be executed. Classic SQL commands or comments will be ignored.
{% endhint %}

## 💡 Analyzing raw metrics

Everytime an expectation embedded in a table-to-table data operation is executed, it generates some metrics that are added to the tailer\_common.expectation\_results table in your project. For example:

```sql
SELECT * FROM `dlk-demo.tailer_common.expectation_results` LIMIT 1000 
```

Here are the fields of this:

| Field Name          | Type   | Description                             |
| ------------------- | ------ | --------------------------------------- |
| job\_id             | STRING | Identifier of the Job                   |
| dag\_id             | STRING | Identifier of the Direct Acyclic Graph  |
| account             | STRING | Account Name                            |
| environment         | STRING | Execution Environnement (DEV, PROD,...) |
| run\_id             | STRING | Identifier of the Execution             |
| configuration\_type | STRING | Configuration Type                      |
| configuration\_id   | STRING | Identifier of the Configuration         |
| task\_id            | STRING | Identifier of the Task                  |
| execution\_date     | STRING | Execution Date                          |
| criticality         | STRING | Task Criticality                        |
| expectation\_result | STRING | Expectation Result                      |

The field called “expectation\_result” contains additionnal informations about the expectation in JSON format. The generic output is defined here, and some specific results could be added for some expectations (see details in the [list of expectations](list-of-expectations.md)):

```json
{
    "dataset": "dlk-demo.dlk_demo_pda",
    "tablename": "products",
    "column_name": "",
    "procedure_name": "table_count_greater",
    "date_count": "2021-12-03T14:02:51.284016",
    "all_count": 158357,
    "target_dataset": "",
    "target_table_name": "",
    "target_value": [
        "100000"
    ],
    "reject_count": 58357,
    "reject_threshold": 0.01,
    "passed": false,
    "start_date": "1642411639", 
    "end_date": "1642411644"
}
```

{% hint style="info" %}
The metrics of a test launched from the BigQuery console won't be inserted into the metrics table. Only the results of the tests embedded into a table-to-table configuration will be stored.
{% endhint %}

## :desktop: Tailer Studio integration

You can find an Expectations Overview in Tailer Studio.&#x20;

Click in the left pannel on "Expectations Overview" in the "Data Quality" section and see all the expectations that has been recently tested.&#x20;

![](<../../.gitbook/assets/image (1) (3).png>)

## **📋** A list of available Expectations

A complete documentation of all avaibable expectations and their specific parameters is available on the next page: [List of Expectations](list-of-expectations.md).

## 🌍 Multiple regions

{% hint style="info" %}
If you need expectations in a specific BigQuery location where they are not already available, please feel free to contact us!
{% endhint %}

The first set of expectations is located in region EU. So this call will work if you apply it to a BigQuery table located in EU:

```sql
CALL `tailer-ai.expect.primarykey`('my_project.my_dataset_EU', 'products', 'PK_products', 0);                     
```

We've also released expectations on a few other region, each in a specific dataset. For example, you can use primarykey on a table in europe-west1 region using excpect\_euw1:

```sql
CALL `tailer-ai.expect_euw1.primarykey`('my_project.my_dataset_europe_west1', 'products', 'PK_products', 0);                     
```
