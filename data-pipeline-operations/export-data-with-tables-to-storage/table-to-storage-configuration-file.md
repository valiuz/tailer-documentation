---
description: >-
  This is the description of the JSON configuration file of a Table to Storage
  data operation.
---

# Table to Storage configuration file

The configuration file is in JSON format. It contains the following sections:

* Global parameters: General information about the data operation.
* Table copy parameters: Optionally, you can add a creation step for a table that will contain the result of the extraction.

## 👁🗨 Example

Here is an example of TTS configuration file:

```text
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
  "output_filename": "{{DATE}}_some_file_name.csv",
  "copy_table": false,
  "dest_gcp_project_id": "GCP Project ID used if copy_table is true",
  "dest_gbq_dataset": "GBQ Dataset used if copy_table is true",
  "dest_gbq_table": "GBQ Table name used if copy_table is true",
  "dest_gbq_table_suffix": "dag_execution_date",
  "delete_dest_bucket_content": true
}
```

## 🌐 Global parameters

General information about the data operation.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>configuration_type</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Type of data operation.</p>
        <p>For a TTS data operation, the value is always &quot;table-to-storage&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>configuration_id</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>ID of the data operation.</p>
        <p>You can pick any name you want, but is has to be <b>unique</b> for this
          data operation type.</p>
        <p>Note that in case of conflict, the newly deployed data operation will
          overwrite the previous one. To guarantee its uniqueness, the best practice
          is to name your data operation by concatenating:</p>
        <ul>
          <li>your account ID,</li>
          <li>the word &quot;extract&quot;,</li>
          <li>and a description of the data to extract.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>short_description</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Short description of the table to storage data operation.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>environment</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Deployment context.</p>
        <p>Values: PROD, PREPROD, STAGING, DEV.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>account</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Your account ID is a 6-digit number assigned to you by your Tailer&#xA0;Platform
        administrator.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>activated</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Flag used to enable/disable the execution of the data operation.</p>
        <p>If not specified, the default value will be &quot;true&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>archived</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Flag used to enable/disable the visibility of the data operation&apos;s
          configuration and runs in Tailer&#xAF;Studio.</p>
        <p>If not specified, the default value will be &quot;false&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_dest_bucket</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Google Cloud Storage destination bucket.</p>
        <p>This is the bucket where the data is going to be extracted.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_dest_prefix</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Path in the GCS bucket where the files will be extracted, e.g. &quot;some/sub/dir&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>delete_dest_bucket_content</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>If set to true, this parameter will trigger the deletion of any items
          present in the destination directory.</p>
        <p>Default value: false</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcp_project</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">ID of the Google Cloud project containing the BigQuery instance.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>field_delimiter</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Separator for fields in the CSV output file, e.g. &quot;;&quot;.</p>
        <p><b>Note</b>: For Tab separator, set to &quot;\t&quot;.</p>
        <p>Default value: &quot;|&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>print_header</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Print a header row in the exported data.</p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Default value: true</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sql_file</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Path to the file containing the extraction query.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>compression</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Compression mode for the output file.</p>
        <p>Default value: &quot;None&quot;</p>
        <p>Possible values: &quot;None&quot;, &quot;GZIP&quot;</p>
        <p>Note that if you specify &quot;GZIP&quot;, a &quot;.gz&quot; extension
          will be added at the end of the filename.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>output_filename</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Template for the output filename.</p>
        <p>You can use the following placeholders inside the name:</p>
        <ul>
          <li>{{DATE}}: The date format will be YYYYMMDD</li>
          <li>{{TIME}}: The time format will be hhmmss</li>
        </ul>
        <p>For example, &quot;{{DATE}}_{{TIME}}_my_data_extraction.csv&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>destination_format</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Define the format of the output file :</p>
        <p>Default value: &quot;CSV&quot;</p>
        <p>Possible values: &quot;NEWLINE_DELIMITED_JSON&quot; (JSON file), &quot;AVRO&quot;</p>
        <p>Note that if you specify &quot;NEWLINE_DELIMITED_JSON&quot;, the field-delimiter
          parameter is not taken into account.</p>
      </td>
    </tr>
  </tbody>
</table>

## 👬 Table copy parameters

If you want to create a copy of your output data in a BigQuery table, you need to set the following parameters.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>copy_table</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Parameter used to enable a copy of the output data in a BigQuery table.</p>
        <p>Default value: &quot;false&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>dest_gcp_project_id</b>
        </p>
        <p>mandatory if <b>copy_table</b> is set to &quot;true&quot;</p>
      </td>
      <td style="text-align:left">ID of the GCP project that will contain the table copy.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>dest_gbq_dataset</b>
        </p>
        <p>mandatory if <b>copy_table</b> is set to &quot;true&quot;</p>
      </td>
      <td style="text-align:left">Name of the BigQuery dataset that will contain the table copy.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>dest_gbq_table</b>
        </p>
        <p>mandatory if <b>copy_table</b> is set to &quot;true&quot;</p>
      </td>
      <td style="text-align:left">Name of the BigQuery table copy.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>dest_gbq_table_suffix</b>
        </p>
        <p>optional, to use only if <b>copy_table</b> is set to &quot;true&quot;</p>
      </td>
      <td style="text-align:left">
        <p>The only supported value for this parameter is &quot;dag_execution_date&quot;.</p>
        <p>This will add &quot;_yyyymmdd&quot; at the end of the table name to enable
          ingestion time partitioning.</p>
      </td>
    </tr>
  </tbody>
</table>

