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

## 👁🗨 Example

Here is an example of STS configuration file for a GCS to SFTP transfer:

```text
{
  "configuration_type": "storage-to-storage",
  "configuration_id": "152-composer-test-gcs-to-sftp",
  "environment": "PROD",
  "account": "000010",
  "activated": true,
  "archived": false,
  "filename_templates": [
    {
      "filename_template": "{{FD_DATE}}_test_file.txt",
      "file_description": "This is a description for test_file.txt."
    },
    {
      "filename_template": "{{FD_DATE}}_other.txt",
      "file_description": "This is a description for other.txt."
    }
  ],
  "source": {
    "type": "gcs"
    "gcs_source_bucket" : "152-composer-test",
    "gcs_source_prefix" : "INPUT_SOMEDIR",
    "gcs_archive_prefix": "archive",
    "gcp_credentials_secret": {
      "cipher_aes": "b42724dcbbf6c3310aba89a0f106d1c4",
      "tag": "5c8816ea0a7aded9cb47c6f2df61f5b9",
      "ciphertext": "fdf09c6e",
      "enc_session_key": "8f63f7c"
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
        "cipher_aes": "3926f71cd00f8d2b812d10b07d0fee4e",
        "tag": "1f5c066351db5f91041343a2ab37aebe",
        "ciphertext": "921776fd0caa71a04228fe8aaa42af04",
        "enc_session_key": "2fb0ad2b0d0edf9771"
      },
      "sftp_destination_dir": "/",
      "sftp_destination_dir_create": false
    }
  ]
}
```

## 🌐 Global parameters

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
        <p>For an STS data operation, the value is always &quot;storage-to-storage&quot;.</p>
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
          <li>the source bucket name,</li>
          <li>and the source directory name.</li>
        </ul>
      </td>
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
        <p><b>filename_templates</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>List of filename templates that will be processed.</p>
        <p>You can set the value to &quot;*&quot; for all files to be copied. However,
          this is not recommended, as unnecessary or sensitive files might be included
          by mistake. Besides, the date value specified in <b>filename_template</b> will
          be used to sort files in the archive folder. If no date value is specified,
          all files will be stored together under one folder named <b>/ALL</b>.</p>
        <p>The best practice is to specify one or more filename templates with the <b>filename_template</b> and <b>file_description</b> parameters
          as described below.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>filename_template</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Template for the files to be processed.
          <br />The following placeholders are currently supported:</p>
        <ul>
          <li>&quot;FD_DATE&quot; looks for an 8-digit date (e.g. &quot;20191015&quot;).</li>
          <li>&quot;FD_TIME&quot; looks for a 6-digit time (e.g. &quot;124213&quot;).</li>
          <li>&quot;FD_BLOB_XYZ&quot;, where XYZ is a non-zero positive integer, looks
            for a string of characters of XYZ length.</li>
        </ul>
        <p><b>Example 1</b>
        </p>
        <p>This template:</p>
        <p><code>&quot;stores_{{FD_DATE}}_{{FD_TIME}}.txt&quot;</code>
        </p>
        <p>will allow you to process this type of files:</p>
        <p>&quot;stores_20201116_124213.txt&quot;</p>
        <p></p>
        <p><b>Example 2</b>
        </p>
        <p>This template:</p>
        <p><code>&quot;{{FD_DATE}}_{{FD_BLOB_5}}_fixedvalue_{{FD_BLOB_11}}.gz&quot;</code>
        </p>
        <p>will allow you to process this type of files:</p>
        <p>&quot;20201116_12397_fixedvalue_12312378934.gz&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>file_description</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Short description of the files that will match the filename template.</td>
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
          configuration and runs in Tailer&#xA0;Studio.</p>
        <p>If not specified, the default value will be &quot;false&quot;.</p>
      </td>
    </tr>
  </tbody>
</table>

## ⬇ Source parameters

There can only be one source block, as STS data operations can only process one source at a time.

### **Google Cloud Storage source**

Example:

