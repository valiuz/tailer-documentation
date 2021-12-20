---
description: This is the guide page to Fashion Data Expectation stored procedures.
---

# Asserting Data quality with Expectations

## :map: A Short Introduction to “Expectations”

What are expectations ? A set of SQL Stored Procedures that ease the writing of tests. Too often, writing tests in SQL requires assertions that are complex to write, that are not factorized. In a complex project, those assertions are spread amongst many projects and scripts and this can be a tedious tasks to maintain them. Fashion Data Expectations are a way to solve these problems:

* a set of stored procedures that manage a large number of common assertions (check for primary key, check for integrity constraints, regular expression, etc.)
* one liners that are still part of the SQL ecosystem, so they live with your SQL code and your Tailer configurations and can benefit from classic SQL syntax.
* fast execution with parallel processing.
* full assertion metrics including number of rejected lines, assertion processing time, timestamping, metrics history, etc.

Let’s say that you imported data into a BigQuery project and you want to check for a primary key constraint with a certain threshold. In SQL you would write something like that:

```sql
ASSERT ((
    (select count(distinct PK_products) from `fd-io-dlk-demo.dlk_demo_pda.products`) 
    - (select count(*) from `fd-io-dlk-demo.dlk_demo_pda.products`) 
    ) = 0
) as "pk issue with table fd-io-dlk-demo.dlk_demo_pda.products";
```

With FD Expectations, you just write:

```sql
CALL `tailer-ai.expect.primarykey_named`('fd-io-dlk-demo.dlk_demo_pda', 'products', 'PK_products');
```

## :airplane\_departure: Getting Started

### Launching Expectation in the BigQuery Console

You can launch an expectation directly from your BigQuery console and check the results. This eases the developpement of a set of expectations and can also be useful for ensuring adhoc quality of an element.

```sql
-- Explectations have usually the following format
-- CALL `tailer-ai.expect.EXPECTATION`('PROJECT_ID.DATASET_ID', 'TABLE_ID', SOME_PARAMETERS); 
CALL `tailer-ai.expect.table_count_greater`('fd-io-dlk-demo.dlk_demo_pda', 'products', 10000, 0); 
```

{% hint style="info" %}
Please note that the metrics of a test launched from the BigQuery console won't be inserted into the metrics table. Only the results of the tests embedded into a table-to-table configuration will be stored.
{% endhint %}

{% hint style="warning" %}
In your expectation call, always specify the name of the project, otherwise the expectation will search for your table in “tailer-ai” (and will fail) instead of wherever your data are.
{% endhint %}

### :gear: Creating an Expectation in a Tailer Table to Table Configuration

To create an expectation, you need two elements: \* a dedicated configuration task \* a dedicated SQL file

The dedicated configuration task must be of type “expectation”. For example:

```
``
```

```
`
```

```
> > {

> “id”: “expects_tables”,
> “task_type”: “expectation”,
> “short_description”: “Check for data integrity (pk, count, dates,…).”,
> “doc_md”: “000001_load_PDA_products.md”,
> “sql_file”: “000001_load_PDA_products_expects_r7.sql”,
> “task_criticality”: “warning”

}
```

```
``
```

```
`
```

In your SQL file, you can add as much expectations as you want:

