---
description: >-
  Learn how to transfer data from files to database tables using the Storage to
  Tables operation.
---

# Load data with Storage to Tables

## :bulb: What is Storage to Tables?

A Storage to Tables (STT) data pipeline operation allows you to load data files from a Google Cloud Storage (GCS) bucket into one or several BigQuery databases.

{% hint style="warning" %}
Note that the uniqueness of the configuration is checked against the GCS bucket name AND directory combination. This means that you can have only **one configuration per bucket/directory combination**, as any new configuration will overwrite the previous one.
{% endhint %}

## ✅ Supported file types

### **Source data files**

* CSV and any delimited flat files
* New line delimited JSON files
* These two file types can be compressed using gzip

### **Databases**

* Google BigQuery

## ⚙️ How it works

Every time a new file matching the specified rule appears in a given directory of a Google Cloud Storage bucket:

* it will be removed from the source directory,
* if options have been set accordingly, the file will be copied to an archive directory located in the same storage, inside a folder named with the date contained in the filename,
* the file data will be loaded into a BigQuery table matching its filename template for each database specified.

## **📋 How to deploy a Storage to Tables data operation**

1. Access your **tailer** folder (created during [installation](../../getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want, and create a JSON file for your data operation inside.
3. Prepare your JSON configuration file. Refer to this page to learn about all the [parameters](storage-to-tables-configuration-file.md).
4. Prepare a DDL file for each database table. Refer to this page to learn about all the [parameters](storage-to-tables-ddl-files.md).
5.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
6.  To deploy the data operation, run the following command:

    ```
    tailer deploy your-file.json
    ```
7. Log in to [Tailer Studio](https://studio.tailer.ai) to check the status and details of your data operation.
8. Access your output table(s), and archive folder, if any, to check the result of the data operation.
