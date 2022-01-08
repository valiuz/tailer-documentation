---
description: >-
  This is the description of the JSON configuration file of a Storage to Storage
  data operation.
---

# Storage to Storage configuration file

The configuration file is in JSON format. It contains the following sections:

* Global parameters: General information about the data operation.
* Source parameters: One source block, containing information about the data source.
* Destination parameters: One or several destination blocks, containing information about the data destinations.

## :eye\_in\_speech\_bubble: Example

Here is an example of STS configuration file for a GCS to SFTP transfer:

```json
{
  "$schema": "http://jsonschema.tailer.ai/schema/storage-to-storage",
  "version": "2",
  
  "configuration_type": "storage-to-storage",
  "configuration_id": "copy-my-files-gcs-to-sftp",
  
  "environment": "PROD",
  "account": "000099",
  
  "activated": true,
  "archived": false,
  
  "max_active_runs": 5,
  
  "filename_templates": [
    {
      "filename_template": "{{FD_DATE}}_sales_file.txt",
      "file_description": "This is a description for sales_file.txt."
    },
    {
      "filename_template": "{{FD_DATE}}_products.txt",
      "file_description": "This is a description for proucts.txt."
    }
  ],
  
  "source": {
    "type": "gcs"
    "gcs_source_bucket" : "my-input-bucket",
    "gcs_source_prefix" : "input-folder",
    "gcs_archive_prefix": "archive-folder",
    "gcp_credentials_secret": {
      "cipher_aes": "b42724dcbbf0aba89a0f106d1c4",
      "tag": "5c8816ea0a7aded7c6f2df61f5b9",
      "ciphertext": "fd096e",
      "enc_session_key": "8f6f7c"
    }
  },
  
  "destinations": [
    {
      "type": "sftp",
      "generate_top_file": "REPLACE_EXTENSION",
      "sftp_host": "sftp.domain.com",
      "sftp_port": 22,
      "sftp_userid": "john_doe",
      "sftp_password_secret": {
        "cipher_aes": "3926f71cd00d10b07d0fee4e",
        "tag": "1f5c066351d91041343a2ab37aebe",
        "ciphertext": "921776fd04228fe8aaa42af04",
        "enc_session_key": "2fb0ad2b0df9771"
      },
      "sftp_destination_dir": "/",
      "sftp_destination_dir_create": false
    }
  ]
}
```

## :globe\_with\_meridians: Global parameters

| Parameter                                                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>configuration_type</strong></p><p>type: string</p><p>mandatory</p> | <p>Type of data operation.</p><p>For an STS data operation, the value is always "storage-to-storage".</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p><strong>configuration_id</strong></p><p>type: string</p><p>mandatory</p>   | <p>ID of the data operation.</p><p>You can pick any name you want, but is has to be <strong>unique</strong> for this data operation type.</p><p>Note that in case of conflict, the newly deployed data operation will overwrite the previous one. To guarantee its uniqueness, the best practice is to name your data operation by concatenating:</p><ul><li>your account ID,</li><li>the source bucket name,</li><li>and the source directory name.</li></ul>                                                                                                                                                                                                                                            |
| <p><strong>environment</strong></p><p>type: string</p><p>mandatory</p>        | <p>Deployment context.</p><p>Values: PROD, PREPROD, STAGING, DEV.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>account</strong></p><p>type: string</p><p>mandatory</p>            | Your account ID is a 6-digit number assigned to you by your Tailer Platform administrator.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| <p><strong>filename_templates</strong></p><p>type: string</p><p>mandatory</p> | <p>List of filename templates that will be processed.</p><p></p><p>You can set the value to "*" for all files to be copied. However, this is <strong>not recommended</strong>, as unnecessary or sensitive files might be included by mistake. Besides, the date value specified in <strong>filename_template</strong> will be used to sort files in the archive folder. If no date value is specified, all files will be stored together under one folder named <strong>/ALL</strong>.</p><p></p><p>The best practice is to specify one or more filename templates with the <strong>filename_template</strong> and <strong>file_description</strong> parameters as described in the next paragraphe.</p> |
| <p><strong>activated</strong></p><p>type: boolean</p><p>optional</p>          | <p>Flag used to enable/disable the execution of the data operation.</p><p>If not specified, the default value will be "true".</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| <p><strong>archived</strong></p><p>type: boolean</p><p>optional</p>           | <p>Flag used to enable/disable the visibility of the data operation's configuration and runs in Tailer Studio.</p><p>If not specified, the default value will be "false".</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| <p><strong>max_active_runs</strong></p><p>type: integer</p><p>optional</p>    | <p>This parameter limits the number of concurrent runs for this data operation.</p><p></p><p>If not set, the default value is 5.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <p><strong>short_description</strong></p><p>type: string</p><p>optional</p>   | Short description of the Data Operation                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| <p><strong>doc_md</strong></p><p>type: string</p><p>optional</p>              | Path to a file containing a detailed description. The file must be in Markdown format.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

