---
description: >-
  The fourth data operation of this tutorial will consist in analyzing our data
  with a machine learning model.
---

# Build predictions

## :map: Overview

During this step, we will create a clustering machine learning model using BigQuery ML. Then, we will aggregate all our data into one BigQuery table and use our new model to analyze it.

## :robot: Create your machine learning model

1. Go to the [BigQuery web UI](https://console.cloud.google.com/bigquery) in the Cloud Console.
2. In the navigation panel, in the **Resources** section, select your project and your dataset.
3.  Enter the following SQL query in the **Query editor** text area:

    ```
    ###################################################
    # Train Clustering Model in your Big Query Console
    ###################################################

    CREATE OR REPLACE MODEL my-gbq-dataset.model_clustering_store_iowa_liquor
    OPTIONS(model_type='kmeans', num_clusters=5, standardize_features = true) AS

    SELECT * except( store_number, store_name, volume_sold_liters, sale_q1, sale_q2, sale_q3, sale_q4)
    --FROM `my-gcp-project.my-gbq-dataset`
    FROM
    (

      with tmp as (
        SELECT store_number,
          store_name,
          EXTRACT(QUARTER FROM date) as quarter,
          case when(lower(category_name) like '%vodka%') then 'vodkas'
            when(lower(category_name) like '%whiskies%') then 'whiskies'
            when(lower(category_name) like '%rum%') then 'rum'
            when(lower(category_name) like '%liqueur%') then 'liqueur'
            when(lower(category_name) like '%tequila%') then 'tequila'
            when(lower(category_name) like '%schnapps%') then 'schnapps'
            when(lower(category_name) like '%gin%') then 'gin'
            when(lower(category_name) like '%cocktails%') then 'cocktails'
            when(lower(category_name) like '%brandies%') then 'brandies'
            when(lower(category_name) like '%spirit%') then 'spirit'
          else 'autre' end cat_alcool,
          sum(sale_dollars) as sale_dollars,
          sum(volume_sold_liters) as volume_sold_liters
        FROM dlk_demo_iowa_liquor_pda.sales_details
        where date >= '2019-01-01' and date < '2020-01-01'
        group by store_number,store_name,quarter,cat_alcool
      )
      select store_number,
        store_name,
        sum(sale_dollars) as sale_dollars,
        sum(volume_sold_liters) as volume_sold_liters,

        round(sum(case when(quarter = 1) then sale_dollars else 0 end) / sum(sale_dollars) , 2) as sale_q1,
        round(sum(case when(quarter = 2) then sale_dollars else 0 end) / sum(sale_dollars) , 2) as sale_q2,
        round(sum(case when(quarter = 3) then sale_dollars else 0 end) / sum(sale_dollars) , 2) as sale_q3,
        round(sum(case when(quarter = 4) then sale_dollars else 0 end) / sum(sale_dollars) , 2) as sale_q4,

        round(sum(case when(cat_alcool = 'vodkas') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_vodkas,
        round(sum(case when(cat_alcool = 'whiskies') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_whiskies,
        round(sum(case when(cat_alcool = 'rum') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_rum,
        round(sum(case when(cat_alcool = 'liqueur') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_liqueur,
        round(sum(case when(cat_alcool = 'tequila') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_tequila,
        round(sum(case when(cat_alcool = 'schnapps') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_schnapps,
        round(sum(case when(cat_alcool = 'gin') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_gin,
        round(sum(case when(cat_alcool = 'cocktails') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_cocktails,
        round(sum(case when(cat_alcool = 'brandies') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_brandies,
        round(sum(case when(cat_alcool = 'spirit') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_spirit
      from tmp
      group by store_number,store_name
    )
    ```
4. Click **Run**.

{% hint style="success" %}
The query takes several minutes to complete. After the first iteration is complete, your model (sample\_model) appears in the navigation panel of the BigQuery web UI.

You can observe the model as it's being trained by viewing the **Training** tab in the BigQuery web UI.
{% endhint %}

## :page\_facing\_up: Create your configuration files

### **Create the JSON file that configures the data pipeline operation**

1. Access your **tailer-demo** folder.
2. Inside, create a folder named **4-Build-predictions** for this new step.
3. In this folder, create a JSON file named **tailer-demo-build-predictions.json** for your data operation.
4.  Copy the following contents into your file:

    ```
    {
      "configuration_type": "table-to-table",
      "configuration_id": "000099-tailer-demo-build-predictions",
      "short_description": "Build Tailer demo predictions",
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
      "task_dependencies": ["iowa_liquor_agg_store >> store_clustering"],
      "workflow": [
        {
          "task_type": "sql",
          "id": "iowa_liquor_agg_store",
          "short_description": "Aggregate data for clustering ",
          "table_name": "iowa_liquor_agg_store",
          "sql_file": "iowa_liquor_agg_store.sql"
        },
        {
          "task_type": "sql",
          "id": "store_clustering",
          "short_description": "Affect every store to the right cluster",
          "table_name": "store_clustering",
          "sql_file": "store_clustering.sql"
        }
      ]
    }
    ```
5. Edit the following values:\
   ◾ Replace **my-gcp-project-id** with the ID of the GCP project containing your BigQuery dataset.\
   ◾ Replace **my-gbq-dataset** with the name of your working dataset.

### **Create the JSON file that triggers the workflow**

1. Inside the **4-Build-predictions** folder, create a file named **workflow.json**.
2.  Copy the following contents into your file:

    ```
    {
      "configuration_type": "workflow",
      "configuration_id": "000099-tailer-demo-build-predictions-workflow",
      "environment": "DEV",
      "short_description": "Launch the Tailer demo model execution with BQ",
      "account": "000099",
      "activated": true,
      "archived": false,
      "authorized_job_ids": ["gbq-to-gbq|tailer-demo-build-predictions_DEV"],
      "target_dag": "tailer-demo-build-predictions_DEV",
      "extra_parameters": {}
    }
    ```

### **Create SQL files**

1.  Inside the **4-Build-predictions** folder, create the following files:\


    **◾ iowa\_liquor\_agg\_store.sql**

    **◾ store\_clustering.sql**
2.  Copy the following contents into the **iowa\_liquor\_agg\_store.sql** file:

    ```
    with tmp as (

    SELECT store_number,
      store_name,
      EXTRACT(QUARTER FROM date) as quarter,
      case when(lower(category_name) like '%vodka%') then 'vodkas'
        when(lower(category_name) like '%whiskies%') then 'whiskies'
        when(lower(category_name) like '%rum%') then 'rum'
        when(lower(category_name) like '%liqueur%') then 'liqueur'
        when(lower(category_name) like '%tequila%') then 'tequila'
        when(lower(category_name) like '%schnapps%') then 'schnapps'
        when(lower(category_name) like '%gin%') then 'gin'
        when(lower(category_name) like '%cocktails%') then 'cocktails'
        when(lower(category_name) like '%brandies%') then 'brandies'
        when(lower(category_name) like '%spirit%') then 'spirit'
      else 'autre' end cat_alcool,
      sum(sale_dollars) as sale_dollars,
      sum(volume_sold_liters) as volume_sold_liters
    FROM dlk_demo_iowa_liquor_pda.sales_details
    where date >= '2019-01-01' and date < '2020-01-01'
    group by store_number,store_name,quarter,cat_alcool

    )

    select store_number,
      store_name,
      sum(sale_dollars) as sale_dollars,
      sum(volume_sold_liters) as volume_sold_liters,

      round(sum(case when(quarter = 1) then sale_dollars else 0 end) / sum(sale_dollars) , 2) as sale_q1,
      round(sum(case when(quarter = 2) then sale_dollars else 0 end) / sum(sale_dollars) , 2) as sale_q2,
      round(sum(case when(quarter = 3) then sale_dollars else 0 end) / sum(sale_dollars) , 2) as sale_q3,
      round(sum(case when(quarter = 4) then sale_dollars else 0 end) / sum(sale_dollars) , 2) as sale_q4,

      round(sum(case when(cat_alcool = 'vodkas') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_vodkas,
      round(sum(case when(cat_alcool = 'whiskies') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_whiskies,
      round(sum(case when(cat_alcool = 'rum') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_rum,
      round(sum(case when(cat_alcool = 'liqueur') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_liqueur,
      round(sum(case when(cat_alcool = 'tequila') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_tequila,
      round(sum(case when(cat_alcool = 'schnapps') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_schnapps,
      round(sum(case when(cat_alcool = 'gin') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_gin,
      round(sum(case when(cat_alcool = 'cocktails') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_cocktails,
      round(sum(case when(cat_alcool = 'brandies') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_brandies,
      round(sum(case when(cat_alcool = 'spirit') then sale_dollars else 0 end) / sum(sale_dollars) , 2) as p_spirit

    from tmp
    group by store_number,store_name
    ```
3. Replace **my-gbq-dataset** with the name of your working dataset.
4.  Copy the following contents into the **store\_clustering.sql** file:

    ```
    ######################################################
    # Affect every store to the right cluster
    ######################################################

    SELECT * except(nearest_centroids_distance) 
    FROM ML.PREDICT(MODEL my-gbq-dataset.model_clustering_store_iowa_liquor, 
    (
    SELECT * except(sale_q1, sale_q2, sale_q3, sale_q4)
    FROM `my-gbq-dataset.iowa_liquor_agg_store`

    ))
    ```
5. Replace **my-gbq-dataset** with the name of your working dataset.

## :arrow\_forward: Deploy the data operation

Once your files are ready, you can deploy the data operation:

1.  Access your working folder by running the following command:

    ```
    cd "[path to your tailer folder]\tailer-demo\4-Build-predictions"
    ```
2.  To deploy the data operation, run the following command:

    ```
    tailer deploy configuration 000099-tailer-demo-build-predictions.json
    ```
3.  To trigger the workflow, run the following command:

    ```
    tailer deploy configuration workflow.json
    ```

{% hint style="success" %}
Your data operation is now deployed, which means all your data will shortly be aggregated and analyzed by the machine learning model you have created. The **Evaluation** tab of your model allows you to view data clustering and get some first insights out of your data.

Your data operation status is now visible in Tailer Studio.
{% endhint %}

## :white\_check\_mark: Check the data operation status in Tailer Studio

1. Access [Tailer Studio](http://studio.tailer.ai).‌
2. In the left navigation menu, select **Table-to-table**.
3. In the **Configurations** tab, search for your data operation, **000099-tailer-demo-build-predictions**. You can see its status is **Activated**.
4. Click the data operation ID to display its parameters and full JSON file, or to leave comments about it.
