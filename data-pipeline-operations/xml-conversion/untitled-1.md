---
description: >-
  This is the description of the JSON configuration file for a Convert XML to
  CSV data operation.
---

# Convert XML to CSV configuration file

The configuration file is in JSON format. It contains the following sections:

* [Global parameters](untitled-1.md#global-parameters): General information about the data operation.
* Working folder parameters: Information related to the input/output folder for the files.
* Filenames templates: Optionally, you can add a creation step for a table that will contain the result of the extraction.

## 👁🗨 Example

Here is an example of xml-conversion configuration file:

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
        <p>For a XML to CSV conversion data operation, the value is always &quot;xml-conversion&quot;.</p>
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
          configuration and runs in Tailer&#xAF;Studio.</p>
        <p>If not specified, the default value will be &quot;false&quot;.</p>
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
        <p>Google Cloud Storage Project Id.</p>
        <p>This is the project id of the bucket where the data is going to be converted.</p>
      </td>
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
      <td style="text-align:left">Path in the GCS bucket where the files will be extracted, e.g. &quot;some/sub/dir&quot;.</td>
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
        <p><b>filename_templates</b>
        </p>
        <p>type: array</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">A list of filename template that will link xml file names, xsd file names
        and eventual output files that need to be kept in the output folder.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>filename_template</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">A file name template that will trigger the conversion. The file name template
        can contains defined keyword (like {{FD_DATE}} for a date substring part).
        See STS for more information about all the keywords.</td>
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
      <td style="text-align:left">the name of the XSD file that will be used to convert the XML into a CSV.
        Note that the XSD will be used to validate the XML file. In the current
        version, only one XSD can be used be XML entry.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>output_suffix_filter</b>
        </p>
        <p>type: array of string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Define the names of the output file that need to be kept after a conversion.</p>
        <p>During a XML to CSV conversion, if the xml file contains of lot of child
          entites, the conversion will create a lot of files (one for each entity
          in fact). This filter allows to prevent uneccessary file upload on the
          output bucket. It works by finding an occurence of the string in the filename.
          For example if after a conversion a file named &quot;coupon_20210404_advantage.tsv&quot;
          is generated and the filter advantage.tsv is added, then this file will
          be kept.</p>
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