### **"Filename Templates" sub-object parameters**

The "**filename\_templates**" object contains the definition of expected source files to copy to the destinations.

| Parameter                                                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>filename_template</strong></p><p>type: string</p><p>optional</p> | <p>Template for the files to be processed.<br>The following placeholders are currently supported:</p><ul><li>"FD_DATE" looks for an 8-digit date (e.g. "20191015").</li><li>"FD_DATE_YEAR_4" looks for 4-digit year (e.g "2021").</li><li>"FD_DATE_YEAR_2" looks for 2-digit year (e.g "21").</li><li>"FD_DATE_MONTH" looks for 2-digit month (e.g "05").</li><li>"FD_DATE_DAY" looks for 2-digit day (e.g "12").</li><li>"FD_TIME" looks for a 6-digit time (e.g. "124213").</li><li>"FD_BLOB_XYZ", where XYZ is a non-zero positive integer, looks for a string of characters of XYZ length.</li></ul><p><strong>Information:</strong></p><ul><li>if "FD_DATE" is specified, it will have priority upon "FD_DATE_YEAR_X".</li><li>if "FD_DATE_YEAR_4" or "FD_DATE_YEAR_2" is specified, the final date will be concatenated with "FD_DATE_MONTH" and "FD_DATE_DAY".</li><li>if "FD_DATE_YEAR_2" is specified, it will be prefixed by "20".</li><li>if "FD_DATE_YEAR_4" or "FD_DATE_YEAR_2" is specified only "FD_DATE_MONTH" and "FD_DATE_DAY" will be set to "01".</li></ul><p><strong>Example 1</strong></p><p>This template:</p><p><code>"stores_{{FD_DATE}}</code><em><code>{{FD_TIME}}.txt"</code></em></p><p><em>will allow you to process this type of files:</em></p><p><em>"stores_20201116_124213.txt"</em></p><p><em><strong>Example 2</strong></em></p><p><em>This template:</em></p><p><em><code>"{{FD_DATE}}_</code></em><code>{{FD_BLOB_5}}_</code><em><code>fixedvalue_</code></em><code>{{FD_BLOB_11}}.gz"</code></p><p>will allow you to process this type of files:</p><p>"20201116_12397_fixedvalue_12312378934.gz"</p> |
| <p><strong>file_description</strong></p><p>type: string</p><p>optional</p>  | Short description of the files that will match the filename template.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

## :arrow\_down: Source parameters

There can only be one source block, as STS data operations can only process one source at a time.

### **Google Cloud Storage source**

Example:

```
{
  "source": {
    "type": "gcs",
    "gcp_project_id": "my_gcp_project",
    "gcs_source_bucket" : "152-composer-test",
    "gcs_source_prefix" : "INPUT_SOMEDIR",
    "gcs_archive_prefix": "archive",
    "gcp_credentials_secret": {
      "cipher_aes": "b42724dcbbf6c3310aba89a0f106d1c4",
      "tag": "5c8816ea0a7aded9cb47c6f2df61f5b9",
      "ciphertext": "fdf09c6e",
      "enc_session_key": "8f63f7c"
    }
  }
}
```

| Parameter                                                                       | **Description**                                                                                                                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p><strong>type</strong></p><p>type: string</p><p>mandatory</p>                 | <p>Type of source.</p><p>In this case : "gcs".</p>                                                                                                                                                                                                                                                                                   |
| <p><strong>gcp_project_id</strong></p><p>type: string</p><p>mandatory</p>       | <p>Set the project where deploy the source configuration and associated cloud functions</p><p>If not set, the user will be prompted to choose a profile where deploy the configuration</p>                                                                                                                                           |
| <p><strong>gcs_source_bucket</strong></p><p>type: string</p><p>mandatory</p>    | Name of the source bucket.                                                                                                                                                                                                                                                                                                           |
| <p><strong>gcs_source_prefix</strong></p><p>type: string</p><p>mandatory</p>    | Path where the files will be found, e.g. "some/sub/dir".                                                                                                                                                                                                                                                                             |
| <p><strong>gcs_archive_prefix</strong></p><p>type: string optional</p>          | <p>Path where the source files will be archived.</p><p>If present and populated, the STS data operation will archive the source files in the location specified, in the GCS source bucket.</p><p>If not present or empty, there will be no archiving.</p>                                                                            |
| <p><strong>gcp_credentials_secret</strong></p><p>type: dict</p><p>mandatory</p> | <p>Encrypted credentials needed to read/move data from the source bucket.</p><p>You should have generated credentials when <a href="../../getting-started/set-up-google-cloud-platform.md">setting up GCP</a>. To learn how to encrypt them, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p> |

