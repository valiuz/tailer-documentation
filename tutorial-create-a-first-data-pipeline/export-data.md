---
description: >-
  The fifth and final data operation of this tutorial will consist in exporting
  our data back to a bucket.
---

# Export data

## :map: Overview

During this step, we will take our aggregated store data located one BigQuery table and export them to a Google Cloud Storage bucket CSV file so they can later be used with other tools, such as a warehouse management system.

## :dividers: Create a bucket and a folder

For the detailed procedure on how to create GCS buckets (manually or using gsutil), refer to this [page](https://cloud.google.com/storage/docs/creating-buckets).

1. Create a bucket in the project of your choice. As bucket names need to be unique globally, you can pick any name you want. Select the settings that you want.‌
2. In this bucket, create a folder named **store\_clustering\_export** that will contain our future export file.

## :page\_facing\_up: Create your configuration files

### **Create the JSON file that configures the data pipeline operation**

1. Access your **tailer-demo** folder.
2. Inside, create a folder named **5-Export-data** for this new step.
3. In this folder, create a JSON file named **000099-tailer-demo-export-data.json** for your data operation.
4.  Copy the following contents into your file:

    ```
    {
      "configuration_type": "table-to-storage",
      "configuration_id": "000099-tailer-demo-export-data",
      "environment": "DEV",
      "account": "000099",
      "activated": true,
      "archive": false,

      "gcp_project": "my-gcp-project",
      "sql_file": "export_data.sql",

      "copy_table": true,
      "dest_gcp_project_id": "my-gcp-project",
      "dest_gbq_dataset": "my-gbq-dataset",
      "dest_gbq_table": "export_data",

      "gcs_dest_bucket": "my-gcs-bucket",
      "gcs_dest_prefix": "store_clustering_export/",
      "delete_dest_bucket_content": false,
      "output_filename": "_export_data.csv",
      "field_delimiter": "|",
      "compression": "NONE"
    }
    ```
5. Edit the following values:\
   ◾ Replace **my-gcp-project** with the ID of the GCP project containing the source table. This is where the SQL query will be run.\
   ◾ Replace **my-gbq-dataset** with the name of the dataset where you want to create a copy of the table generated with the SQL request.\
   ◾ Replace **my-gcs-bucket** with the name of the bucket that you've just created, where the export file will be generated.
6. Save your file.

### **Create a SQL file**

1. Inside the **5-Export-data** folder, create a file named export\_data.sql.
2.  Copy the following contents into the **export\_data.sql** file:

    ```
    SELECT 
    *
    FROM `my-gbq-dataset.store_clustering`
    ```
3. Replace **my-gbq-dataset** with the name of your working dataset in the previous step.
4. Save your file.

### **Create the JSON file that will trigger the workflow**

1. Inside the **5-Export-data** folder, create a file named **workflow.json**.
2.  Copy the following contents into your file:

    ```
    {
      "configuration_type": "workflow",
      "configuration_id": "000099-tailer-demo-export-data",
      "environment": "DEV",
      "short_description": "Launch Tailer demo data export",
      "account": "000099",
      "activated": true,
      "archived": false,
      "authorized_job_ids": ["gbq-to-gbq|000099-tailer-demo-build-predictions_DEV"],
      "target_dag": "table-to-storage",
      "extra_parameters": {
        "firestore_conf_doc_id": "000099-tailer-demo-export-data"
      }
    }
    ```
3. Save your file.

## :arrow\_forward: Deploy the data operation

Once your files are ready, you can deploy the data operation:

1.  Access your working folder by running the following command:

    ```
    cd "[path to your tailer folder]\tailer-demo\5-Export-data"
    ```
2.  To deploy the data operation, run the following command:

    ```
    tailer deploy configuration 000099-tailer-demo-export-data.json
    ```

## :hand\_splayed: Run your workflow manually

{% hint style="info" %}
Deploying the workflow at this stage would not launch it, as the workflow will only be triggered by the execution of the previous step (building predictions). We will run it manually for now so we can see the result. Once we finish setting up the pipeline, the workflow will run automatically starting from its first step (copying files) when we add files into the source bucket.
{% endhint %}

1. Access [Tailer Studio](http://studio.tailer.ai).‌
2. In the left navigation menu, select **Table-to-storage**.
3. In the **Configurations** tab, search for your data operation, **000099-tailer-demo-export-data**.
4. Click the data operation ID to display its details.
5. In the upper right corner, click on **Launch**.

## :ballot\_box: Check the result in GCS

Access the GCS folder in the bucket you've just created. Your data should now appear in the form of a CSV file that you can export to a different system.

{% hint style="success" %}
You can now add more files into the input folder from the [first step of this tutorial](prepare-the-demonstration-environment.md) to see the whole pipeline play out!
{% endhint %}
