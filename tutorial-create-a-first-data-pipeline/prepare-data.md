---
description: >-
  The third data operation of this tutorial will consist in preparing data
  within BigQuery tables.
---

# Prepare data

## 🗺 Overview

The objective of this step will be to create new BigQuery tables into which we will load and reorganize the contents of the tables created at the previous step. As in most cases, this will happen within one BigQuery dataset. For this, we will need:

* a JSON file to configure the data operation,
* a JSON file to trigger the workflow,
* a JSON file for each table creation,
* and a SQL file for each transfer of data into our new tables.

## 📄 Create your files

### **Create the JSON file that configures the data operation**

1. Access your **tailer-demo** folder.
2. Inside, create a folder named **3-Prepare-data** for this new step.
3. In this folder, create a JSON file named **000099-tailer-demo-prepare-data.json** for your data operation.
4. Copy the following contents into your file:

   ```text
   {
     "configuration_type": "table-to-table",
     "configuration_id": "000099-tailer-demo-prepare-data",
     "short_description": "Prepare data for the Tailer demo",
     "doc_md": "tailer-demo-ttt.md",
     "account": "000099",
     "environment": "DEV",
     "activated": true,
     "archived": false,
     "start_date": "2019, 1, 23",
     "catchup": false,
     "schedule_interval": "None",
     "max_active_runs": 1,
     "task_concurrency": 3,
     "default_gcp_project_id": "my-gcp-project",
     "default_bq_dataset": "my-gbq-dataset",
     "default_write_disposition": "WRITE_TRUNCATE",
     "task_dependencies": [
       "[create_stores_table,create_products_table] >> sales_tmp0_load_pda >> [stores_load_pda,products_load_pda,create_sales_details_table] >> sales_details_load_pda"
     ],
     "workflow": [
       {
         "id": "create_stores_table",
         "short_description": "Create my-gcp-project.my-gbq-dataset.stores table",
         "task_type": "create_gbq_table",
         "bq_table": "stores",
         "force_delete": true,
         "ddl_file": "create_stores_table.json"
       },
       {
         "id": "create_products_table",
         "short_description": "Create my-gcp-project.my-gbq-dataset.products table",
         "task_type": "create_gbq_table",
         "bq_table": "products",
         "force_delete": true,
         "ddl_file": "create_products_table.json"
       },
       {
         "task_type": "sql",
         "id": "stores_load_pda",
         "short_description": "Load store repository data",
         "table_name": "stores",
         "sql_file": "load_stores_data.sql"
       },
       {
         "task_type": "sql",
         "id": "products_load_pda",
         "short_description": "Load product repository data",
         "table_name": "products",
         "sql_file": "load_products_data.sql"
       },
       {
         "task_type": "sql",
         "id": "sales_tmp0_load_pda",
         "short_description": "Load temporary sales data",
         "table_name": "sales_tmp0",
         "sql_file": "load_sales_tmp0.sql"
       },
       {
         "id": "create_sales_details_table",
         "short_description": "Create my-gcp-project.my-gbq-dataset.sales_details table",
         "task_type": "create_gbq_table",
         "bq_table": "sales_details",
         "force_delete": true,
         "ddl_file": "create_sales_details_table.json"
       },
       {
         "task_type": "sql",
         "id": "sales_details_load_pda",
         "short_description": "Load final Iowa Liquor sales details data",
         "table_name": "sales_details",
         "sql_file": "load_sales_details_data.sql"
       }
     ]
   }
   ```

5. Edit the following values: ◾ Replace **my-gcp-project-id** with the ID of the GCP project containing your BigQuery dataset. ◾ Replace **my-gbq-dataset** with the name of your working dataset.
6. Create a Markdown file named **000099-tailer-demo-prepare-data.md**. You can use it freely to describe the data operation.

### **Create the JSON file that triggers the workflow**

Inside the **3-Prepare-data** folder, create a file named **workflow.json**.

Copy the following contents into your file:

```
{
  "configuration_type": "workflow",
  "configuration_id": "000099-prepare-data-workflow",
  "environment": "DEV",
  "short_description": "Launch the Tailer demo data load",
  "account": "000099",
  "activated": true,
  "archived": false,
  "authorized_job_ids": [
    "storage-to-tables|000099|000099-tailer-demo-load-files|DEV|sales_-.csv",
    "storage-to-tables|000099|000099-tailer-demo-load-files|DEV|stores--.csv",
    "storage-to-tables|000099|000099-tailer-demo-load-files|DEV|products--.csv"
  ],
  "target_dag": "000099-tailer-demo-prepare-data_DEV",
  "extra_parameters": {}
```

### **Create the JSON files that create new tables**

Inside the **3-Prepare-data** folder, create the following files:  
**◾ create\_stores\_table.json**  
**◾ create\_products\_table.json**  
**◾ create\_sales\_details\_table.json**

