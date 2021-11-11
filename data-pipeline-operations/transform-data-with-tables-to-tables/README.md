---
description: >-
  Learn how to extract, transform and load Google BigQuery data using the Tables
  to Tables operation.
---

# Transform data with Tables to Tables

## :bulb: What is Tables to Tables?

A Tables to Tables (TTT) data pipeline operation allows you to automate the execution of one or several BigQuery tasks in order to extract, transform and load data from tables to other tables.

{% hint style="warning" %}
You cannot JOIN tables from different GCP projects.
{% endhint %}

{% hint style="info" %}
A table can be the source and the destination of the same task.
{% endhint %}

## ✅ Supported databases

* Google BigQuery (source and target)

## ⚙️ How it works

When a Tables to Tables Tailer data operation is triggered by an event (for example a Storage to Tables data operation success) or scheduled to start:

* A number of workflow tasks (SQL queries and JSON table creation/copy tasks) are run in the order set in the **task\_dependencies** parameter of the data operation configuration file.
* You obtain one or several BigQuery tables containing the reorganized data.

**📋 How to deploy a Tables to Tables data operation**

1. Access your **tailer** folder (created during [installation](../../getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want.
3. Create SQL and JSON files corresponding to your [workflow tasks](table-to-table-sql-and-ddl-files.md).
4. Prepare your JSON configuration file to gather all this information. Refer to this page to learn about all its [parameters](tables-to-tables-configuration-file.md).
5. Determine how to launch your Tables to Tables data operation: either use the **schedule\_interval** parameter in the JSON configuration file, and/or create a [Workflow configuration file](../orchestrate-processings-with-workflow/workflow-configuration-file.md) that will define how to trigger it.
6.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
7.  To deploy the data operation, run the following command:

    ```
    tailer deploy your-file.json
    ```
8. Log in to [Tailer Studio](http://studio.tailer.ai) to check the status and details of your data operation.
9. For your workflow to be executed, you either need to run the data operation corresponding to the previous step of your data pipeline (per your Workflow configuration file), or to launch it manually from [Tailer Studio](http://studio.tailer.ai).
10. Access your output table(s) in BigQuery to check the result of the data operation.
