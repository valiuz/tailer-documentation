# VM Launcher: configuration file

After the data extraction through a tables-to-storage data operation, the GBQ to Firestore data pipeline continues with a vm-launcher data operation. It will launch a VM on Google Compute Engine, execute the Python script that loads the JSON data file into Firestore and then stop the VM. You can find the global parameters of this configuration in the [VM-launcher configuration](../execute-code-processings-with-vm-launcher/process-code-with-vm-launcher/vm-launcher-code-processing-configuration-file.md) page.

As a reminder, we've just seen in the previous pages how to create a table-to-storage data operation that generates a set of `freshness_next_execution_data-*.json` data files in the `gbq-to-firestore/000001/next_execution/` directory of the `tailer-freshness` GCS bucket.

The VM Launcher data operation executes the script that is in the "script\_to\_execute" parameter, using the "gcs\_working\_directory" on the "gcs\_bucket" as a working directory. The first script line sets up a few prerequisites. The second line executes file-to-firestore.py. This Python script is described in the next page. A few arguments must be specified, as described below.

{% hint style="info" %}
The file-to-firestore.py Python script must be uploaded in the Google Cloud Storage working directory before executing the VM launcher data operation.
{% endhint %}

## &#x20;:snake: file-to-firestore python call

In the "script\_to\_execute" parameter in the example below, you see a few arguments that are explained here.\
Example: "python3 file-to-firestore.py ./000001 '{\\"next\_execution\\":{\\"sub\_dir\\":\\"next\_execution\\",\\"file\_template\\":\\"freshness\_next\_execution\_data-\*.json\\"\}}'"

| Parameters                                                                                                                                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **file-to-firestore.py**                                                                                                                              | <p>Name of the python file to execute. </p><p>It must be uploaded in the working directory  of the GCS bucket defined in the vm-launcher configuration file before the first execution.<br>You can modify the Python script (provided in the next page) and rename it if you like.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **./000001**                                                                                                                                          | <p><strong>Python script first argument:</strong></p><p>The relative path, in the working directory, of the folder that contains the data files.<br>You can specify sub-directories in the next argument.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <p>'{\"next_execution\":</p><p>{</p><p>\"sub_dir\":\"next_execution\",</p><p>\"file_template\":\"freshness_next_execution_data-*.json\"}</p><p>}'</p> | <p><strong>Python script second argument:</strong></p><p>This is a stringified list of json objects that contain informations on the files to process. Please note that you must escape the double quote characters with a backslash.</p><p><br>In this example, there's only one use-case, named "next_execution", but you could have several of them. For each use-case, you need to specify a "sub_dir" and a "file_template":</p><ul><li>sub_dir is the relative path of the sub directory where the data files are stored. Here it's "next_execution".</li><li>file_template is the template of the names of your data files. It can contain a wildcard "*" that stands for a set of any characters. In our example, this wildcard handles the fact that we could have several data files named for example freshness_next_execution_data-00000000.json and reshness_next_execution_data-00000001.json.</li></ul><p>See the screenshots of the bucket and files below. </p> |

## :eye\_in\_speech\_bubble: Example

Here is an example of VM Launcher configuration file that loads data from the tailer-freshness Google Cloud Storage Bucket. The Firestore destination will be specified inside the Python script (see next page).

```json
{
    "configuration_type": "vm-launcher",
    "configuration_id": "000001_json_to_firestore_freshness_next_execution",
    "environment": "PROD",
    "account": "000001",
    "activated": true,
    "archived": false,
    "direct_execution": true,
    "gcp_project_id": "fd-tailer-datalake",
    "gcs_bucket": "tailer-freshness",
    "gcs_working_directory": "gbq-to-firestore",
    "credentials": {
        "gcp-credentials.json": {
            "content": {
                "cipher_aes": "xxx", 
                "tag": "xxx", 
                "ciphertext": "xxx", 
                "enc_session_key": "xxx"
            }
        }
    },
    "script_to_execute": [
        "pip3 install google-cloud-firestore simplejson pytz",
        "python3 file-to-firestore.py ./000001 '{\"next_execution\":{\"sub_dir\":\"next_execution\",\"file_template\":\"freshness_next_execution_data-*.json\"}}'"
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

This configuration uses the gbq-to-firestore directory of the tailer-freshness GCS bucket as a working directory. You can see the Python file and the data directory are located here:

![Place where file-to-firestore.py is deposited for the example](<../../.gitbook/assets/Capture d’écran 2022-05-24 à 17.14.43.png>)

Insead the data directory, you find data files as specified:

![Path in GCS for the file reading by File-to-firestore.py](<../../.gitbook/assets/Capture d’écran 2022-05-24 à 17.03.00.png>)
