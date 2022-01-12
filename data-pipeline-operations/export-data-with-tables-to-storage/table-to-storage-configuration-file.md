---
description: >-
  This is the description of the JSON configuration file of a Table to Storage
  data operation.
---

# Table to Storage configuration file

The configuration file is in JSON format. It contains the following sections:

* Global parameters: General information about the data operation.
* Table copy parameters: Optionally, you can add a creation step for a table that will contain the result of the extraction.

## :eye\_in\_speech\_bubble: Example

Here is an example of TTS configuration file:

```
{
  "configuration_type": "table-to-storage",
  "configuration_id": "tts-some-id-example",
  "short_description" : "Short description of the job",
  "environment": "DEV",
  "account": "000111",
  "activated": true,
  "archived": false,
  "gcs_dest_bucket": "152-composer-test",
  "gcs_dest_prefix": "jultest_table_to_storage/",
  "gcp_project": "fd-tailer-datalake",
  "field_delimiter": "|",
  "print_header": true,
  "sql_file": "jul_test.sql",
  "compression": "None",
  "output_filename": "{{FD_DATE}}_some_file_name.csv",
  "copy_table": false,
  "dest_gcp_project_id": "GCP Project ID used if copy_table is true",
  "dest_gbq_dataset": "GBQ Dataset used if copy_table is true",
  "dest_gbq_table": "GBQ Table name used if copy_table is true",
  "dest_gbq_table_suffix": "dag_execution_date",
  "delete_dest_bucket_content": true
}
```

## :globe\_with\_meridians: Global parameters

General information about the data operation.

| Parameter                                                                             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>configuration_type</strong></p><p>type: string</p><p>mandatory</p>         | <p>Type of data operation.</p><p>For a TTS data operation, the value is always "table-to-storage".</p>                                                                                                                                                                                                                                                                                                                                                                |
| <p><strong>configuration_id</strong></p><p>type: string</p><p>mandatory</p>           | <p>ID of the data operation.</p><p>You can pick any name you want, but is has to be <strong>unique</strong> for this data operation type.</p><p>Note that in case of conflict, the newly deployed data operation will overwrite the previous one. To guarantee its uniqueness, the best practice is to name your data operation by concatenating:</p><ul><li>your account ID,</li><li>the word "extract",</li><li>and a description of the data to extract.</li></ul> |
| <p><strong>short_description</strong></p><p>type: string</p><p>optional</p>           | Short description of the table to storage data operation.                                                                                                                                                                                                                                                                                                                                                                                                             |
| <p><strong>environment</strong></p><p>type: string</p><p>mandatory</p>                | <p>Deployment context.</p><p>Values: PROD, PREPROD, STAGING, DEV.</p>                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p><strong>account</strong></p><p>type: string</p><p>mandatory</p>                    | Your account ID is a 6-digit number assigned to you by your Tailer Platform administrator.                                                                                                                                                                                                                                                                                                                                                                            |
| <p><strong>activated</strong></p><p>type: boolean</p><p>optional</p>                  | <p>Flag used to enable/disable the execution of the data operation.</p><p>If not specified, the default value will be "true".</p>                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>archived</strong></p><p>type: boolean</p><p>optional</p>                   | <p>Flag used to enable/disable the visibility of the data operation's configuration and runs in Tailer¯Studio.</p><p>If not specified, the default value will be "false".</p>                                                                                                                                                                                                                                                                                         |
| <p><strong>gcs_dest_bucket</strong></p><p>type: string</p><p>mandatory</p>            | <p>Google Cloud Storage destination bucket.</p><p>This is the bucket where the data is going to be extracted.</p>                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>gcs_dest_prefix</strong></p><p>type: string</p><p>mandatory</p>            | Path in the GCS bucket where the files will be extracted, e.g. "some/sub/dir".                                                                                                                                                                                                                                                                                                                                                                                        |
| <p><strong>delete_dest_bucket_content</strong></p><p>type: boolean</p><p>optional</p> | <p>If set to true, this parameter will trigger the deletion of any items present in the destination directory.</p><p>Default value: false</p>                                                                                                                                                                                                                                                                                                                         |
| <p><strong>gcp_project</strong></p><p>type: string</p><p>mandatory</p>                | ID of the Google Cloud project containing the BigQuery instance.                                                                                                                                                                                                                                                                                                                                                                                                      |
| <p><strong>field_delimiter</strong></p><p>type: string</p><p>optional</p>             | <p>Separator for fields in the CSV output file, e.g. ";".</p><p><strong>Note</strong>: For Tab separator, set to "\t".</p><p>Default value: "</p>                                                                                                                                                                                                                                                                                                                     |
| <p><strong>print_header</strong></p><p>type: boolean</p><p>optional</p>               | <p>Print a header row in the exported data.</p><p>Default value: true</p>                                                                                                                                                                                                                                                                                                                                                                                             |
| <p><strong>sql_file</strong></p><p>type: string</p><p>mandatory</p>                   | Path to the file containing the extraction query.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>compression</strong></p><p>type: string</p><p>optional</p>                 | <p>Compression mode for the output file.</p><p>Default value: "None"</p><p>Possible values: "None", "GZIP"</p><p>Note that if you specify "GZIP", a ".gz" extension will be added at the end of the filename.</p>                                                                                                                                                                                                                                                     |
| <p><strong>output_filename</strong></p><p>type: string</p><p>mandatory</p>            | <p>Template for the output filename.</p><p>You can use the following placeholders inside the name:</p><ul><li>{{FD_DATE}}: The date format will be YYYYMMDD</li><li>{{FD_TIME}}: The time format will be hhmmss</li></ul><p>For example, "{{FD_DATE}}<em>{{FD</em>TIME}}_my_data_extraction.csv"</p>                                                                                                                                                                  |
| <p><strong>destination_format</strong></p><p>type: string</p><p>optional</p>          | <p>Define the format of the output file :</p><p>Default value: "CSV"</p><p>Possible values: "NEWLINE_DELIMITED_JSON" (JSON file), "AVRO"</p><p>Note that if you specify "NEWLINE_DELIMITED_JSON", the field-delimiter parameter is not taken into account.</p>                                                                                                                                                                                                        |

## :two\_men\_holding\_hands: Table copy parameters

If you want to create a copy of your output data in a BigQuery table, you need to set the following parameters.

| Parameter                                                                                                                 | Description                                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>copy_table</strong></p><p>type: boolean</p><p>optional</p>                                                     | <p>Parameter used to enable a copy of the output data in a BigQuery table.</p><p>Default value: "false"</p>                                                                     |
| <p><strong>dest_gcp_project_id</strong></p><p>mandatory if <strong>copy_table</strong> is set to "true"</p>               | ID of the GCP project that will contain the table copy.                                                                                                                         |
| <p><strong>dest_gbq_dataset</strong></p><p>mandatory if <strong>copy_table</strong> is set to "true"</p>                  | Name of the BigQuery dataset that will contain the table copy.                                                                                                                  |
| <p><strong>dest_gbq_table</strong></p><p>mandatory if <strong>copy_table</strong> is set to "true"</p>                    | Name of the BigQuery table copy.                                                                                                                                                |
| <p><strong>dest_gbq_table_suffix</strong></p><p>optional, to use only if <strong>copy_table</strong> is set to "true"</p> | <p>The only supported value for this parameter is "dag_execution_date".</p><p>This will add "_yyyymmdd" at the end of the table name to enable ingestion time partitioning.</p> |
