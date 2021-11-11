---
description: >-
  This is the description of the JSON configuration file for a VM Launcher code
  processing data operation.
---

# VM Launcher configuration file for code processing

The configuration file is in JSON format. It contains the following sections:

* Global parameters: General information about the data operation.
* Script parameters: Information about the script location and instructions to execute on the VM.
* VM parameters: Information related to the VM where to execute the script.

## :eye\_in\_speech\_bubble: Example

Here is an example of VM Launcher configuration file for code processing:

```
{
    "configuration_type": "vm-launcher",
    "configuration_id": "000010_critizr_JUL_to_file",
    "environment": "PROD",
    "account": "000010",
    "activated": false,
    "archived": false,
    "direct_execution": true,
    "gcp_project_id": "fd-io-sct-jules",
    "gcs_bucket": "fd-io-sct-jules-s-critizr-in",
    "gcs_working_directory": "/",
    "credentials": {
        "gcp-credentials.json": {
            "content": {
                "cipher_aes": "", 
                "tag": "", 
                "ciphertext": "", 
                "enc_session_key": ""
            }
        }
    },
    "script_to_execute": [
        "mkdir -p input_DEV",
        "cd ./input_DEV && python3 critizr_API_JUL.py"
    ],
    "vm_delete": true,
    "vm_core_number": "2",
    "vm_memory_amount": "4",
    "vm_disk_size": "20",
    "vm_compute_zone": "europe-west1-b",
    "vm_custom_os_image_family": "ubuntu-2004-lts",
    "vm_custom_os_image_project": "ubuntu-os-cloud"
}
```

## :globe\_with\_meridians: Global parameters

| Parameter                                                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>configuration_type</strong></p><p>type: string</p><p>mandatory</p> | <p>Type of data operation.</p><p>For an STS data operation, the value is always "storage-to-storage".</p>                                                                                                                                                                                                                                                                                                                                                      |
| <p><strong>configuration_id</strong></p><p>type: string</p><p>mandatory</p>   | <p>ID of the data operation.</p><p>You can pick any name you want, but is has to be <strong>unique</strong> for this data operation type.</p><p>Note that in case of conflict, the newly deployed data operation will overwrite the previous one. To guarantee its uniqueness, the best practice is to name your data operation by concatenating:</p><ul><li>your account ID,</li><li>the source bucket name,</li><li>and the source directory name.</li></ul> |
| <p><strong>environment</strong></p><p>type: string</p><p>mandatory</p>        | <p>Deployment context.</p><p>Values: PROD, PREPROD, STAGING, DEV.</p>                                                                                                                                                                                                                                                                                                                                                                                          |
| <p><strong>account</strong></p><p>type: string</p><p>mandatory</p>            | Your account ID is a 6-digit number assigned to you by your Tailer Platform administrator.                                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>activated</strong></p><p>type: boolean</p><p>optional</p>          | <p>Flag used to enable/disable the execution of the data operation.</p><p>If not specified, the default value will be "true".</p>                                                                                                                                                                                                                                                                                                                              |
| <p><strong>archived</strong></p><p>type: boolean</p><p>optional</p>           | <p>Flag used to enable/disable the visibility of the data operation's configuration and runs in Tailer Studio.</p><p>If not specified, the default value will be "false".</p>                                                                                                                                                                                                                                                                                  |

## :writing\_hand: Script parameters

Information about the script location and instructions to execute it.

| Parameter                                                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p><strong>gcp_project_id</strong></p><p>type: string</p><p>mandatory</p>        | Google Cloud Platform project ID for the bucket containing the script.                                                                                                                                                                                                                                                                                                                                                                                                   |
| <p><strong>gcs_bucket</strong></p><p>type: string</p><p>mandatory</p>            | Name of the GCS bucket containing the script.                                                                                                                                                                                                                                                                                                                                                                                                                            |
| <p><strong>gcs_working_directory</strong></p><p>type: string</p><p>mandatory</p> | Path in the GCS bucket containing the script, e.g. "some/sub/dir".                                                                                                                                                                                                                                                                                                                                                                                                       |
| <p><strong>gcp_credentials_secret</strong></p><p>type: dict</p><p>mandatory</p>  | <p>Encrypted credentials needed to read/move data from the source bucket.</p><p>You should have generated credentials when <a href="https://app.gitbook.com/s/-MIIsP_DvP2J-c1szWrQ/data-pipeline-operations/getting-started/set-up-google-cloud-platform.md">setting up GCP</a>. To learn how to encrypt them, refer to <a href="https://app.gitbook.com/s/-MIIsP_DvP2J-c1szWrQ/data-pipeline-operations/getting-started/encrypt-your-credentials.md">this page</a>.</p> |
| <p><strong>script_to_execute</strong></p><p>type: array</p><p>mandatory</p>      | List of Unix commands to be executed (similar to a Bash script) on the VM.                                                                                                                                                                                                                                                                                                                                                                                               |

## :computer: VM parameters

Information related to the Google Cloud Compute Engine VM where the script will be executed.

| Parameter                                                                            | Description                                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p><strong>vm_delete</strong></p><p>type: string</p><p>optional</p>                  | <p> If set to "true", this parameter will force the deletion of the VM at the end of the data operation. Running Compute Engine VMs will incur extra costs, so it is recommended to leave this parameter on "true". <br></p><p>Default value: true</p> |
| <p><strong>vm_core_number</strong></p><p>type: string</p><p>optional</p>             | <p>Virtual CPU (vCPU) count.<br>It is recommended to leave the default parameter, as this should allow sufficient performance to run a standard script.</p><p></p><p>Default value: 2</p>                                                              |
| <p><strong>vm_memory_amount</strong></p><p>type: string</p><p>optional</p>           | <p>System memory size (in GB).</p><p>It is recommended to leave the default parameter, as this should allow sufficient performance to run a standard script.</p><p></p><p>Default value: 4</p>                                                         |
| <p><strong>vm_disk_size</strong></p><p>type: string</p><p>optional</p>               | <p>Persistent disk size (in GB).</p><p>It is recommended to leave the default parameter, as this should provide enough space to store the data to process.</p><p></p><p>Default value: 20</p>                                                          |
| <p><strong>vm_custom_os_image_family</strong></p><p>type: string</p><p>optional</p>  | <p>Image family of the custom image.</p><p></p><p>Note that for the time being, custom OS images MUST be based on a Ubuntu 20.04 LTS.</p><p></p><p>Default value: ubuntu-2004-lts</p>                                                                  |
| <p><strong>vm_custom_os_image_project</strong></p><p>type: string</p><p>optional</p> | <p>GCP Project hosting the custom image.</p><p></p><p>Note that this parameter is mandatory if <strong>vm_custom_os_image_family</strong> is set.</p><p><strong></strong></p><p>Default value: ubuntu-os-cloud</p>                                     |
