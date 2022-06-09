---
description: >-
  Learn how to transfer a google big query table to Firestore documents
  organisation.
---

# Transfer data with GBQ to Firestore

## :bulb: What is a gbq to Firestore configuration?

A gbq to firestore  data pipeline operation allows you to transform a dataset into Firestore documents for each row of the table.

## ✅ **Overall operations needed**

**Caution : gbq-to-firestore is an advanced configuration which requires prior knowledge :**

* [Tables to storage configuration](../export-data-with-tables-to-storage/table-to-storage-configuration-file.md)
* [Vm-launcher configuration](../execute-code-processings-with-vm-launcher/process-code-with-vm-launcher/vm-launcher-code-processing-configuration-file.md)
* [Workflow configuration](../orchestrate-processings-with-workflow/workflow-configuration-file.md)
* Knowledge of python [(Data structures - Dictionaries)](https://docs.python.org/3/tutorial/datastructures.html?highlight=dictionary#dictionaries)
* Knowledge of how [Firestore works](https://firebase.google.com/docs/firestore)

## ⚙️ How it works

**When the Tables to Storage Tailer data operation is triggered** by an event (for example a Tables to Tables data operation success) or scheduled to start :

* The SQL query you specify will be executed to extract the relevant data from the source BigQuery table.
* The data will be exported to a JSON file located in the GCS bucket you specified.
* A VM with the specified characteristics is triggered on GCE.
* The instructions set in the JSON configuration file script\_to\_execute parameter are executed, and the script is launched.
* Once the execution is complete, the VM is stopped automatically
* Check the file tree in Firestore !

## **📋 How to deploy a gbq-to-firestore data operation for code processing :**

You must follow these steps :

1. ****[**How to deploy a tables to storage data operation**](https://docs.tailer.ai/data-pipeline-operations/export-data-with-tables-to-storage#how-to-deploy-a-table-to-storage-data-operation)****
2. ****[**How to deploy a vm-launcher data operation for code processing**](https://docs.tailer.ai/data-pipeline-operations/execute-code-processings-with-vm-launcher/process-code-with-vm-launcher#how-to-deploy-a-vm-launcher-data-operation-for-code-processing)****
3. ****[**How to deploy a workflow data operation**](https://docs.tailer.ai/data-pipeline-operations/execute-code-processings-with-vm-launcher/process-code-with-vm-launcher#how-to-deploy-a-vm-launcher-data-operation-for-code-processing)****
