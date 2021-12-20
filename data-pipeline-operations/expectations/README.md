# Guide

This is the guide page to Fashion Data Expectation stored procedures.

# A Short Introduction to “Expectations”

What are expectations ? A set of simple SQL Stored Procedures that ease the writing of tests.
Too often, writing tests in sql requires assertions that are complex to write, that are not
factorized. In a complex project, those assertions are spread amongst many
projects and scripts and this can be a tedious tasks to maintain them. Fashion Data Expectations
are a way to solve these problems :
- a set of simple stored procedures that manage a large number of common assertions (check for
primary key, check for integrity constraints, regular expression, etc.)
- one liners that are still part of the SQL ecosystem, so they live with your SQL Code and
your Tailer configurations and can benefit from classic sql syntax.
- fast execution with parallel processing.
- full assertion metrics including number of rejected lines, assertion processing time,
timestamping, metrics history, etc.

Let’s say that you imported data into a bigquery project and you want to check for a primary key constraint
with a certain threshold. In SQL you would write something like that :

`\`
assert ( ((select count(distinct PK_products) from  \`fd-io-dlk-demo.dlk_demo_pda.products\`) / (select count(\*) from  \`fd-io-dlk-demo.dlk_demo_pda.products\`) ) = 0)  as "pk issue with table fd-io-dlk-demo.dlk_demo_pda.products";
\``

With FD Expectations, you just write :



```
``
```

\`
CALL 

```
`
```

tailer-ai.expect.primarykey\`(‘fd-io-dlk-demo.dlk_demo_pda’, ‘products_r7’);



```
``
```



```
`
```


# Getting Started

## Launching Expectation in the Bigquery Console

You can launch an expectation directly from your bigquery console. This eases the developpement of a set of expectations
and can also be useful for ensuring adhoc quality of an element.

`\`
--- Expectations have usually the following format
--- CALL \`tailer-ai.expect.EXPECTATION\`('PROJECT_ID.DATASET_ID', 'TABLE_ID', SOME_PARAMETERS);
CALL \`tailer-ai.expect.table_count_greater\`('fd-io-dlk-demo.dlk_demo_pda', 'products_r7', 100000,0);
\``

*Important Note* : In your expectation call, always specify the name of the project, otherwise the expectation
will search for your table in “tailer-ai” (and will fail) instead of wherever your data are.

## Creating an Expectation in a Tailer Table to Table Configuration

To create an expectation, you need two elements :
\* a dedicated configuration task
\* a dedicated sql file

The dedicated configuration task must be of type “expectation”. For example :



```
``
```



```
`
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
``
```



```
`
```


In your sql file, you can add as much expectations as you want :

`\`
-- assert count greater than 0
CALL \`tailer-ai.expect.table_count_greater\`('fd-io-dlk-demo.dlk_demo_pda', 'products_r7', 100000,0);
-- assert primary key is ok
CALL \`tailer-ai.expect.primarykey\`('fd-io-dlk-demo.dlk_demo_pda', 'products_r7');
-- assert freshness on the final table (we want to have at least 10k products for today iteration)
CALL \`tailer-ai.expect.values_to_contain\`('fd-io-dlk-demo.dlk_demo_pda', 'products_r7', 'max_importdate' ,cast(current_date() as string),10000,0);
-- assert freshness on the psa table (we want to have at least 10k product for today psa)
CALL \`tailer-ai.expect.table_count_greater\`('fd-io-dlk-demo.dlk_demo_psa', concat('products_', replace(cast(current_date() as string), '-','')),100000,0);
-- assert freshness on the psa table for yesterday(we want to have at least 10k product for yesterday psa)
CALL \`tailer-ai.expect.table_count_greater\`('fd-io-dlk-demo.dlk_demo_psa', concat('products_', replace(cast(date_sub(current_date(), interval 1 day) as string), '-','')),100000,0);
\``

*Important Note 1* : In your sql expectation file, only expectations will be executed. Classic sql commands or comments will be ignored.

*Important Note 2* : Your call to a stored procedure will be treated as a sql instruction. This allows writing
great expectations with powerful features. For example, doing “CONCAT” or using “DATE_SUB” or “CURRENT_DATE”
enable counting with a sliding window on a specific table.

`\`
CALL \`tailer-ai.expect.table_count_greater\`('fd-io-dlk-demo.dlk_demo_psa', concat('products_', replace(cast(date_sub(current_date(), interval 1 day) as string), '-','')),100000,0);
\``

# Analyzing raw metrics

Everytime an expectation is executed, it generates some metrics that are added to the follwing table :

`\`
SELECT \* FROM \`fd-io-dlk-demo.tailer_common.expectation_results\` LIMIT 1000
\``

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


The field called “expectation_result” contains additionnal and specific informations about the expectation in JSON format:



```
``
```

\`
{

> “dataset”: “fd-io-dlk-demo.dlk_demo_pda”,
> “tablename”: “products_r7”,
> “column_name”: “”,
> “procedure_name”: “expect_table_count_greater”,
> “date_count”: “2021-12-03T14:02:51.284016”,
> “all_count”: 158357,
> “target_dataset”: “”,
> “target_table_name”: “”,
> “target_value”: [

> > “100000”

> ],
> “reject_count”: 58357,
> “reject_threshold”: 0,
> “passed”: true

A complete description of the specifics field is available on each expectation documentation.

# Tailer Studio Integration

To be announced.

Within the TTT Run result and configuration :
Expectation will be displayed as a specific task in your task list of your configuration.

In a dedicated screen for expectation :
Screenshot

# A Quick List of available Expectations

A complete documentation is available on [https://documentation.tailer.ai](https://documentation.tailer.ai). But here’s a quick list
of all the available stored procedures :


* expect_column_values_to_be_between


* expect_column_values_to_be_in_set


* expect_column_values_to_contain


* expect_column_values_to_not_be_in_set


* expect_everyday_increasing_since


* expect_everyday_since


* expect_everymonth_since


* expect_everyweek_since


* expect_foreignkey


* expect_not_null


* expect_null


* expect_primarykey


* expect_primarykey_named


* expect_primarykey_threshold


* expect_table_row_count_to_be_between


* expect_table_row_count_to_be_equal


* expect_table_row_count_to_be_equal_other_table


* expect_table_row_count_to_be_greater


* expect_type


* expect_unique
