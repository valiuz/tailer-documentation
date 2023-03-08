---
description: >-
  This is the description of the JSON configuration file for a VM Launcher data
  decryption data operation.
---

# VM Launcher configuration file for data decryption

The configuration file is in JSON format. It contains the following sections:

* Global parameters: General information about the data operation.
* Working directory parameters: Information about the input/output directories.
* Credential parameters: Information about the credentials for the input/output buckets and the PGP private key.

## :eye\_in\_speech\_bubble: Example

Here is an example of VM Launcher configuration file for data decryption:

```json
{
    "configuration_type": "vm-launcher",
    "configuration_id": "pgp-decrypt-from-valiuz",
    "environment": "PROD",
    "account": "000010",
    "activated": true,
    "archive": false,
    "pgp_mode": "DECRYPT",
    "gcp_project_id": "fd-jarvis-datalake",
    "gcs_source_bucket": "fd-io-exc-jules-n-val-in",
    "gcs_source_prefix": "input",
    "destination_gcs_bucket": "mirror-fd-io-exc-jules-n-val-in",
    "destination_gcs_path": "input_decrypted",
    "vm_delete": true,
    "credentials": {
        "input-credentials.json": {
            "content": {
                "cipher_aes": "",
                "tag": "",
                "ciphertext": "",
                "enc_session_key": ""
            }
        },
        "output-credentials.json": {
            "content": {
                "cipher_aes": "",
                "tag": "",
                "ciphertext": "",
                "enc_session_key": ""
            }
            
        },
        "private_key.pgp": {
            "passphrase": {
                "cipher_aes": "",
                "tag": "",
                "ciphertext": "",
                "enc_session_key": ""
            },
            "recipient": "olivier.colin@fashiondata.io",
            "content": {
                "cipher_aes": "",
                "tag": "",
                "ciphertext": "",
                "enc_session_key": ""
            }
        }
    }
}
```

## 🌐 Global parameters

| Parameter                                                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>configuration_type</strong></p><p>type: string</p><p>mandatory</p> | <p>Type of data operation.</p><p>For an STS data operation, the value is always "storage-to-storage".</p>                                                                                                                                                                                                                                                                                                                                                        |
| <p><strong>configuration_id</strong></p><p>type: string</p><p>mandatory</p>   | <p>ID of the data operation.</p><p>You can pick any name you want, but is has to be <strong>unique</strong> for this data operation type.</p><p>Note that in case of conflict, the newly deployed data operation will overwrite the previous one. To guarantee its uniqueness, the best practice is to name your data operation by concatenating:</p><ul><li>your account ID,</li><li>"pgp-decrypt",</li><li>and a description of the data to decrypt.</li></ul> |
| <p><strong>environment</strong></p><p>type: string</p><p>mandatory</p>        | <p>Deployment context.</p><p>Values: PROD, PREPROD, STAGING, DEV.</p>                                                                                                                                                                                                                                                                                                                                                                                            |
| <p><strong>account</strong></p><p>type: string</p><p>mandatory</p>            | Your account ID is a 6-digit number assigned to you by your Tailer Platform administrator.                                                                                                                                                                                                                                                                                                                                                                       |
| <p><strong>activated</strong></p><p>type: boolean</p><p>optional</p>          | <p>Flag used to enable/disable the execution of the data operation.</p><p>If not specified, the default value will be "true".</p>                                                                                                                                                                                                                                                                                                                                |
| <p><strong>archived</strong></p><p>type: boolean</p><p>optional</p>           | <p>Flag used to enable/disable the visibility of the data operation's configuration and runs in Tailer Studio.</p><p>If not specified, the default value will be "false".</p>                                                                                                                                                                                                                                                                                    |
| <p><strong>pgp_mode</strong></p><p>type: string</p><p>optional</p>            | <p>PGP mode.</p><p>For data decryption, the value is always "DECRYPT".</p>                                                                                                                                                                                                                                                                                                                                                                                       |

## 💼 Working directory parameters

Information about the script location and instructions to execute it.

| Parameter                                                                       | Description                                                                      |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| <p><strong>gcp_project_id</strong></p><p>type: string</p><p>mandatory</p>       | Google Cloud Platform project ID for the bucket containing the files to decrypt. |
| <p><strong>gcs_source_bucket</strong></p><p>type: string</p><p>mandatory</p>    | Name of the GCS bucket containing the files to decrypt.                          |
| <p><strong>gcs_source_prefix</strong></p><p>type: string</p><p>mandatory</p>    | Path in the GCS bucket containing the files to decrypt, e.g. "some/sub/dir".     |
| <p><strong>destination_gcs_bucket</strong></p><p>type: dict</p><p>mandatory</p> | Name of the GCS bucket containing the decrypted files.                           |
| <p><strong>destination_gcs_path</strong></p><p>type: array</p><p>mandatory</p>  | Path in the GCS bucket containing the decrypted files, e.g. "some/sub/dir".      |

## 🖥 VM parameters

Information related to the Google Cloud Compute Engine VM where the script will be executed.

| Parameter                                                                   | Description                                                                                                                                                                                                                                          |
| --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>vm_delete</strong></p><p>type: string</p><p>mandatory</p>        | <p>If set to "true", this parameter will force the deletion of the VM at the end of the data operation. Running Compute Engine VMs will incur extra costs, so it is recommended to leave this parameter on "true".<br></p><p>Default value: true</p> |
| <p><strong>vm_core_number</strong></p><p>type: string</p><p>mandatory</p>   | <p>Virtual CPU (vCPU) count.<br>It is recommended to leave the default parameter, as this should allow sufficient performance to run a standard script.</p><p>Default value: 2</p>                                                                   |
| <p><strong>vm_memory_amount</strong></p><p>type: string</p><p>mandatory</p> | <p>System memory size (in GB).</p><p>It is recommended to leave the default parameter, as this should allow sufficient performance to run a standard script.</p><p>Default value: 4</p>                                                              |
| <p><strong>vm_disk_size</strong></p><p>type: string</p><p>mandatory</p>     | <p>Persistent disk size (in GB).</p><p>It is recommended to leave the default parameter, as this should provide enough space to store the data to process.</p><p>Default value: 20</p>                                                               |

## 🔐 Credential parameters

| Parameter                                                            | Description                                                                                                                                                 |
| -------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>credentials</strong></p><p>type:array</p><p>mandatory</p> | <p>Array containing three entities:<br>input credentials for the input bucket,<br>output credentials for the output bucket,<br>and the private PGP key.</p> |
