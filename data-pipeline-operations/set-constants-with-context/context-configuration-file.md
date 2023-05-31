---
description: >-
  This is the description of the JSON configuration file for a Context data
  operation.
---

# Context configuration file

The configuration file is in JSON format. It contains the following sections:

* [Global parameters](context-configuration-file.md#global-parameters): General information about the data operation.
* [Constant parameters](context-configuration-file.md#constant-parameters): Information related to the constants to set.

## :eye\_in\_speech\_bubble: Example

Here is an example of Context configuration file:

```json
{
	"$schema": "http://jsonschema.tailer.ai/schema/context-veditor",
	"configuration_type": "context",
	"configuration_id": "context_dev",
	"environment": "DEV",
	"account": "000099",
	"doc_md": "readme.md",
	"gcp_project_id": "fd-io-jarvis-demo-dlk",
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
				"cipher_aes": "xxx",
				"ciphertext": "xxx",
				"enc_session_key": "xxx",
				"tag": "xxx"
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
				"cipher_aes": "xxx",
				"ciphertext": "xxx",
				"enc_session_key": "xxx",
				"tag": "xxx"
			},
			"type": "object",
			"resource": "gcp_credentials_secret",
			"description": "The GCP Credentials used to store data in the gcp_credentials_secret_mirror_n_in bucket"
		},

		"gcp_credentials_secret_bigquery_dlk": {
			"value": {
				"cipher_aes": "xxx",
				"ciphertext": "xxx",
				"enc_session_key": "xxx",
				"tag": "xxx"
			},
			"type": "object",
			"resource": "gcp_credentials_secret",
			"description": "The GCP Credentials used to load data in the gcp_project_id_dlk Bigquery"
		}
	}
}
```

## :globe\_with\_meridians: Global parameters

General information about the configuration.

| Parameter                                                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>$schema</strong><br>type: string<br>optional</p>                   | The url of the json-schema that contains the properties that your configuration must verify. Most Code Editor can use that to validate your configuration, display help boxes and enlighten issues.                                                                                                                                                                                                                                       |
| <p><strong>configuration_type</strong></p><p>type: string</p><p>mandatory</p> | <p>Type of configuration.</p><p>For an Context configuration, the value is always "context".</p>                                                                                                                                                                                                                                                                                                                                          |
| <p><strong>configuration_id</strong></p><p>type: string</p><p>mandatory</p>   | <p>ID of the configuration.</p><p>You can pick any name you want, but is has to be <strong>unique</strong> for this configuration type in your account. For this configuration, the account is added as a prefix to your configuration_id to generate the definitive configuration id that will be displayed in Tailer Studio.</p><p>Note that in case of conflict, the newly deployed configuration will overwrite the previous one.</p> |
| <p><strong>environment</strong></p><p>type: string</p><p>mandatory</p>        | <p>Deployment environment.</p><p>Values: PROD, PREPROD, STAGING, DEV.</p>                                                                                                                                                                                                                                                                                                                                                                 |
| <p><strong>account</strong></p><p>type: string</p><p>mandatory</p>            | Your account ID is a 6-digit number assigned to you by your Tailer Platform administrator.                                                                                                                                                                                                                                                                                                                                                |
| <p><strong>doc_md</strong></p><p>type: string</p><p>mandatory</p>             | Path to a file containing a detailed description of the data operation. The file must be in Markdown format.                                                                                                                                                                                                                                                                                                                              |
| <p><strong>gcp_project_id</strong></p><p>type: string</p><p>mandatory</p>     | <p>Main Google Cloud Platform project ID for this configuration. </p><p>A same Context can be used for several GCP projects so this gcp project ID is not limitative. If not specified, then you'll be asked to provide it at the deployment.</p>                                                                                                                                                                                         |
| <p><strong>activated</strong></p><p>type: boolean</p><p>optional</p>          | <p>Flag used to enable/disable the execution of the data operation.</p><p>If not specified, the default value will be "true".</p>                                                                                                                                                                                                                                                                                                         |
| <p><strong>archived</strong></p><p>type: boolean</p><p>optional</p>           | <p>Flag used to enable/disable the visibility of the data operation's configuration and runs in Tailer Studio.</p><p>If not specified, the default value will be "false".</p>                                                                                                                                                                                                                                                             |

## :symbols: Constant parameters

Information related to the constants to set.

| Parameter                                                              | **Description**                                                                                                                                 |
| ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>parameters</strong></p><p>type: array</p><p>mandatory</p>   | Array containing a list of constants, all of them including a name, and the parameters listed below.                                            |
| <p><strong>value</strong></p><p>type: string</p><p>mandatory</p>       | Value that will be used to replace the placeholder in data operation configuration files.                                                       |
| <p><strong>type</strong></p><p>type: string</p><p>mandatory</p>        | <p>Type of constant.<br>Possible values:</p><ul><li>integer</li><li>string</li><li>float</li><li>object</li></ul>                               |
| <p><strong>resource</strong></p><p>type: string</p><p>mandatory</p>    | <p>Type of resource.</p><p>Possible values:</p><ul><li>value</li><li>gcp_project_id</li><li>gcs_bucket</li><li>gcp_credentials_secret</li></ul> |
| <p><strong>description</strong></p><p>type: string</p><p>mandatory</p> | Description of the constant.                                                                                                                    |
