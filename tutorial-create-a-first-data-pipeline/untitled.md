---
description: >-
  The first data operation of this tutorial will consist in transferring the
  files from one bucket located in one GCP project to another bucket located in
  a different GCP project.
---

# Copy files from one bucket to another

## 📄 Create a JSON file

1. Access your **tailer** folder \(created during [installation](../getting-started/install-tailer-sdk.md)\).
2. Create a working folder named **tailer-demo** for this tutorial, and inside create a folder named **1-Copy\_files** for this step.
3. In your **1-Copy\_files** folder, create a JSON file named **000099-tailer-demo-sts.json** for your data operation.
4. Copy the following contents into your file:

   ```text
   {
     "configuration_type": "storage-to-storage",
     "configuration_id": "000099-tailer-demo-copy-files",
     "environment": "DEV",
     "account": "000099",
     "activated": true,
     "archive": false,
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
       "gcp_project_id": "my-gcp-project",
       "gcs_source_bucket": "my-source-bucket",
       "gcs_source_prefix": "tailer-demo-input-folder",
       "gcs_archive_prefix": "tailer-demo-archive-folder",
       "gcp_credentials_secret": {
         "cipher_aes": "xxx",
         "ciphertext": "xxx",
         "enc_session_key": "xxx",
         "tag": "xxx"
       }
     },
     "destinations": [
       {
         "type": "gcs",
         "gcs_destination_bucket": "my-destination-bucket",
         "gcs_destination_prefix": "tailer-demo-input-folder",
         "gcp_credentials_secret": {
           "cipher_aes": "xxx",
           "ciphertext": "xxx",
           "enc_session_key": "xxx",
           "tag": "xxx"
         }
       }
     ]
   }
   ```

5. Take note of the different parameters. For detailed information on storage-to-storage configuration file parameters, refer to [this page](https://support.fashiondata.io/knowledge/move-files-with-storage-to-storage).
6. Edit the following values: ◾ In the **source** section, replace **my-gcp-project** with the ID of the GCP project containing your source bucket. ◾ In the **source** section, replace **my-source-bucket** with the name of the GCS bucket containing the source files. ◾ In the **source** section, replace the value of the **gcp\_credentials\_secret parameter** with the service account credentials for the source GCP project. If you haven't generated them yet, refer to [this page](https://support.fashiondata.io/knowledge/generate-credentials). ◾ In the **destinations** section, replace **my-destination-bucket** with the name of the GCS bucket that will contain the output files. ◾ In the **destinations** section, replace the value of the **gcp\_credentials\_secret parameter** with the service account credentials for the destination GCP project. If you haven't done it yet, refer to [this page](https://support.fashiondata.io/knowledge/generate-credentials).

{% hint style="success" %}
Your JSON file is now ready to use.
{% endhint %}

## ▶ Deploy a first data operation

Once your JSON file is ready, you can deploy the data operation:

1. Access your working folder by running the following command:

   ```text
   cd "[path to your tailer folder]\tailer-demo\1-Copy_files"
   ```

2. To deploy the data operation, run the following command:

   ```text
   tailer deploy 000099-tailer-demo-copy-files.json
   ```

## ✅ Check the data operation status in Tailer Studio

1. Access [Tailer Studio](https://jarvis-platform.io/sign-in?redirect=%2F&__hstc=57968821.199e85015347f5cf00c120e5932c4c81.1601276395705.1601309087670.1601364404124.6&__hssc=57968821.3.1601364404124&__hsfp=649433320).
2. Sign in with your Tailer Platform credentials.
3. In the left navigation menu, select **Storage-to-storage**.
4. In the **Configurations** tab, search for your data operation, **000099-tailer-demo-copy-files**. You can see its status is **Activated**.
5. Click the data operation ID to display its parameters and full JSON file, or to leave comments about it.

## 🗳 Check the result in GCS

Access the folders you created when [preparing the demonstration environment](prepare-the-demonstration-environment.md):

* In your fist bucket, **input-tailer-demo-folder** should be empty.
* In your fist bucket, **archive-tailer-demo-folder** should contain a folder for each input file, named as the filename date.
* In your second bucket, **input-tailer-demo-folder** should contain a copy of the input files.

