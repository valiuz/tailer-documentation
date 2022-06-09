---
description: >-
  This is the description of the SQL file used for a Table to Storage data
  operation.
---

# Table to Storage SQL file

To run a Table to Storage data operation, you first need to prepare a SQL query that will extract the data to export.

The SQL file must contains a BigQuery Standard SQL query. You can write it directly in the query editor of [BigQuery](https://console.cloud.google.com/bigquery) and then save it to a .sql file.

This query will be executed when the data operation is deployed, and the result will display in the JSON export file.

## :eye\_in\_speech\_bubble: Example for a gbq to firestore transfer :

```
WITH
tmp AS (
    SELECT
        CURRENT_TIMESTAMP() AS timestamp,
        "tailer-activities-runs" AS tailer_activities_runs,
        "freshness" AS freshness,
        "next_execution" AS next_execution,
        account,
        configuration_type,
        configuration_id,
        job_id,
        last_execution_datetime,
        next_execution_datetime,
        frequence,
        status
    FROM
        `fd-jarvis-datalake.dlk_tailer_bda_freshness.metrics`
    WHERE
        (1 = 1)
    GROUP BY
        account,
        configuration_type,
        configuration_id,
        timestamp,
        job_id,
        last_execution_datetime,
        next_execution_datetime,
        frequence,
        status
)

SELECT
    timestamp,
    CONCAT(tailer_activities_runs, 
            "|", account, 
            "|", configuration_type, 
            "|", configuration_id, 
            "|", freshness, 
            "|", "job_id", 
            "|", REPLACE(job_id, "|", "_"), 
            "|", next_execution) 
            AS firestore_path,
    account,
    configuration_type,
    configuration_id,
    job_id,
    CONCAT("freshness_", job_id) AS collection_groupe_name,
    last_execution_datetime,
    next_execution_datetime,
    frequence,
    status
FROM
    tmp     
```

## :globe\_with\_meridians: Global parameters

**To use the python deployment automation on Firestore, the SQL must follow a specific pattern.**

| Variables                                                                 | Descriptions                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>timestamp</strong></p><p>type: timestamp</p><p>optional</p>    | For your different use cases, it can be interesting to have the last calculation date of your dataset to Firestore.                                                                                                                                                                                                                                                                                                                                                                                                    |
| <p><strong>firestore_path</strong></p><p>type: string</p><p>mandatory</p> | <p>Variable read by the python code to define the path of documents and collections in Firestore. Each category and sub-category must be separated by a pipe "|". <br><br><strong>Caution</strong>, remember to remove the "|" in the variables that will be used as path name<br><br>The Firestore path storage is a succession of documents and collections. You must at all costs end up on a collection of documents. The defined path must therefore contain an even number of categories and sub-categories.</p> |
| <p><strong>variables</strong></p><p>type: string</p><p>optional</p>       | The other variables are the ones you want to display in your firestore document, there are no limits and can be defined with a table to create sub categories. (Example with a table below)                                                                                                                                                                                                                                                                                                                            |

![Here is an example of a display at the end of a GBQ to Firestore with this SQL file](<../../.gitbook/assets/Capture d’écran 2022-05-19 à 10.33.46.png>)

## :eye\_in\_speech\_bubble: Example 2 : create a list in the document



```
WITH
tmp AS (
    SELECT
        CURRENT_TIMESTAMP() AS timestamp,
        "tailer-activities-runs" AS tailer_activities_runs,
        "freshness" AS freshness,
        "next_execution" AS next_execution,
        account,
        configuration_type,
        configuration_id,
        job_id,
        ARRAY_AGG(STRUCT(
            job_id,
            last_execution_datetime,
            next_execution_datetime,
            frequence,
            status
        )) AS data
    FROM
        `fd-jarvis-datalake.dlk_tailer_bda_freshness.metrics`
    WHERE
        (1 = 1)
    GROUP BY
        account,
        configuration_type,
        configuration_id,
        timestamp,
        job_id,
        last_execution_datetime,
        next_execution_datetime,
        frequence,
        status
    )
SELECT
    timestamp,
    CONCAT(tailer_activities_runs,
            "|", account, 
            "|", configuration_type, 
            "|", configuration_id, 
            "|", freshness, 
            "|", "job_id", 
            "|", REPLACE(job_id, "|", "_"), 
            "|", next_execution) 
            AS firestore_path,
    account,
    configuration_type,
    configuration_id,
    job_id,
    data
FROM
    tmp

```

## :globe\_with\_meridians: Global parameters

**To use the python deployment automation on Firestore, the SQL must follow a specific pattern.**

| Variables                                                                                               | Descriptions                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>timestamp</strong></p><p>type: timestamp</p><p>optional</p>                                  | For your different use cases, it can be interesting to have the last calculation date of your dataset to Firestore.                                                                                                                                                                                                                                                                                                                                                                                                    |
| <p><strong>firestore_path</strong></p><p>type: string</p><p>mandatory</p>                               | <p>Variable read by the python code to define the path of documents and collections in Firestore. Each category and sub-category must be separated by a pipe "|". <br><br><strong>Caution</strong>, remember to remove the "|" in the variables that will be used as path name<br><br>The Firestore path storage is a succession of documents and collections. You must at all costs end up on a collection of documents. The defined path must therefore contain an even number of categories and sub-categories.</p> |
| <p><strong>variables</strong></p><p>type: string</p><p>optional</p>                                     | The other variables are the ones you want to display in your firestore document, there are no limits and can be defined with a table to create sub categories. (Example with a table below)                                                                                                                                                                                                                                                                                                                            |
| <p><strong>data</strong></p><p>type: struct of string or struct of struct of string</p><p>mandatory</p> | Allows you to create a list of sub-elements that correspond N times to the element (for example for a product, a list of sales sorted by date)                                                                                                                                                                                                                                                                                                                                                                         |

![Here is an example of a display at the end of a GBQ to Firestore with this SQL file (array version)](<../../.gitbook/assets/Capture d’écran 2022-05-19 à 14.46.42.png>)



