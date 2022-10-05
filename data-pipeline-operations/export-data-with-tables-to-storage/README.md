---
description: >-
  Learn how to export data located in a BigQuery table into CSV/JSON files using
  a Table to Storage operation.
---

# Export data with Tables to Storage

## :bulb: What is Table to Storage?

A Table to Storage (TTS) data pipeline operation allows you to export your data from a BigQuery table to a CSV/JSON file in a Google Cloud Storage bucket so you can leverage them with other tools, such as a warehouse management system.

## ✅ Supported file types

### **Databases**

* Google BigQuery

### **Export files**

* CSV file in Google Cloud Storage
* JSON file in Google Cloud Storage

{% hint style="info" %}
Please note that all BigQuery export limits apply to table-to-storage data operations. See BigQuery [documentation](https://cloud.google.com/bigquery/docs/exporting-data#export\_limitations) for more information.

In particular:

* You can export up to 1 GB of table data to a single file. If you are exporting more than 1 GB of data, the data is loaded into multiple files. When you export data to multiple files, the size of the files will vary.
* When you export data in [JSON](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types#json\_type) format, [INT64](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types#integer-type) (integer) data types are encoded as JSON strings to preserve 64-bit precision when the data is read by other systems.
* Lorsque vous exportez une table au format JSON, les symboles `<`, `>` et `&` sont convertis à l'aide de la notation Unicode `\uNNNN`, où `N` est un chiffre hexadécimal. Par exemple, `profit&loss` devient `profit\u0026loss`. Cette conversion Unicode est effectuée pour éviter les failles de sécurité.
{% endhint %}

## ⚙️ How it works

When the Table to Storage Tailer workflow is triggered by an event (usually a BigQuery table update):

* The SQL query you specify will be executed to extract the relevant data from the source BigQuery table.
* The data will be exported to a CSV file or a JSON file located in the GCS bucket you specified.

## **📋 How to deploy a Table to Storage data operation**

1. Access your **tailer** folder (created during [installation](../../getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want, and create a JSON file for your data operation inside.
3. Create a [SQL file](table-to-storage-sql-file.md) to determine what data to extract.
4. Prepare your JSON configuration file. Refer to this page to learn about all the [parameters](table-to-storage-configuration-file.md).
5. Create a [Workflow configuration file](../orchestrate-processings-with-workflow/workflow-configuration-file.md) that will define how to trigger it.
6.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
7.  To deploy the data operation, run the following command:

    ```
    tailer deploy configuration your-configuration.json
    ```
8. Log in to [Tailer Studio](http://studio.tailer.ai) to check the status and details of your data operation.
9. For your workflow to be executed, you either need to run the data operation that is set to trigger it in your Workflow data operation (previous step in your data pipeline), or to launch it manually from [Tailer Studio](http://studio.tailer.ai).
10. Access the GCS bucket to check your output file (CSV or JSON).