### **Amazon S3 source**

Example:

```
{
  "source": {
    "type": "s3",
    "aws_access_key": "3VJ3F6JJQBA2",
    "aws_access_key_secret": {
      "cipher_aes": "e6f5a68d4de8af89e83ea93e42facbed",
      "tag": "20e174e34c5d0c537be77d85ed8dda33",
      "ciphertext": "60a98b884110aab84",
      "enc_session_key": "9c4648e"
    }
  }
}
```

| Parameter                                                                        | **Description**                                                                                                                                                                                                                                  |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p><strong>type</strong></p><p>type: string</p><p>mandatory</p>                  | <p>Type of source.</p><p>In this case : "s3".</p>                                                                                                                                                                                                |
| <p><strong>aws_access_key</strong></p><p>type: string</p><p>mandatory</p>        | Amazon S3 access key ID.                                                                                                                                                                                                                         |
| <p><strong>aws_access_key_secret</strong></p><p>type: string</p><p>mandatory</p> | <p>Encrypted Amazon S3 access private key.</p><p>This is needed to read/move data from the source bucket. To learn how to encrypt the private key value, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p> |

### **SFTP source**

Example:

```
{
  "source": {
    "type": "sftp",
    "sftp_source_filename": "20190621_test_file.txt",
    "sftp_source_directory": "/",
    "sftp_host": "sftp.domain.com",
    "sftp_port": 22,
    "sftp_userid": "john_doe",
    "sftp_password_secret": {
      "cipher_aes": "3926f71cd00f8d2b812d10b07d0fee4e",
      "tag": "1f5c066351db5f91041343a2ab37aebe",
      "ciphertext": "921776fd0caa71a04228fe8aaa42af04",
      "enc_session_key": "2fb2f8d271"
    }
  }
}
```

| **Parameter**                                                                    | Description                                                                                                                                                                                                                                  |
| -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>type</strong></p><p>type: string</p><p>mandatory</p>                  | <p>Type of source.</p><p>In this case : "sftp".</p>                                                                                                                                                                                          |
| <p><strong>sftp_source_filename</strong></p><p>type: string</p><p>mandatory</p>  | File to retrieve.                                                                                                                                                                                                                            |
| <p><strong>sftp_source_directory</strong></p><p>type: string</p><p>mandatory</p> | Sub-path to switch to before downloading the file.                                                                                                                                                                                           |
| <p><strong>sftp_host</strong></p><p>type: string</p><p>mandatory</p>             | SFTP host, e.g. "sftp.something.com".                                                                                                                                                                                                        |
| <p><strong>sftp_port</strong></p><p>type: integer</p><p>mandatory</p>            | SFTP port, e.g. "22".                                                                                                                                                                                                                        |
| <p><strong>sftp_userid</strong></p><p>type: string</p><p>mandatory</p>           | SFTP user ID, e.g. "john\_doe".                                                                                                                                                                                                              |
| <p><strong>sftp_password_secret</strong></p><p>type: dict</p><p>mandatory</p>    | <p>Encrypted SFTP password for the user ID.</p><p>This is needed to read/move data from the source SFTP.</p><p>To learn how to encrypt the password, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p> |

## :arrow\_up: Destination parameters

These parameters allow you specify a list of destinations. You can add as many "destination" sub-objects as you want, they will all be processed.

### **Google Cloud Storage destination**

Example:

```
{
  "destinations": [
    {
      "type": "gcs",
      "gcs_destination_bucket": "152-composer-test",
      "gcs_destination_prefix": "JULTEST",
      "gcp_credentials_secret": {
        "cipher_aes": "b42724dcbbf6c3310aba89a0f106d1c4",
        "tag": "5c8816ea0a7aded9cb47c6f2df61f5b9",
        "ciphertext": "fdf09c6e",
        "enc_session_key": "8f634f7f7c"
      }
    }
  ]
}
```

| Parameter                                                                         | Description                                                                                                                                                                                                                                                                                                                                     |
| --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>type</strong></p><p>type: string</p><p>mandatory</p>                   | <p>Type of destination.</p><p>In this case : "gcs".</p>                                                                                                                                                                                                                                                                                         |
| <p><strong>gcs_destination_bucket</strong></p><p>type: string</p><p>mandatory</p> | Google Cloud Storage destination bucket.                                                                                                                                                                                                                                                                                                        |
| <p><strong>gcs_destination_prefix</strong></p><p>type: string</p><p>mandatory</p> | Google Cloud Storage destination path, e.g. "/subdir/subdir\_2" to send the files to "gs://BUCKET/subdir/subdir\_2/source\_file.ext"                                                                                                                                                                                                            |
| <p><strong>gcp_credentials_secret</strong></p><p>type: dict</p><p>mandatory</p>   | <p>Encrypted credentials needed to read/write/move data from the destination bucket.</p><p>You should have generated credentials when <a href="../../getting-started/set-up-google-cloud-platform.md">setting up GCP</a>. To learn how to encrypt them, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p> |