`\` -- assert count greater than 0 CALL \`tailer-ai.expect.table\_count\_greater\`('fd-io-dlk-demo.dlk\_demo\_pda', 'products\_r7', 100000,0); -- assert primary key is ok CALL \`tailer-ai.expect.primarykey\`('fd-io-dlk-demo.dlk\_demo\_pda', 'products\_r7'); -- assert freshness on the final table (we want to have at least 10k products for today iteration) CALL \`tailer-ai.expect.values\_to\_contain\`('fd-io-dlk-demo.dlk\_demo\_pda', 'products\_r7', 'max\_importdate' ,cast(current\_date() as string),10000,0); -- assert freshness on the psa table (we want to have at least 10k product for today psa) CALL \`tailer-ai.expect.table\_count\_greater\`('fd-io-dlk-demo.dlk\_demo\_psa', concat('products\_', replace(cast(current\_date() as string), '-','')),100000,0); -- assert freshness on the psa table for yesterday(we want to have at least 10k product for yesterday psa) CALL \`tailer-ai.expect.table\_count\_greater\`('fd-io-dlk-demo.dlk\_demo\_psa', concat('products\_', replace(cast(date\_sub(current\_date(), interval 1 day) as string), '-','')),100000,0); \`\`

_Important Note 1_: In your SQL expectation file, only expectations will be executed. Classic SQL commands or comments will be ignored.

_Important Note 2_: Your call to a stored procedure will be treated as a SQL instruction. This allows writing great expectations with powerful features. For example, doing “CONCAT” or using “DATE\_SUB” or “CURRENT\_DATE” enable counting with a sliding window on a specific table.

`\` CALL \`tailer-ai.expect.table\_count\_greater\`('fd-io-dlk-demo.dlk\_demo\_psa', concat('products\_', replace(cast(date\_sub(current\_date(), interval 1 day) as string), '-','')),100000,0); \`\`

## Analyzing raw metrics

Everytime an expectation is executed, it generates some metrics that are added to the follwing table:

`\` SELECT \* FROM \`fd-io-dlk-demo.tailer\_common.expectation\_results\` LIMIT 1000 \`\`

Here are the fields of this:

```
|Field Name|Type|Description|
```

```
|:------------|
```

:————-

```
|:-------------|
```

```
|job_id|STRING|Identifier of the Job|
```

```
|dag_id|STRING|Identifier of the Direct Acyclic Graph|
```

```
|account|STRING|Account Name|
```

```
|environment|STRING|Execution Environnement (DEV, PROD,...)|
```

```
|run_id|STRING|Identifier of the Execution|
```

```
|configuration_type|STRING|Configuration Type|
```

```
|configuration_id|STRING|Identifier of the Configuration|
```

```
|task_id|STRING|Identifier of the Task|
```

```
|execution_date|STRING|Execution Date|
```

```
|task_criticality|STRING|Task Criticality|
```

```
|expectation_result|STRING|Expectation Result|
```

The field called “expectation\_result” contains additionnal and specific informations about the expectation in JSON format:

```
``
```

\` {

> “dataset”: “fd-io-dlk-demo.dlk\_demo\_pda”, “tablename”: “products\_r7”, “column\_name”: “”, “procedure\_name”: “expect\_table\_count\_greater”, “date\_count”: “2021-12-03T14:02:51.284016”, “all\_count”: 158357, “target\_dataset”: “”, “target\_table\_name”: “”, “target\_value”: \[

> > “100000”

> ], “reject\_count”: 58357, “reject\_threshold”: 0, “passed”: true

A complete description of the specifics field is available on each expectation documentation.

## Tailer Studio Integration

To be announced.

Within the TTT Run result and configuration: Expectation will be displayed as a specific task in your task list of your configuration.

In a dedicated screen for expectation: Screenshot

## A Quick List of available Expectations

A complete documentation is available on [https://documentation.tailer.ai](https://documentation.tailer.ai). But here’s a quick list of all the available stored procedures:

* expect\_column\_values\_to\_be\_between
* expect\_column\_values\_to\_be\_in\_set
* expect\_column\_values\_to\_contain
* expect\_column\_values\_to\_not\_be\_in\_set
* expect\_everyday\_increasing\_since
* expect\_everyday\_since
* expect\_everymonth\_since
* expect\_everyweek\_since
* expect\_foreignkey
* expect\_not\_null
* expect\_null
* expect\_primarykey
* expect\_primarykey\_named
* expect\_primarykey\_threshold
* expect\_table\_row\_count\_to\_be\_between
* expect\_table\_row\_count\_to\_be\_equal
* expect\_table\_row\_count\_to\_be\_equal\_other\_table
* expect\_table\_row\_count\_to\_be\_greater
* expect\_type
* expect\_unique
