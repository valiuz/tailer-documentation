---
description: >-
  The second data operation of this tutorial will consist in loading the
  contents of our files into BigQuery tables.
---

# Load files into BigQuery tables

## 📄 Create your configuration files

### **Create the JSON file for the data pipeline operation**

1. Access your **tailer-demo** folder.
2. Inside, create a folder named **2-Load\_files** for this new step.
3. In this folder, create a JSON file named **000099-tailer-demo-load-files.json** for your data operation.
4. Copy the following contents into your file:

   ```text
   {
     "configuration_type": "storage-to-tables",
     "configuration_id": "000099-tailer-demo-load-files",
     "environment": "DEV",
     "account": "000099",
     "activated": true,
     "archived": false,
     "short_description": "This data operation loads files into BigQuery tables.",
     "doc_md": "tailer-demo-stt.md",
     "source": {
       "type": "gcs",
       "gcp_project_id": "my-gcp-project",
       "gcs_source_bucket": "my-second-bucket",
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
         "type": "bigquery",
         "gcp_project_id": "my-gcp-project",
         "gbq_dataset": "my-gbq-dataset",
         "source_format": "CSV",
         "create_disposition": "CREATE_IF_NEEDED",
         "write_disposition": "WRITE_TRUNCATE",
         "skip_leading_rows": 1,
         "field_delimiter": ",",
         "quote_character": "\"",
         "gcp_credentials_secret": {
           "cipher_aes": "xxx",
           "ciphertext": "xxx",
           "enc_session_key": "xxx",
           "tag": "xxx"
         },
         "tables": [
           {
             "table_name": "stores",
             "short_description": "Store repository",
             "filename_template": "stores-{{FD_DATE}}-{{FD_TIME}}.csv",
             "ddl_file": "ddl/stores.json",
             "doc_md": "ddl/stores.md"
           },
           {
             "table_name": "products",
             "short_description": "Product repository",
             "filename_template": "products-{{FD_DATE}}-{{FD_TIME}}.csv",
             "ddl_file": "ddl/products.json"
           },
           {
             "table_name": "sales",
             "short_description": "Daily Iowa Liquor sales",
             "filename_template": "sales_{{FD_BLOB_8}}-{{FD_DATE}}.csv",
             "ddl_file": "ddl/sales.json"
           },
           {
             "table_name": "sales_daily",
             "short_description": "Daily Iowa Liquor sales",
             "filename_template": "sales_{{FD_DATE}}.csv",
             "ddl_file": "ddl/sales_daily.json"
           }
         ]
       }
     ]
   }
   ```

5. Edit the following values:  


   ◾ In the **source** section, replace **my-gcp-project** with the ID of the GCP project containing the source bucket.

   ◾ In the **source** section, replace **my-second-bucket** with the name of the GCS bucket containing the input files \(output files from the previous step\).

   ◾In the **source** section, replace the value of the **gcp\_credentials\_secret parameter** with the service account credentials for the GCP project containing the source bucket.

   ◾ In the **destinations** section, replace **my-gcp-project** with the ID of the GCP project containing the target dataset. It can be the same as in the source section or a different one.

   ◾ In the **destinations** section, replace **my-gbq-dataset** with the name of the dataset that will contain the tables.

   ◾ In the **destinations** section, replace the value of the **gcp\_credentials\_secret parameter** with the service account credentials for the GCP project containing the target dataset.

6. Create a Markdown file named **tailer-demo-stt.md**. You can use it freely to describe the data operation.

### **Create the table schema files‌**

1. Inside the **2-Load\_files** folder, create a folder named **ddl**. It will contain the table schema files.
2. Inside the **ddl** folder, create four files: **◾ stores.json** ◾ **products.json** **◾ sales.json** **◾ sales\_daily.json**
3. Copy the following contents into the **stores.json** file:

   ```text
   {
     "schema": [
       {
         "name": "store_number",
         "type": "STRING",
         "description": "Unique number of the store that ordered the liquor."
       },
       {
         "name": "store_name",
         "type": "STRING",
         "description": "Name of the store that ordered the liquor."
       },
       {
         "name": "address",
         "type": "STRING",
         "description": "Address of the store that ordered the liquor."
       },
       {
         "name": "city",
         "type": "STRING",
         "description": "City of the store that ordered the liquor."
       },
       {
         "name": "zip_code",
         "type": "STRING",
         "description": "Zip code of the store that ordered the liquor."
       },
       {
         "name": "store_location",
         "type": "STRING",
         "description": "Location of the store that ordered the liquor."
       },
       {
         "name": "county_number",
         "type": "STRING",
         "description": "Iowa county number of the store that ordered the liquor."
       },
       {
         "name": "county",
         "type": "STRING",
         "description": "County of the store that ordered the liquor."
       }
     ]
   }
   ```

