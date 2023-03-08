# Table to Storage: configuration file

The GBQ to Firestore data pipeline starts with a table-to-storage (TTS) data operation. You can find the global parameters of this configuration in the [Tables to storage configuration](../export-data-with-tables-to-storage/table-to-storage-configuration-file-1.md) page.

This data operation executes a SQL query to extract data from BigQuery and stores it into a data file (or a set of data files if there's a lot of data to extract) in the Google Cloud Storage of your choice. You can configure it as you like, but you need to store the data into a JSON file and the SQL must follow a specific pattern.

{% hint style="info" %}
If you want to extract data from BigQuery to load it into Firestore, then **you must** specify "destination\_format": "**NEWLINE\_DELIMITED\_JSON**" in your configuration file.
{% endhint %}

## :eye\_in\_speech\_bubble: Configuration file example

Here is an example of TTS configuration file for a GBQ to Firestore data pipeline:

```json
{
    "configuration_type" : "table-to-storage",
    "configuration_id" : "000001_load_bda_freshness_next_exe_export_json",
    "short_description" : "this is a short description",
    "environment" : "PROD",
    "account" : "000001",
    "activated" : true,
    "archive" : false,
    "gcs_dest_bucket" : "tailer-freshness",
    "gcs_dest_prefix" : "gbq-to-firestore/000001/next_execution/",
    "delete_dest_bucket_content" : false,
    "gcp_project" : "fd-tailer-datalake",
    "gcp_project_id" : "fd-tailer-demo",
    "field_delimiter" : ",",
    "print_header": false,
    "sql_file" : "000001_load_bda_freshness_next_exe_export_json.sql",
    "compression" : "None",
    "output_filename" : "freshness_next_execution_data.json",
    "destination_format": "NEWLINE_DELIMITED_JSON",
    "copy_table" : false
}
```
