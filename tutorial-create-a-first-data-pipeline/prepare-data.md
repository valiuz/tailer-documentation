---
description: >-
  The third data operation of this tutorial will consist in preparing data
  within BigQuery tables.
---

# Prepare data

## :map: Overview

The objective of this step will be to create new BigQuery tables into which we will load and reorganize the contents of the tables created at the previous step. As in most cases, this will happen within one BigQuery dataset. For this, we will need:

* a JSON file to configure the data operation,
* a JSON file to trigger the workflow,
* a JSON file for each table creation,
* and a SQL file for each transfer of data into our new tables.

## :page\_facing\_up: Create your files

### **Create the JSON file that configures the data operation**

1. Access your **tailer-demo** folder.
2. Inside, create a folder named **3-Prepare-data** for this new step.
3. In this folder, create a JSON file named **000099-tailer-demo-prepare-data.json** for your data operation.
4.  The data operation will load a temporary table, and then, if the query runs correctly, then we copy the temporary table into the target table. Copy the following contents into your file:

    ```json
    {
      "$schema": "http://jsonschema.tailer.ai/schema/table-to-table-veditor",
      "configuration_type": "table-to-table",
      "configuration_id": "000099-load_my_gbq_dataset_my_table",
      "short_description": "Prepare data for the Tailer demo",
      "account": "000099",
      "environment": "DEV",
      "activated": true,
      "archived": false,
      "start_date": "2023, 1, 23",
      "catchup": false,
      "schedule_interval": "None",
      "default_gcp_project_id": "my-gcp-project",
      "default_bq_dataset": "my_gbq_dataset",
      "default_write_disposition": "WRITE_TRUNCATE",
      "task_dependencies": [
        "load_temp_sales >> swap_sales_tables"
      ],
      "workflow": [
        {
          "task_type": "sql",
          "id": "load_temp_sales",
          "short_description": "Load temp sales table",
          "table_name": "sales",
          "sql_file": "load_sales.sql"
        },
        {
          "id": "swap_sales_tables",
          "task_type": "copy_gbq_table",
          "source_gcp_project_id": "my-project",
          "source_bq_dataset": "temp",
          "source_bq_table": "sales",
          "destination_bq_table": "sales"
        }
      ]
    }
    ```
5. Edit the following values:\
   ◾ Replace **my-gcp-project-id** with the ID of the GCP project containing your BigQuery dataset.\
   ◾ Replace **my-gbq-dataset** with the name of your working dataset.\
   ◾ Also replace the project and dataset in the configuration\_id

### Create a SQL file&#x20;

Create a SQL file in the same directory and name it load\_sales.sql. It must contain a query that will load the sales table.

### **Create the JSON file that triggers the workflow**

Inside the **3-Prepare-data** folder, create a file named **workflow.json**.

Copy the following contents into your file:

```json
{
    "$schema": "http://jsonschema.tailer.ai/schema/workflow-veditor",
    "configuration_type": "workflow",
    "version":"2",
    "configuration_id": "000099-trigger_load_my_gbq_dataset_my_table_DEV",
    "short_description": "Launch the Tailer demo data load",
    "account": "000099",
    "environment": "PROD",
    "activated": true,
    "archived": false,
    "gcp_project_id": "my-project",
    "authorized_job_ids": [
      "storage-to-tables|000099|000099-tailer-demo-load-files-YOUR-NAME|DEV|sales_{{FD_BLOB_8}}-{{FD_DATE}}.csv"
    ],
    "target_dag": {
        "configuration_type":"table-to-table",
        "configuration_id":"000099-load_my_gbq_dataset_my_table_DEV"
    }
}
```

This worklfow will trigger our Table-to-Table that loads the sales table each time a sales file is ingested with our Storage-to-Table operation.&#x20;

The authorized\_job\_ids defines the job that triggers the target job. We need to insert here the job\_id of the Storage-to-Table operation. You can find it in on Tailer Studio. Navigate to the Storage-to-Table runs section, find the last run for a sales file and go to the Run Details tab. Search for a job\_id and copy it into the authorized\_job\_ids section.

Replace the configuration\_id in the target\_dag section by your configuration's configuration\_id, concatenated with "\_DEV" (for its environment should be DEV).

## :arrow\_forward: Deploy the data operation

Once your files are ready, you can deploy the data operation:

1.  Access your working folder by running the following command:

    ```
    cd "[path to your tailer folder]\tailer-demo\3-Prepare-data"
    ```
2.  To deploy the data operation, run the following command:\\

    ```
    tailer deploy configuration 000099-tailer-demo-prepare-data.json
    ```
3.  To trigger the workflow, run the following command:

    ```
    tailer deploy configuration workflow.json
    ```

You may be asked to select a context (see [this page](../data-pipeline-operations/set-constants-with-context/context-configuration-file.md) for more information). If you haven't deployed any context, then choose "no context". You can also use the flag --context to specify the context of your choice, or NO\_CONTEXT if that's what you want:

```
tailer deploy configuration 000099-tailer-demo-prepare-data.json --context NO_CONTEXT
tailer deploy configuration workflow.json --context NO_CONTEXT
```

{% hint style="success" %}
Your data operation is now deployed, which means the new tables will shortly be created and loaded with data, and your data operation status is now visible in Tailer Studio.
{% endhint %}

## :white\_check\_mark: Check the data operation status in Tailer Studio

1. Access [Tailer Studio](http://studio.tailer.ai) again.‌
2. In the left navigation menu, select **Tables-to-tables**.
3. In the **Configurations** tab, search for your data operation. You can see its status is **Activated**. You can see in the top right corner that it as been deployed by you a few seconds ago.
4. Check for your workflow configuration. It should also be activated and deployed by you a few seconds ago.
5. Try to copy a sales file in your Storage-to-Tables source bucket. It should automatically trigger a Storage-to-Tables run. If it's successful, then it should trigger the workflow and create a Table-to-Table run.
6. When it's completed and successful, you can check on BigQuery and see that your target table  is loaded.