4. Copy the following contents into the **products.json** file.

   ```text
   {
     "schema": [
       {
         "name": "category",
         "type": "STRING",
         "description": "Category code of the liquor."
       },
       {
         "name": "category_name",
         "type": "STRING",
         "description": "Category of the liquor."
       },
       {
         "name": "vendor_number",
         "type": "STRING",
         "description": "The vendor number of the company for the liquor brand."
       },
       {
         "name": "item_number",
         "type": "STRING",
         "description": "Item number for each individual liquor product."
       },
       {
         "name": "item_description",
         "type": "STRING",
         "description": "Description of each individual liquor product."
       },
       {
         "name": "pack",
         "type": "STRING",
         "description": "The number of bottles in one box for the liquor."
       },
       {
         "name": "bottle_volume_ml",
         "type": "STRING",
         "description": "Volume of each liquor bottle in milliliters."
       },
       {
         "name": "state_bottle_cost",
         "type": "STRING",
         "description": "The amount the State paid for each bottle of liquor."
       },
       {
         "name": "state_bottle_retail",
         "type": "STRING",
         "description": "The amount the store paid for each bottle of liquor."
       }
     ]
   }
   ```

5. Copy the following contents into the **sales.json** file.

   ```text
   {
     "schema": [
       {
         "name": "invoice_and_item_number",
         "type": "STRING",
         "description": "Concatenated invoice and line number of the liquor order."
       },
       {
         "name": "date",
         "type": "STRING",
         "description": "Date of order."
       },
       {
         "name": "store_number",
         "type": "STRING",
         "description": "Unique number of the store that ordered the liquor."
       },
       {
         "name": "item_number",
         "type": "STRING",
         "description": "Item number for each individual liquor product ordered."
       },
       {
         "name": "bottles_sold",
         "type": "STRING",
         "description": "The number of bottles of liquor ordered by the store."
       },
       {
         "name": "bottle_volume_ml",
         "type": "STRING",
         "description": "Volume of each liquor bottle ordered in milliliters."
       },
       {
         "name": "sale_dollars",
         "type": "STRING",
         "description": "Total cost of liquor order."
       },
       {
         "name": "volume_sold_liters",
         "type": "STRING",
         "description": "Total volume of liquor ordered in liters."
       },
       {
         "name": "volume_sold_gallons",
         "type": "STRING",
         "description": "Total volume of liquor ordered in gallons."
       }
     ]
   }
   ```

6. Copy the following contents into the **sales\_daily.json** file.

   ```text
   {
     "schema": [
       {
         "name": "invoice_and_item_number",
         "type": "STRING",
         "description": "Concatenated invoice and line number of the liquor order."
       },
       {
         "name": "date",
         "type": "STRING",
         "description": "Date of order"
       },
       {
         "name": "store_number",
         "type": "STRING",
         "description": "Unique number of the store that ordered the liquor."
       },
       {
         "name": "item_number",
         "type": "STRING",
         "description": "Item number for each individual liquor product ordered."
       },
       {
         "name": "bottles_sold",
         "type": "STRING",
         "description": "The number of bottles of liquor ordered by the store."
       },
       {
         "name": "bottle_volume_ml",
         "type": "STRING",
         "description": "Volume of each liquor bottle ordered in milliliters."
       },
       {
         "name": "sale_dollars",
         "type": "STRING",
         "description": "Total cost of liquor order."
       },
       {
         "name": "volume_sold_liters",
         "type": "STRING",
         "description": "Total volume of liquor ordered in liters."
       },
       {
         "name": "volume_sold_gallons",
         "type": "STRING",
         "description": "Total volume of liquor ordered in gallons."
       }
     ]
   }
   ```

7. Create Markdown files for each DDL file. You can use them freely to describe the table schemas.

{% hint style="info" %}
By default, in the DDL, all the database fields created have the "string" type. This will be modified during the next data pipeline operation if necessary.
{% endhint %}

## ▶ Deploy the data operation

Once your files are ready, you can deploy the data operation:

1. Access your working folder by running the following command:

   ```text
   cd "[path to your tailer folder]\jarvis-demo\2-Load_files"
   ```

2. To deploy the data operation, run the following command:

   ```text
   tailer deploy configuration 000099-tailer-demo-load-files.json
   ```

{% hint style="success" %}
Your data operation is now deployed, which means the files will shortly be loaded into tables, and your data operation status is now visible in Tailer Studio.
{% endhint %}

## ✅ Check the data operation status in Tailer Studio

1. Access [Tailer Studio](http://studio.tailer.ai/) again.‌
2. In the left navigation menu, select **Storage-to-tables**.
3. In the **Configurations** tab, search for your data operation, **000099-tailer-demo-load-files**. You can see its status is **Activated**.
4. Click the data operation ID to display its parameters and full JSON file, or to leave comments about it. in the **Tables** section, you can access the table schema, parameters, and documentation provided in the Markdown files.

