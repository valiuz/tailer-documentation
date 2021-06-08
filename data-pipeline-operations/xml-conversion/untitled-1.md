---
description: >-
  This is the description of the JSON configuration file for a Convert XML to
  CSV data operation.
---

# Convert XML to CSV configuration file

The configuration file is in JSON format. It contains the following sections:

* [Global parameters](untitled-1.md#global-parameters): General information about the data operation.
* [Working folder parameters](untitled-1.md#working-folder-parameters): Information related to the input/output folder for the files.
* [Conversion parameters](untitled-1.md#conversion-parameters): Information about the input file to convert and the output files generated.

## 👁🗨 Example

Here is an example of Convert XML to CSV configuration file:

```text
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
        <p>For a Convert XML to CSV data operation, the value is always &quot;xml-conversion&quot;.</p>
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
          <li>&quot;xml-conversion&quot;.</li>
          <li>and a description of the data to convert.</li>
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
      <td style="text-align:left">Your account ID is a 6-digit number assigned to you by your Tailer Platform
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
          configuration and runs in Tailer&#xA0;Studio.</p>
        <p>If not specified, the default value will be &quot;false&quot;.</p>
      </td>
    </tr>
  </tbody>
</table>

## 💼 Working folder parameters

Information related to the input/output working directory in Google Cloud Storage.

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
        <p><b>gcp_project_id</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Google Cloud Platform project ID for the bucket where the data is going
        to be converted.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_bucket</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Name of the GCS bucket where the data is going to be converted.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_working_directory</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Path in the GCS bucket where the input files will be placed, and the output
        files generated, e.g. &quot;some/sub/dir&quot;.</td>
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

## 💱 Conversion parameters

Information about the input file to convert and the output files generated.

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
        <p><b>filename_templates</b>
        </p>
        <p>type: array</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Array containing one or several <b>filename_template</b> parameters (see
        below).</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>filename_template</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>When a file with a name matching the template set here is added to the
          specified GCS folder, the conversion will be launched automatically.</p>
        <p>
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
        <p>mandatory</p>
      </td>
      <td style="text-align:left">A short description of the file template entry.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>xsd_schema_file</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Name of the XSD file that will be used to validate the XML file before
          the conversion.</p>
        <p>In the current version, only one XSD can be used per XML entry.
          <br />The XSD file name <b>must be identical</b> to the corresponding XML file
          name, excluding suffixes. For example &quot;coupon.xsd&quot; can be used
          to validate &quot;coupon_20210404.xml&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>output_suffix_filter</b>
        </p>
        <p>type: array of string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Names of the output files that need to be kept after the conversion.</p>
        <p>If the XML file contains many child entities, the conversion will create
          a lot of CSV files (one for each entity). This filter allows you to prevent
          unnecessary file upload to the output bucket.</p>
        <p>It works by finding an occurrence of the string in the filename. For example,
          if after a conversion a file named &quot;coupon_20210404_advantage.tsv&quot;
          is generated and the filter &quot;advantage.tsv&quot; is added, then this
          file will be kept.</p>
      </td>
    </tr>
  </tbody>
</table>

