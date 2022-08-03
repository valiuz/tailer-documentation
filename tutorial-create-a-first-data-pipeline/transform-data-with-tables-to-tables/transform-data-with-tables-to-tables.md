---
description: >-
  Learn how to extract, transform and load Google BigQuery data using the Tables
  to Tables operation.
---

# Launch configuration and furthermore

## ⚙️ How it works

When a Tables to Tables Tailer data operation is triggered by an event (for example a Storage to Tables data operation success) or scheduled to start:

* A number of workflow tasks (SQL queries and JSON table creation/copy tasks) are run in the order set in the **task\_dependencies** parameter of the data operation configuration file.
* You obtain one or several BigQuery tables containing the reorganized data.

**📋 How to deploy a Tables to Tables data operation**

1. Access your **tailer** folder (created during [installation](../../getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want.
3. Create SQL and JSON files corresponding to your [workflow tasks](../../data-pipeline-operations/transform-data-with-tables-to-tables/table-to-table-sql-and-ddl-files.md).
4. Prepare your JSON configuration file to gather all this information. Refer to this page to learn about all its [parameters](../../data-pipeline-operations/transform-data-with-tables-to-tables/tables-to-tables-configuration-file.md).
5. Determine how to launch your Tables to Tables data operation: either use the **schedule\_interval** parameter in the JSON configuration file, and/or create a [Workflow configuration file](../../data-pipeline-operations/orchestrate-processings-with-workflow/workflow-configuration-file.md) that will define how to trigger it.
6.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
7.  To deploy the data operation, run the following command:

    ```
    tailer deploy your-file.json
    ```
8. Log in to [Tailer Studio](http://studio.tailer.ai) to check the status and details of your data operation.
9. For your workflow to be executed, you either need to run the data operation corresponding to the previous step of your data pipeline (per your Workflow configuration file), or to launch it manually from [Tailer Studio](http://studio.tailer.ai).
10. Access your output table(s) in BigQuery to check the result of the data operation.

## 💡Modify scripts for other use cases

The subject taken with the Iowa dataset aggregates all sales from a year at each iteration to account for new sales by the week. Here are some examples to better address a need or use case:

* Proceed to a daily aggregation with a daily CRON trigger after receiving the sales file (applicable by the week, month or quarter)
* Modify the script to modify only the current week and avoid calculating old dates
* Link the output data table to a datastudio visualization
