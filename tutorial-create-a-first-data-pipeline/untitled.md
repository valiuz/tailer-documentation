---
description: >-
  The first data operation of this tutorial will consist in transferring the
  files from one bucket located in one GCP project to another bucket located in
  a different GCP project.
---

# Copy files from one bucket to another

## :page\_facing\_up: Create a JSON file

1. Access your **tailer** folder (created during [installation](../getting-started/install-tailer-sdk.md)).
2. Create a working folder named **tailer-demo** for this tutorial, and inside create a folder named **1-Copy\_files** for this step.
3. In your **1-Copy\_files** folder, create a JSON file named **000099-tailer-demo-sts.json** for your data operation.
4.  Copy the following contents into your file:

    ```json
    {
      "$schema": "http://jsonschema.tailer.ai/schema/storage-to-storage-veditor",
      "configuration_type": "storage-to-storage",
      "configuration_id": "000099-tailer-demo-copy-files-YOUR-NAME",
      "environment": "DEV",
      "account": "000099",
      "version": "3",
      "activated": true,
      "archived": false,
      "filename_templates": [
        {
          "filename_template": "stores-{{FD_DATE}}-{{FD_TIME}}.csv",
          "file_description": "Stores repository. The store listing is the file could evolve over time"
        },
        {
          "filename_template": "products-{{FD_DATE}}-{{FD_TIME}}.csv",
          "file_description": "Products repository. The product listing in the file could evolve over time"
        },
        {
          "filename_template": "sales_{{FD_BLOB_8}}-{{FD_DATE}}.csv",
          "file_description": "Daily Sales. There are many days in each files. And some days are repeated in different files"
        },
        {
          "filename_template": "sales_{{FD_DATE}}.csv",
          "file_description": "Daily Sales. There are many days in each files. And some days are repeated in different files"
        }
      ],
      
      "source": {
        "type": "gcs",
        "gcp_project_id": "my_gcp_project",
        "gcs_source_bucket" : "my-source-bucket",
        "gcs_source_prefix" : "input-folder-YOUR-NAME",
        "archive_prefix": "archive-folder-YOUR-NAME",
        "gcp_credentials_secret": {
          "cipher_aes": "b42xxx",
          "tag": "5c8xxx",
          "ciphertext": "fd0xxx",
          "enc_session_key": "8f6xxx"
        }
      },
      
      "destinations": [
        {
          "type": "gcs",
          "gcs_destination_bucket": "my-destination-bucket",
          "gcs_destination_prefix": "tailer-demo-input-folder-YOUR-NAME",
          "gcp_credentials_secret": {
            "cipher_aes": "b42xxx",
            "tag": "5c8xxx",
            "ciphertext": "fd0xxx",
            "enc_session_key": "8f6xxx"
          }
        }
      ]
    }
    ```
5. Take note of the different parameters. For detailed information on storage-to-storage configuration file parameters, refer to [this page](../data-pipeline-operations/move-files-with-storage-to-storage/storage-to-storage-configuration-file.md).
6. Edit the following values:\
   ◾ In the **source** section, replace **my-gcp-project** with the ID of the GCP project containing your source bucket.\
   ◾ In the **source** section, replace **my-source-bucket** with the name of the GCS bucket containing the source files.\
   ◾ In the **source** section, replace the value of the **gcp\_credentials\_secret parameter** with the service account credentials for the source GCP project. If you haven't generated them yet, refer to [this page](../getting-started/encrypt-your-credentials.md).\
   ◾ In the **destinations** section, replace **my-destination-bucket** with the name of the GCS bucket that will contain the output files.\
   ◾ In the **destinations** section, replace the value of the **gcp\_credentials\_secret parameter** with the service account credentials for the destination GCP project. If you haven't done it yet, refer to [this page](../getting-started/encrypt-your-credentials.md).\
   ◾ If you share the demo project with other developers, then in the configuration\_id, replace YOUR-NAME by a personal value, like your name. This way, you won't overwrite a configuration deployed by someone else. You should also add your name in the source's gcs\_source\_prefix and archive\_prefix, and in the destinations' gcs\_destination\_prefix to avoid any interferences with another developer's data operation.

{% hint style="success" %}
Your JSON file is now ready to use.
{% endhint %}

## :arrow\_forward: Deploy a first data operation

Once your JSON file is ready, you can deploy the data operation:

1.  Access your working folder by running the following command:

    ```
    cd "[path to your tailer folder]\tailer-demo\1-Copy_files"
    ```
2.  To deploy the data operation, run the following command:

    ```
    tailer deploy configuration 000099-tailer-demo-copy-files.json
    ```

You may be asked to select a context (see [this page](../data-pipeline-operations/set-constants-with-context/context-configuration-file.md) for more information). If you haven't deployed any context, then choose "no context". You can also use the flag --context to specify the context of your choice, or NO\_CONTEXT if that's what you want:

```
tailer deploy configuration 000099-tailer-demo-copy-files.json --context NO_CONTEXT
```

## :white\_check\_mark: Check the data operation status in Tailer Studio

1. Access [Tailer Studio](http://studio.tailer.ai).
2. Sign in with your Tailer Platform credentials.
3. In the left navigation menu, select **Storage-to-storage**.
4. In the **Configurations** tab, search for your data operation. You can see its status is **Activated**.
5. Click the data operation ID to display its parameters and full JSON file, or to leave comments about it.

## :ballot\_box: Check the result in GCS

Now that our configuration is deployed, we can test it. Let's mimic production behavior. Access the folders you created when [preparing the demonstration environment](prepare-the-demonstration-environment.md):

* In your source bucket, copy a file. The file name must match one of the filename\_template specified in the configuration.
* On Tailer Studio, in the Storage-to-Storage section, Runs tab, you should see a run for your data operation. It should appear as "running" and quickly get the status "success".
* In your source bucket, **input-folder** should be empty.
* In your source bucket, **archive-tailer-demo-folder** should contain a folder for each input file, named as the filename date.
* In your destination bucket, **input-tailer-demo-folder** should contain a copy of the input files.