Copy the following contents into the **create\_stores\_table.json** file:

```
{
  "bq_table_description": "Iowa Liquor store repository",
  "bq_table_schema": [
    {
      "name": "store_number",
      "type": "STRING",
      "description": "Unique number of the store that ordered the liquor."
    },
    {
      "name": "store_name",
      "type": "STRING",
      "description": "Name of store that ordered the liquor."
    },
    {
      "name": "address",
      "type": "STRING",
      "description": "Address of store that ordered the liquor."
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
  ],
  "bq_table_clustering_fields": ["store_number"]
}
```

Copy the following contents into the **create\_products\_table.json** file:

```
{
  "bq_table_description": "Iowa Liquor product repository",
  "bq_table_schema": [
    {
      "name": "category",
      "type": "STRING",
      "description": "Category code associated with the liquor."
    },
    {
      "name": "category_name",
      "type": "STRING",
      "description": "Category of the liquor"
    },
    {
      "name": "vendor_number",
      "type": "STRING",
      "description": "The vendor number of the company for the liquor brand."
    },
    {
      "name": "item_number",
      "type": "STRING",
      "description": "Item number for the individual liquor product."
    },
    {
      "name": "item_description",
      "type": "STRING",
      "description": "Description of the individual liquor product."
    },
    {
      "name": "pack",
      "type": "INTEGER",
      "description": "The number of bottles in a case for the liquor."
    },
    {
      "name": "bottle_volume_ml",
      "type": "INTEGER",
      "description": "Volume of each liquor bottle in milliliters."
    },
    {
      "name": "state_bottle_cost",
      "type": "FLOAT",
      "description": "The amount the State paid for each bottle of liquor."
    },
    {
      "name": "state_bottle_retail",
      "type": "FLOAT",
      "description": "The amount the store paid for each bottle of liquor."
    }
  ],
  "bq_table_clustering_fields": ["item_number"]
}
```

Copy the following contents into the **create\_sales\_details\_table.json** file:

```
{
  "bq_table_description": "Iowa Liquor sales details",
  "bq_table_schema": [
    {
      "name": "invoice_and_item_number",
      "type": "STRING",
      "description": "Concatenated invoice and line number of the liquor order."
    },
    {
      "name": "date",
      "type": "DATE",
      "description": "Date of order."
    },
    {
      "name": "bottles_sold",
      "type": "INTEGER",
      "description": "The number of bottles of liquor ordered by the store."
    },
    {
      "name": "sale_dollars",
      "type": "FLOAT",
      "description": "Total cost of liquor order."
    },
    {
      "name": "volume_sold_liters",
      "type": "FLOAT",
      "description": "Total volume of liquor ordered in liters."
    },
    {
      "name": "volume_sold_gallons",
      "type": "FLOAT",
      "description": "Total volume of liquor ordered in gallons."
    },
    {
      "name": "category",
      "type": "STRING",
      "description": "Category code associated with the liquor."
    },
    {
      "name": "category_name",
      "type": "STRING",
      "description": "Category of the liquor."
    },
    {
      "name": "vendor_number",
      "type": "STRING",
      "description": "The vendor number of the company for the brand of liquor."
    },
    {
      "name": "item_number",
      "type": "STRING",
      "description": "Item number for the individual liquor product."
    },
    {
      "name": "item_description",
      "type": "STRING",
      "description": "Description of the individual liquor product."
    },
    {
      "name": "pack",
      "type": "INTEGER",
      "description": "The number of bottles in a case for the liquor"
    },
    {
      "name": "bottle_volume_ml",
      "type": "INTEGER",
      "description": "Volume of each liquor bottle in milliliters"
    },
    {
      "name": "state_bottle_cost",
      "type": "FLOAT",
      "description": "The amount that the State paid for each bottle of liquor."
    },
    {
      "name": "state_bottle_retail",
      "type": "FLOAT",
      "description": "The amount the store paid for each bottle of liquor"
    },
    {
      "name": "store_number",
      "type": "STRING",
      "description": "Unique number of the store that ordered the liquor."
    },
    {
      "name": "store_name",
      "type": "STRING",
      "description": "Name of store that ordered the liquor."
    },
    {
      "name": "address",
      "type": "STRING",
      "description": "Address of store who ordered the liquor."
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
      "description": "Location of store who ordered the liquor. "
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
  ],
  "bq_table_timepartitioning_field": "date",
  "bq_table_clustering_fields": ["item_number", "vendor_number"]
```

### **Create the SQL files that load data into the new tables**

