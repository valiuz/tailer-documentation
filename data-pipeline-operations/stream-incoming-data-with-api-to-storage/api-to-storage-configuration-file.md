# API To Storage configuration file

The configuration file is in JSON format. It contains the following sections:

* Global parameters: General information about the data operation.
* Source parameters: Information related to the data source provider.
* Destination parameters: One or several destination blocks, containing information about the data destinations.

## :eye\_in\_speech\_bubble: Example

Here is an example of ATS configuration file exposing a Pub/Sub endpoint and an output to GCS :

```json
{
  "$schema": "http://jsonschema.tailer.ai/schema/api-to-storage-veditor",
  "configuration_type": "api-to-storage",
  "configuration_id": "000099-ats-example-products",
  "environment": "DEV",
  "account": "000099",
  "activated": true,
  "archived": false,
  "short_description": "This API will receive PRODUCTS and store them to GCS blobs.",
  "doc_md": "000099-ats-example-products.md",
  "source": {
    "type": "pubsub",
    "gcp_project_id": "fd-io-jarvis-demo-dlk",
    "pubsub_topic_suffix": "example-products",
    "protocol_buffers_file": "000099-ats-example-products.proto"
  },
  "destinations": [
    {
      "type": "gcs",
      "gcs_destination_bucket": "fd-io-demo-n-in",
      "gcs_destination_prefix": "ats-repository/products/input",
      "gcs_filename_template": "products.json",
      "gcp_credentials_secret": {
        "cipher_aes": "473a2d0cb3",
        "tag": "6f72f",
        "ciphertext": "ba9c04ebe99e7c83cdd1fde63e63a8485472906",
        "enc_session_key": "1bd17b6fd0ac286161d6"
      },
      "description": "This is a short description of the GCS output."
    }
  ]
}
```

## :globe\_with\_meridians: Global parameters

General information about the data operation

| Parameter                                                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>$schema</strong> </p><p>type: string </p><p>optional</p>           | The url of the json-schema that contains the properties that your configuration must verify. Most Code Editor can use that to validate your configuration, display help boxes and enlighten issues.                                                                                                                                                                                                                                                            |
| <p><strong>configuration_type</strong></p><p>type: string</p><p>mandatory</p> | <p>Type of data operation.<br><br>For an ATS data operation, the value is always "api-to-storage".</p>                                                                                                                                                                                                                                                                                                                                                         |
| <p><strong>configuration_id</strong></p><p>type: string</p><p>mandatory</p>   | <p>ID of the data operation.</p><p>You can pick any name you want, but is has to be <strong>unique</strong> for this data operation type.</p><p>Note that in case of conflict, the newly deployed data operation will overwrite the previous one. To guarantee its uniqueness, the best practice is to name your data operation by concatenating:</p><ul><li>your account ID,</li><li>the source bucket name,</li><li>and the source directory name.</li></ul> |
| <p><strong>environment</strong></p><p>type: string</p><p>Mandatory</p>        | <p>Deployment context.<br><br>Values: PROD, PREPROD, STAGING, DEV.</p>                                                                                                                                                                                                                                                                                                                                                                                         |
| <p><strong>account</strong></p><p>type: string</p><p>mandatory</p>            | Your account ID is a 6-digit number assigned to you by your Tailer Platform administrator.                                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>activated</strong></p><p>type: boolean</p><p>optional</p>          | <p>Flag used to enable/disable the execution of the data operation.<br><br>If not specified, the default value will be "true".</p>                                                                                                                                                                                                                                                                                                                             |
| <p><strong>archived</strong></p><p>type: boolean</p><p>optional</p>           | <p>Flag used to enable/disable the visibility of the data operation's configuration and runs in Tailer Studio.<br><br>If not specified, the default value will be "false".</p>                                                                                                                                                                                                                                                                                 |
| <p><strong>max_active_runs</strong></p><p>type: integer</p><p>optional</p>    | <p>This parameter limits the number of concurrent runs for this data operation.</p><p>If not set, the default value is 1.</p>                                                                                                                                                                                                                                                                                                                                  |
| <p><strong>short_description</strong></p><p>type: string</p><p>optional</p>   | Short description of the context of the configuration.                                                                                                                                                                                                                                                                                                                                                                                                         |
| <p><strong>doc_md</strong></p><p>type: string</p><p>optional</p>              | Path to a file containing a detailed description. The file must be in Markdown format.                                                                                                                                                                                                                                                                                                                                                                         |

## Source parameters (Pub/Sub)

The destination section contains all information related to the data source provider.

```
"source": {
    "type": "pubsub",
    "gcp_project_id": "fd-io-jarvis-demo-dlk",
    "pubsub_topic_suffix": "example-products",
    "protocol_buffers_file": "000099-ats-example-products.proto"
  }
```

