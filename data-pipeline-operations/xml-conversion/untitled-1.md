---
description: >-
  This is the description of the JSON configuration file for a Convert XML to
  CSV data operation.
---

# Convert XML to CSV configuration file

The configuration file is in JSON format. It contains the following sections:

* [Global parameters](untitled-1.md#global-parameters): General information about the data operation.
* [Working folder parameters](untitled-1.md#working-folder-parameters): Information related to the working folder for the files.
* [Conversion parameters](untitled-1.md#conversion-parameters): Information about the input file to process and the output files generated.

## :eye\_in\_speech\_bubble: Example

Here is an example of Convert XML to CSV configuration file:

```json
{
    "configuration_type": "xml-conversion",
    "configuration_id": "000099-test-xml-conversion",
    "environment": "DEV",
    "account": "000099",
    "activated": true,
    "archived": false,
    "gcp_project_id": "fd-io-jarvis-demo-dlk",
    "gcs_bucket": "fd-io-demo-ds",
    "gcs_working_directory": "test_xml_conversion",
    "credentials": {
        "gcp-credentials.json": {
            "content": {
                "cipher_aes": "dd34e56f4...",
                "tag": "3d968340...",
                "ciphertext": "046ffe41f9c00448ea0f816119...",
                "enc_session_key": "36a0f8ffe1b1f0..."
            }
        }
    },
    "filename_templates": [
        {
            "filename_template": "coupon_{{FD_DATE}}.xml",
            "file_description": "This is a description.",
            "xsd_schema_file": "coupon.xsd",
            "output_suffix_filters":[
                "advantage.tsv",
                "barcodeType.tsv"
            ]
        }
    ]
}
```

## :globe\_with\_meridians: Global parameters

General information about the data operation.

| Parameter                                                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>configuration_type</strong></p><p>type: string</p><p>mandatory</p> | <p>Type of data operation.</p><p>For a Convert XML to CSV data operation, the value is always "xml-conversion".</p>                                                                                                                                                                                                                                                                                                                                                 |
| <p><strong>configuration_id</strong></p><p>type: string</p><p>mandatory</p>   | <p>ID of the data operation.</p><p>You can pick any name you want, but is has to be <strong>unique</strong> for this data operation type.</p><p>Note that in case of conflict, the newly deployed data operation will overwrite the previous one. To guarantee its uniqueness, the best practice is to name your data operation by concatenating:</p><ul><li>your account ID,</li><li>"xml-conversion".</li><li>and a description of the data to convert.</li></ul> |
| <p><strong>environment</strong></p><p>type: string</p><p>mandatory</p>        | <p>Deployment context.</p><p>Values: PROD, PREPROD, STAGING, DEV.</p>                                                                                                                                                                                                                                                                                                                                                                                               |
| <p><strong>account</strong></p><p>type: string</p><p>mandatory</p>            | Your account ID is a 6-digit number assigned to you by your Tailer Platform administrator.                                                                                                                                                                                                                                                                                                                                                                          |
| <p><strong>activated</strong></p><p>type: boolean</p><p>optional</p>          | <p>Flag used to enable/disable the execution of the data operation.</p><p>If not specified, the default value will be "true".</p>                                                                                                                                                                                                                                                                                                                                   |
| <p><strong>archived</strong></p><p>type: boolean</p><p>optional</p>           | <p>Flag used to enable/disable the visibility of the data operation's configuration and runs in Tailer Studio.</p><p>If not specified, the default value will be "false".</p>                                                                                                                                                                                                                                                                                       |

## :briefcase: Working folder parameters

Information related to the input/output working directory in Google Cloud Storage.

| Parameter                                                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                            |
| -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>gcp_project_id</strong></p><p>type: string</p><p>mandatory</p>        | Google Cloud Platform project ID for the bucket where the data is going to be converted.                                                                                                                                                                                                                                                                                                                               |
| <p><strong>gcs_bucket</strong></p><p>type: string</p><p>mandatory</p>            | Name of the GCS bucket where the data is going to be converted.                                                                                                                                                                                                                                                                                                                                                        |
| <p><strong>gcs_working_directory</strong></p><p>type: string</p><p>mandatory</p> | Path in the GCS bucket where the input files will be placed, and the output files generated, e.g. "some/sub/dir".                                                                                                                                                                                                                                                                                                      |
| <p><strong>gcp_credentials_secret</strong></p><p>type: dict</p><p>mandatory</p>  | <p>Encrypted credentials needed to read and write data in the GCS bucket.</p><p>You should have generated credentials when <a href="https://app.gitbook.com/s/-MIIsP_DvP2J-c1szWrQ/getting-started/set-up-google-cloud-platform.md">setting up GCP</a>. To learn how to encrypt them, refer to <a href="https://app.gitbook.com/s/-MIIsP_DvP2J-c1szWrQ/getting-started/encrypt-your-credentials.md">this page</a>.</p> |

## :currency\_exchange: Conversion parameters

Information about the input file to process and the output files generated.

| Parameter                                                                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>filename_templates</strong></p><p>type: array</p><p>mandatory</p>             | Array containing one or several **filename\_template** parameters (see below).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| <p><strong>filename_template</strong></p><p>type: string</p><p>mandatory</p>             | <p>When a file with a name matching the template set here is added to the specified GCS folder, the conversion will be launched automatically.</p><p><br>The following placeholders are currently supported:</p><ul><li>"FD_DATE" looks for an 8-digit date (e.g. "20191015").</li><li>"FD_TIME" looks for a 6-digit time (e.g. "124213").</li><li>"FD_BLOB_XYZ", where XYZ is a non-zero positive integer, looks for a string of characters of XYZ length.</li></ul><p><strong>Example 1</strong></p><p>This template:</p><p><code>"stores_{{FD_DATE}}</code><em><code>{{FD_TIME}}.txt"</code></em></p><p><em>will allow you to process this type of files:</em></p><p><em>"stores_20201116_124213.txt"</em></p><p><em><strong>Example 2</strong></em></p><p><em>This template:</em></p><p><em><code>"{{FD_DATE}}</code></em><code>{{FD_BLOB_5}}</code><em><code>fixedvalue</code></em><code>{{FD_BLOB_11}}.gz"</code></p><p>will allow you to process this type of files:</p><p>"20201116_12397_fixedvalue_12312378934.gz"</p> |
| <p><strong>file_description</strong></p><p>type: string</p><p>mandatory</p>              | A short description of the file template entry.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| <p><strong>xsd_schema_file</strong></p><p>type: string</p><p>mandatory</p>               | <p>Name of the XSD file that will be used to validate the XML file before the conversion.</p><p>In the current version, only one XSD can be used per XML entry.<br>The XSD file name <strong>must be identical</strong> to the corresponding XML file name, excluding suffixes. For example "coupon.xsd" can be used to validate "coupon_20210404.xml".</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <p><strong>output_suffix_filters</strong></p><p>type: array of string</p><p>optional</p> | <p>Names of the output files to be kept after the conversion.</p><p>If the XML file contains many child entities, the conversion will create a lot of CSV files (one for each entity). This filter allows you to prevent unnecessary file upload to the output bucket.</p><p>It works by finding an occurrence of the string in the filename. For example, if a file named "coupon_20210404_advantage.tsv" is generated and the filter "advantage.tsv" was added, then this file will be kept.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
