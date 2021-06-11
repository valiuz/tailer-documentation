---
description: >-
  This is the description of the JSON configuration file for a Context data
  operation.
---

# Context configuration file

The configuration file is in JSON format. It contains the following sections:

* [Global parameters](context-configuration-file.md#global-parameters): General information about the data operation.
* [Constant parameters](context-configuration-file.md#constant-parameters): Information related to the constants to set.

## 👁🗨 Example

Here is an example of Context configuration file:

```text
{
	"configuration_type": "context",
	"configuration_id": "context_dev",
	"environment": "DEV",
	"account": "000099",
	"activated": true,
	"archived": false,
	"parameters": {
		"test_integer": {
			"value": 123456,
			"type": "integer",
			"resource": "value",
			"description": "Some random value"
		},
		"test_float": {
			"value": 123456.78910,
			"type": "float",
			"resource": "value",
			"description": "Some random value"
		},
		"test_boolean": {
			"value": true,
			"type": "boolean",
			"resource": "value",
			"description": "Some random value"
		},
		"gcp_project_id_exc": {
			"value": "fd-io-jarvis-demo-exc",
			"type": "string",
			"resource": "gcp_project_id",
			"description": "The Default Exchange GCP Project ID"
		},

		"gcs_source_bucket_n_in": {
			"value": "fd-io-exc-demo-n-in",
			"type": "string",
			"resource": "gcs_bucket",
			"description": "The Default Exchange Source IN Bucket ID"
		},

		"gcp_credentials_secret_source_n_in": {
			"value": {
				"cipher_aes": "",
				"ciphertext": "",
				"enc_session_key": "",
				"tag": ""
			},
			"type": "object",
			"resource": "gcp_credentials_secret",
			"description": "The GCP Credentials used to load data from the gcp_credentials_secret_source_n_in bucket"
		},

		"gcp_project_id_dlk": {
			"value": "fd-io-jarvis-demo-dlk",
			"type": "string",
			"resource": "gcp_project_id",
			"description": "The Default Exchange GCP Project ID"
		},

		"gcs_mirror_bucket_n_in": {
			"value": "mirror-fd-io-demo-n-in",
			"type": "string",
			"resource": "gcs_bucket",
			"description": "The Default Exchange Mirror IN Bucket ID"
		},

		"gcp_credentials_secret_mirror_n_in": {
			"value": {
				"cipher_aes": "",
				"ciphertext": "",
				"enc_session_key": "",
				"tag": ""
			},
			"type": "object",
			"resource": "gcp_credentials_secret",
			"description": "The GCP Credentials used to store data in the gcp_credentials_secret_mirror_n_in bucket"
		},

		"gcp_credentials_secret_bigquery_dlk": {
			"value": {
				"cipher_aes": "",
				"ciphertext": "",
				"enc_session_key": "",
				"tag": ""
			},
			"type": "object",
			"resource": "gcp_credentials_secret",
			"description": "The GCP Credentials used to load data in the gcp_project_id_dlk Bigquery"
		}
	}
}

```

## 🌐 Global parameters

General information about the data operation.

* 
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
          overwrite the previous one.</p>
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
          configuration and runs in Tailer Studio.</p>
        <p>If not specified, the default value will be &quot;false&quot;.</p>
      </td>
    </tr>
  </tbody>
</table>

## 🔣 Constant parameters

Information related to the constants to set.

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
        <p><b>parameters</b>
        </p>
        <p>type: array</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Array containing a list of constants, all of them including a name, and
        the parameters listed below.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>value</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Value that will be used to replace the placeholder in data operation configuration
        files.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>type</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Type of constant.
          <br />Possible values:</p>
        <ul>
          <li>integer</li>
          <li>string</li>
          <li>float</li>
          <li>object</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>resource</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Type of resource.</p>
        <p>Possible values:</p>
        <ul>
          <li>value</li>
          <li>gcp_project_id</li>
          <li>gcs_bucket</li>
          <li>gcp_credentials_secret</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>description</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Description of the constant.</td>
    </tr>
  </tbody>
</table>

