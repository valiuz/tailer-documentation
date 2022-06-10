---
description: >-
  Learn how to transfer a Google BigQuery table contents into Firestore
  documents.
---

# Transfer data with GBQ to Firestore

## :bulb: What is a GBQ to Firestore configuration?

A GBQ to Firestore data pipeline allows you to transform each row of a BigQuery table into Firestore documents.

## ✅ **Overall operations needed**

****:warning: GBQ to Firestore is an advanced data pipeline which requires prior knowledge of:

* [Tables to storage configuration](../export-data-with-tables-to-storage/table-to-storage-configuration-file.md)
* [VM-launcher configuration](../execute-code-processings-with-vm-launcher/process-code-with-vm-launcher/vm-launcher-code-processing-configuration-file.md)
* [Workflow configuration](../orchestrate-processings-with-workflow/workflow-configuration-file.md)
* Python [(Data structures - Dictionaries)](https://docs.python.org/3/tutorial/datastructures.html?highlight=dictionary#dictionaries)
* [Firestore](https://firebase.google.com/docs/firestore)

## ⚙️ How it works

1. A tables-to-storage data operation is scheduled, or triggered by an event (for example a tables-to-tables successful run)
2. The SQL query you specified in your tables-to-storage configuration is executed to extract the relevant data from BigQuery into a JSON file located in the Google Cloud Storage bucket of your choice
3. Then a vm-launcher data operation is triggered to launch a VM on Google Compute Engine to execute the Python script that loads the JSON file into Firestore
4. Once the execution is complete, the VM is stopped automatically
5. Lastly, you can check the file tree in Firestore and see your data!

## **📋 How to deploy a GBQ to Firestore data pipeline:**

You must follow these steps:

1. ****[**Deploy a tables to storage data operation**](https://docs.tailer.ai/data-pipeline-operations/export-data-with-tables-to-storage#how-to-deploy-a-table-to-storage-data-operation)****
2. ****[**Deploy a vm-launcher data operation for code processing**](https://docs.tailer.ai/data-pipeline-operations/execute-code-processings-with-vm-launcher/process-code-with-vm-launcher#how-to-deploy-a-vm-launcher-data-operation-for-code-processing)****
3. ****[**Deploy a workflow data operation**](../orchestrate-processings-with-workflow/workflow-configuration-file.md)****

The following pages describe how to deploy a first end-to-end GBQ to Firestore data pipeline.