| Parameter                                                                       | Description                                                                                                                                                                                           |
| ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>type</strong></p><p>type: string</p><p>mandatory</p>                 | <p>Source type.</p><p>The only supported source type for now is "pubsub".</p>                                                                                                                         |
| <p><strong>gcp_project_id</strong></p><p>type: string</p><p>mandatory</p>       | <p>Specify the Google Cloud Platform project where to deploy the data operation and its associated cloud functions.</p><p>If not set, the user will be prompted to choose a project.</p>              |
| <p><strong>pubsub_topic_suffix</strong></p><p>type: string</p><p>mandatory</p>  | Name of the Pub/Sub topic that will be created.                                                                                                                                                       |
| <p><strong>protocol_buffers_file</strong></p><p>type: string</p><p>optional</p> | <p>Filename pointing to a Protocol Buffers 2 file.</p><p>If specified, all incoming data streamed through the topic will be checked against the definition included in the Protocol Buffers file.</p> |

### Protocol Buffers File syntax

Pub/Sub allow to verify an incoming payload using a Protocol Buffers definition.

Protocol Buffers (proto2) langage guide : [https://developers.google.com/protocol-buffers/docs/proto?hl=fr](https://developers.google.com/protocol-buffers/docs/proto?hl=fr)

The definition should be defined as follow. Note that the message "Item" is where you must customize your payload schema.

Let's use the following example where the attribute "new\_item" is optional:

```json
{
    "input_data": [
        {
            "product_id": "123456789",
            "label": "Some label ABC",
            "description": "A specific description for product 123456789"
        },
        {
            "product_id": "987654321",
            "label": "Some label YUI",
            "description": "A specific description for product 987654321"
        },
        {
            "label": "Some label XYZ",
            "product_id": "66668888",
            "description": "A specific description for product 66668888"
        },
        {
            "product_id": "66668888",
            "label": "Some label XYZ",
            "description": "A specific description for product 66668888",
            "new_item": "some new data"
        }
    ]
}
```

The corresponding Protocol Buffers definition should be like this:

```
syntax = "proto2";

message GlobalMessage {

  message Item {
    required string product_id = 1;
    required string label = 2;
    required string description = 3;
    optional string new_item = 4;
  }

  repeated Item input_data = 1;
}
```

## Destination parameters

These parameters allow you specify a list of destinations. You can add as many "destination" sub-objects as you want, they will all be processed.

### **Google Cloud Storage destination**

Example:

```json
"destinations": [
    {
      "type": "gcs",
      "gcs_destination_bucket": "fd-io-demo-n-in",
      "gcs_destination_prefix": "ats-repository/products/input",
      "gcs_filename_template": "products.json",
      "gcp_credentials_secret": {
        "cipher_aes": "473a2d0cb3",
        "tag": "6f72f",
        "ciphertext": "c9e7c83cdd1fde63e63a8485472906",
        "enc_session_key": "1bd17b6fd0ac286161d621bf07c20593f14ea"
      },
      "description": "This is a short description of the GCS output."
    }
  ]
```

| Parameter                                                                         | Description                                                                                                                                                                                                                                                                                                                                     |
| --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>type</strong></p><p>type: string</p><p>mandatory</p>                   | <p>Type of destination.</p><p>In this case : "gcs".</p>                                                                                                                                                                                                                                                                                         |
| <p><strong>gcs_destination_bucket</strong></p><p>type: string</p><p>mandatory</p> | Google Cloud Storage destination bucket.                                                                                                                                                                                                                                                                                                        |
| <p><strong>gcs_destination_prefix</strong></p><p>type: string</p><p>mandatory</p> | Google Cloud Storage destination path, e.g. "/subdir/subdir\_2" to send the files to "gs://BUCKET/subdir/subdir\_2/source\_file.ext"                                                                                                                                                                                                            |
| <p><strong>gcp_credentials_secret</strong></p><p>type: dict</p><p>mandatory</p>   | <p>Encrypted credentials needed to read/write/move data from the destination bucket.</p><p>You should have generated credentials when <a href="../../getting-started/set-up-google-cloud-platform.md">setting up GCP</a>. To learn how to encrypt them, refer to <a href="../../getting-started/encrypt-your-credentials.md">this page</a>.</p> |
| <p><strong>gcs_filename_template</strong></p><p>type: dict</p><p>mandatory</p>    | Filename template that will be used to write incoming data to a GCS storage.                                                                                                                                                                                                                                                                    |
| <p><strong>description</strong></p><p>type: dict</p><p>optional</p>               | Short description of the destination.                                                                                                                                                                                                                                                                                                           |

