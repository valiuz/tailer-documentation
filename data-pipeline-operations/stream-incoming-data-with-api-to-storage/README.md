---
description: >-
  Learn how to easily setup an incoming data streaming endpoint with API To
  Storage data operation.
---

# Stream incoming data with API To Storage



## **📋 How to deploy an API To Storage data operation**

1. Access your **tailer** folder (created during [installation](../../getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want, and create a JSON file for your data operation inside.
3. Prepare your JSON configuration file. Refer to this page to learn about all the [parameters](../load-data-with-storage-to-tables/storage-to-tables-configuration-file.md).
4.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
5.  To deploy the data operation, run the following command:

    ```
    tailer deploy your-file.json
    ```
6. Log in to [Tailer Studio](https://studio.tailer.ai) to check the status and details of your data operation.
7. Access your output table(s), and archive folder, if any, to check the result of the data operation.
