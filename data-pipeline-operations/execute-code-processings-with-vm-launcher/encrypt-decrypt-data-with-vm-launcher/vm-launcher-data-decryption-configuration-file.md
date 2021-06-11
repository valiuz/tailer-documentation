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

## 👁🗨 Example

Here is an example of VM Launcher configuration file for data decryption:

```text
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
          <li>&quot;pgp-decrypt&quot;,</li>
          <li>and a description of the data to decrypt.</li>
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
          configuration and runs in Tailer Studio.</p>
        <p>If not specified, the default value will be &quot;false&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>pgp_mode</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>PGP mode.</p>
        <p>For data decryption, the value is always &quot;DECRYPT&quot;.</p>
      </td>
    </tr>
  </tbody>
</table>

## 💼 Working directory parameters

Information about the script location and instructions to execute it.

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
      <td style="text-align:left">Google Cloud Platform project ID for the bucket containing the files to
        decrypt.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_source_bucket</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Name of the GCS bucket containing the files to decrypt.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcs_source_prefix</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Path in the GCS bucket containing the files to decrypt, e.g. &quot;some/sub/dir&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>destination_gcs_bucket</b>
        </p>
        <p>type: dict</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Name of the GCS bucket containing the decrypted files.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>destination_gcs_path</b>
        </p>
        <p>type: array</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Path in the GCS bucket containing the decrypted files, e.g. &quot;some/sub/dir&quot;.</td>
    </tr>
  </tbody>
</table>

## 🖥 VM parameters

Information related to the Google Cloud Compute Engine VM where the script will be executed.

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
        <p><b>vm_delete</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>If set to &quot;true&quot;, this parameter will force the deletion of
          the VM at the end of the data operation. Running Compute Engine VMs will
          incur extra costs, so it is recommended to leave this parameter on &quot;true&quot;.
          <br
          />
        </p>
        <p>Default value: true</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>vm_core_number</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Virtual CPU (vCPU) count.
          <br />It is recommended to leave the default parameter, as this should allow
          sufficient performance to run a standard script.</p>
        <p>Default value: 2</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>vm_memory_amount</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>System memory size (in GB).</p>
        <p>It is recommended to leave the default parameter, as this should allow
          sufficient performance to run a standard script.</p>
        <p>Default value: 4</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>vm_disk_size</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Persistent disk size (in GB).</p>
        <p>It is recommended to leave the default parameter, as this should provide
          enough space to store the data to process.</p>
        <p>Default value: 20</p>
      </td>
    </tr>
  </tbody>
</table>

## 🔐 Credential parameters

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
        <p><b>credentials</b>
        </p>
        <p>type:array</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Array containing three entities:
        <br />input credentials for the input bucket,
        <br />output credentials for the output bucket,
        <br />and the private PGP key.</td>
    </tr>
  </tbody>
</table>