### **Amazon S3 destination**

Example:

```
{
  "destinations": [
    {
      "type": "s3",
      "s3_bucket" : "fd-io-exc-ysance-n-in",
      "s3_destination_prefix": "TEST1",
      "aws_access_key": "J3F6JLUVJQ",
      "aws_access_key_secret": {
        "cipher_aes": "e6f5a68d4de8af89e83ea93e42facbed",
        "tag": "20e174e34c5d0c537be77d85ed8dda33",
        "ciphertext": "60a84",
        "enc_session_key": "9c4619048e"
      }
    }
  ]
}
```

| Parameter                                                                        | Description                                                                                                                                                                                                                                                       |
| -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>type</strong></p><p>type: string</p><p>mandatory</p>                  | <p>Type of destination.</p><p>In this case : "s3".</p>                                                                                                                                                                                                            |
| <p><strong>s3_bucket</strong></p><p>type: string</p><p>mandatory</p>             | Amazon S3 bucket name.                                                                                                                                                                                                                                            |
| <p><strong>s3_destination_prefix</strong></p><p>type: string</p><p>mandatory</p> | Amazon S3 destination path, e.g. "subdir\_A/subdir\_B" to send the files to "http://bucket.s3.amazonaws.com/subdir\_A/subdir\_B/source\_file.ext".                                                                                                                |
| <p><strong>aws_access_key</strong></p><p>type: string</p><p>mandatory</p>        | Amazon S3 access key ID.                                                                                                                                                                                                                                          |
| <p><strong>aws_access_key_secret</strong></p><p>type: dict</p><p>mandatory</p>   | <p>Encrypted Amazon S3 access private key.</p><p>This is needed to read/write/move data from the destination bucket.</p><p>To learn how to encrypt the private key value, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p> |

### **SFTP destination**

Example:

```
{
  "source": [
    {
      "type": "sftp",
      "generate_top_file": "REPLACE_EXTENSION",
      "sftp_host": "sftp.domain.com",
      "sftp_port": 22,
      "sftp_userid": "john_doe",
      "sftp_password_secret": {
        "cipher_aes": "3926f71cd00f8d2b812d10b07d0fee4e",
        "tag": "1f5c066351db5f91041343a2ab37aebe",
        "ciphertext": "921776fd0caa71a04228fe8aaa42af04",
        "enc_session_key": "2fb0adf8d271"
      },
      "sftp_destination_dir": "/",
      "sftp_destination_dir_create": false
    }
  ]
}
```

| Parameter                                                                              | Description                                                                                                                                                                                                                                                                                                                                            |
| -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p><strong>type</strong></p><p>type: string</p><p>mandatory</p>                        | <p>Type of destination.</p><p>In this case : "sftp".</p>                                                                                                                                                                                                                                                                                               |
| <p><strong>generate_top_file</strong></p><p>type: string</p><p>mandatory</p>           | <p>This flag, if set, will generate a TOP file along with the file copied.</p><p>Possible values are:</p><ul><li>"REPLACE_EXTENSION": if the source file is "20190708_data.txt", the TOP file will be "20190708_data.top".</li><li>"ADD_EXTENSION": if the source file is "20190708_data.txt", the TOP file will be "20190708_data.txt.top".</li></ul> |
| <p><strong>sftp_destination_dir</strong></p><p>type: string</p><p>mandatory</p>        | Path to switch to before uploading the file.                                                                                                                                                                                                                                                                                                           |
| <p><strong>sftp_destination_dir_create</strong></p><p>type: string</p><p>mandatory</p> | Will try to create the subdir specified in **sftp\_destination\_dir** on the SFTP filesystem before switching to it and copying files.                                                                                                                                                                                                                 |
| <p><strong>sftp_host</strong></p><p>type: string</p><p>mandatory</p>                   | SFTP host, e.g. "sftp.something.com".                                                                                                                                                                                                                                                                                                                  |
| <p><strong>sftp_port</strong></p><p>type: string</p><p>mandatory</p>                   | SFTP port, e.g. "22".                                                                                                                                                                                                                                                                                                                                  |
| <p><strong>sftp_userid</strong></p><p>type: string</p><p>mandatory</p>                 | SFTP user ID, e.g. "john\_doe".                                                                                                                                                                                                                                                                                                                        |
| <p><strong>sftp_password_secret</strong></p><p>type: string</p><p>mandatory</p>        | <p>Encrypted SFTP password for the user ID.</p><p>This is needed to read/move data from the source SFTP.</p><p>To learn how to encrypt the password, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p>                                                                                                           |