1. Inside the **3-Prepare-data** folder, create the following files: **◾ load\_stores\_data.sql** **◾ load\_products\_data.sql** **◾ load\_sales\_tmp0.sql** **◾ load\_sales\_details\_data.sql**
2. Copy the following contents into the **stores\_load\_data.sql** file:

   ```text
   WITH last_stores_info AS (
   SELECT
   store_number,
   MAX(_TABLE_SUFFIX) as MAX_TABLE_SUFFIX
   FROM `my-gbq-dataset.stores_*`
   GROUP BY 1)
   SELECT
   DISTINCT
   t1.store_number,
   t1.store_name,
   t1.address,
   t1.city,
   t1.zip_code,
   t1.store_location,
   t1.county_number,
   t1.county
   FROM `my-gbq-dataset.stores_*` t1
   INNER JOIN last_stores_info t2 on t1.store_number = t2.store_number AND t1._TABLE_SUFFIX = t2.MAX_TABLE_SUFFIX
   ```

3. Replace **my-gbq-dataset** with the name of your working dataset.
4. Copy the following contents into the **products\_load\_data.sql** file:

   ```text
   WITH last_products_info AS (
   SELECT
   item_number,
   MAX(_TABLE_SUFFIX) as MAX_TABLE_SUFFIX
   FROM `my-gbq-dataset.products_*`
   GROUP BY 1)
   SELECT
   DISTINCT
   t1.category,
   t1.category_name,
   t1.vendor_number,
   t1.item_number,
   t1.item_description,
   CAST(t1.pack AS INT64) as pack,
   CASt(t1.bottle_volume_ml as INT64) as bottle_volume_ml,
   CAST(t1.state_bottle_cost as FLOAT64) as state_bottle_cost,
   CAST(t1.state_bottle_retail as FLOAT64) as state_bottle_retail
   FROM `my-gbq-dataset.products_*` t1
   INNER JOIN last_products_info t2 on t1.item_number = t2.item_number AND t1._TABLE_SUFFIX = t2.MAX_TABLE_SUFFIX
   ```

5. Replace **my-gbq-dataset** with the name of your working dataset.
6. Copy the following contents into the **sales\_details\_load\_data.sql** file:

   ```text
   SELECT
   t1.invoice_and_item_number,
   t1.date,
   t1.item_number,
   t1.store_number,
   t1.bottles_sold,
   t1.sale_dollars,
   t1.volume_sold_liters,
   t1.volume_sold_gallons,
   t2.store_name,
   t2.address,
   t2.city,
   t2.zip_code,
   t2.store_location,
   t2.county_number,
   t2.county,
   t3.category,
   t3.category_name,
   t3.vendor_number,
   t3.item_description,
   t3.pack,
   t3.bottle_volume_ml,
   t3.state_bottle_cost,
   t3.state_bottle_retail
   FROM my-gbq-dataset.sales_tmp0 t1
   LEFT JOIN my-gbq-dataset.stores t2 on t1.store_number = t2.store_number
   LEFT JOIN my-gbq-dataset.products t3 on t1.item_number = t3.item_number
   ```

7. Replace **my-gbq-dataset** with the name of your working dataset.
8. Copy the following contents into the **sales\_tmp0.sql** file:

   ```text
   SELECT
   DISTINCT
   invoice_and_item_number,
   CAST(date AS DATE) as date,
   store_number,
   item_number,
   CAST(bottles_sold AS INT64) as bottles_sold,
   CAST(bottle_volume_ml as INT64) as bottle_volume_ml,
   CAST(sale_dollars AS FLOAT64) as sale_dollars,
   CAST(volume_sold_liters AS FLOAT64) as volume_sold_liters,
   CAST(volume_sold_gallons AS FLOAT64) as volume_sold_gallons
   FROM `my-gbq-dataset.sales_*`
   ```

9. Replace **my-gbq-dataset** with the name of your working dataset.

## ▶ Deploy the data operation

Once your files are ready, you can deploy the data operation:

1. Access your working folder by running the following command:

   ```text
   cd "[path to your tailer folder]\tailer-demo\3-Prepare-data"
   ```

2. To deploy the data operation, run the following command:  


   ```text
   tailer deploy configuration 000099-tailer-demo-prepare-data.json
   ```

3. To trigger the workflow, run the following command:

   ```text
   tailer deploy configuration workflow.json
   ```

{% hint style="success" %}
Your data operation is now deployed, which means the new tables will shortly be created and loaded with data, and your data operation status is now visible in Tailer Studio.
{% endhint %}

## ✅ Check the data operation status in Tailer Studio

1. Access [Tailer Studio](https://jarvis-platform.io/sign-in?redirect=%2F&__hstc=57968821.199e85015347f5cf00c120e5932c4c81.1601276395705.1601366647379.1601368536601.8&__hssc=57968821.1.1601368536601&__hsfp=649433320) again.‌
2. In the left navigation menu, select **Tables-to-tables**.
3. In the **Configurations** tab, search for your data operation, **000099-tailer-demo-prepare-data**. You can see its status is **Activated**.
4. Click the data operation ID to display its parameters and full JSON file, or to leave comments about it.