```text
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

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>type</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Type of source.</p>
        <p>In this case : &quot;gcs&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcp_project_id</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Set the project where deploy the source configuration and associated cloud
          functions</p>
        <p>If not set, the user will be prompted to choose a profile where deploy
          the configuration</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_source_bucket</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Name of the source bucket.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_source_prefix</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Path where the files will be found, e.g. &quot;some/sub/dir&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_archive_prefix</b>
        </p>
        <p>type: string optional</p>
      </td>
      <td style="text-align:left">
        <p>Path where the source files will be archived.</p>
        <p>If present and populated, the STS data operation will archive the source
          files in the location specified, in the GCS source bucket.</p>
        <p>If not present or empty, there will be no archiving.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcp_credentials_secret</b>
        </p>
        <p>type: dict</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Encrypted credentials needed to read/move data from the source bucket.</p>
        <p>You should have generated credentials when <a href="../../getting-started/set-up-google-cloud-platform.md">setting up GCP</a>.
          To learn how to encrypt them, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p>
      </td>
    </tr>
  </tbody>
</table>

### **Amazon S3 source**

Example:

```text
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

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>type</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Type of source.</p>
        <p>In this case : &quot;s3&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>aws_access_key</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Amazon S3 access key ID.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>aws_access_key_secret</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Encrypted Amazon S3 access private key.</p>
        <p>This is needed to read/move data from the source bucket. To learn how
          to encrypt the private key value, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p>
      </td>
    </tr>
  </tbody>
</table>

### **SFTP source**

Example:

```text
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

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Parameter</b>
      </th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>type</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Type of source.</p>
        <p>In this case : &quot;sftp&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_source_filename</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">File to retrieve.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_source_directory</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Sub-path to switch to before downloading the file.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_host</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">SFTP host, e.g. &quot;sftp.something.com&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_port</b>
        </p>
        <p>type: integer</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">SFTP port, e.g. &quot;22&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_userid</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">SFTP user ID, e.g. &quot;john_doe&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_password_secret</b>
        </p>
        <p>type: dict</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Encrypted SFTP password for the user ID.</p>
        <p>This is needed to read/move data from the source SFTP.</p>
        <p>To learn how to encrypt the password, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p>
      </td>
    </tr>
  </tbody>
</table>

## ⬆ Destination parameters

These parameters allow you specify a list of destinations. You can add as many "destination" sub-objects as you want, they will all be processed.

### **Google Cloud Storage destination**

Example:

```text
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
        <p><b>type</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Type of destination.</p>
        <p>In this case : &quot;gcs&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_destination_bucket</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Google Cloud Storage destination bucket.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_destination_prefix</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Google Cloud Storage destination path, e.g. &quot;/subdir/subdir_2&quot;
        to send the files to &quot;gs://BUCKET/subdir/subdir_2/source_file.ext&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcp_credentials_secret</b>
        </p>
        <p>type: dict</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Encrypted credentials needed to read/write/move data from the destination
          bucket.</p>
        <p>You should have generated credentials when <a href="../../getting-started/set-up-google-cloud-platform.md">setting up GCP</a>.
          To learn how to encrypt them, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p>
      </td>
    </tr>
  </tbody>
</table>

### **Amazon S3 destination**

Example:

```text
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
        <p><b>type</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Type of destination.</p>
        <p>In this case : &quot;s3&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>s3_bucket</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Amazon S3 bucket name.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>s3_destination_prefix</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Amazon S3 destination path, e.g. &quot;subdir_A/subdir_B&quot; to send
        the files to &quot;http://bucket.s3.amazonaws.com/subdir_A/subdir_B/source_file.ext&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>aws_access_key</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Amazon S3 access key ID.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>aws_access_key_secret</b>
        </p>
        <p>type: dict</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Encrypted Amazon S3 access private key.</p>
        <p>This is needed to read/write/move data from the destination bucket.</p>
        <p>To learn how to encrypt the private key value, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p>
      </td>
    </tr>
  </tbody>
</table>

### **SFTP destination**

Example:

```text
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
        <p><b>type</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Type of destination.</p>
        <p>In this case : &quot;sftp&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>generate_top_file</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>This flag, if set, will generate a TOP file along with the file copied.</p>
        <p>Possible values are:</p>
        <ul>
          <li>&quot;REPLACE_EXTENSION&quot;: if the source file is &quot;20190708_data.txt&quot;,
            the TOP file will be &quot;20190708_data.top&quot;.</li>
          <li>&quot;ADD_EXTENSION&quot;: if the source file is &quot;20190708_data.txt&quot;,
            the TOP file will be &quot;20190708_data.txt.top&quot;.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_destination_dir</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Path to switch to before uploading the file.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_destination_dir_create</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Will try to create the subdir specified in <b>sftp_destination_dir</b> on
        the SFTP filesystem before switching to it and copying files.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_host</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">SFTP host, e.g. &quot;sftp.something.com&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_port</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">SFTP port, e.g. &quot;22&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_userid</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">SFTP user ID, e.g. &quot;john_doe&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sftp_password_secret</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Encrypted SFTP password for the user ID.</p>
        <p>This is needed to read/move data from the source SFTP.</p>
        <p>To learn how to encrypt the password, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p>
      </td>
    </tr>
  </tbody>
</table>

