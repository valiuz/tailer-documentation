---
description: >-
  Learn how to transfer files across storages using the Storage to Storage
  operation.
---

# Move files with Storage to Storage

## :bulb: What is Storage to Storage?

A Storage to Storage (STS) data pipeline operation allows you to retrieve files from one source storage, and to run a multiple copy job to one or several destination storages.

## ✅ Supported source and destination storage types

* Google Cloud Storage bucket
* Amazon S3 bucket
* SFTP directory

## ⚙️ How it works

Every time a new file matching the specified rule appears in the source directory, it will be:

* removed from the source directory,
* if options have been set accordingly, copied to an archive directory located in the same storage, inside a folder named as the filename date,
* and transferred to one or more output directories (destinations) located in different storages.

## **📋 How to deploy a Storage to Storage data operation**

1. Access your **tailer** folder (created during [installation](../../getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want, and create a JSON file for your data operation inside.
3. Prepare your JSON configuration file. Refer to this page to learn about all the [parameters](storage-to-storage-configuration-file.md).
4.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
5.  To deploy the data operation, run the following command:

    ```
    tailer deploy configuration your-file.json
    ```
6. Log in to [Tailer Studio](http://studio.tailer.ai) to check the status and details of your data operation.
7. Add a file with the right template in the source folder
8. Access your output folder(s), and archive folder, if any, to check the result of the data operation.
