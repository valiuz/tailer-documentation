---
description: >-
  This is the description of the JSON configuration file for a Workflow data
  operation.
---

# Workflow configuration file

A Workflow data operation is used to specify one or several data operation IDs that need to be successfully executed for one target data operation to be triggered.

## 👁🗨 Example

```text
{
  "configuration_type": "workflow",
  "configuration_id": "000099_iowa_liquor_workflow_export_cluster_stores",
  "environment": "DEV",
  "short_description": "Launch Iowa Liquor Tailer Demo Export Cluster Stores Tables",
  "account": "000099",
  "activated": true,
  "archived": false,
  "gcp_project_id": "fd-io-jarvis-demo-dlk",
  "authorized_job_ids": ["gbq-to-gbq|000099-build-iowa-liquor-model_DEV"],
  "target_dag": "table-to-storage",
  "extra_parameters": {
    "firestore_conf_doc_id": "000099_iowa_liquor_export_cluster_stores"
  }
}
```

## ⚙ Parameters

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
      <td style="text-align:left">Type of configuration file.
        <br />
        <br />In this case, the value has to be &quot;workflow&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>configuration_id</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>ID of the workflow.</p>
        <p>You can pick any name you want, but is has to be <b>unique</b> for this
          type of configuration file.</p>
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
        <p><b>short_description</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Short description of the context of the configuration file.</td>
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
        <p><b>gcp_project</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">ID of the Google Cloud project containing the BigQuery instance to be
        triggered.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>activated</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Flag used to enable/disable the execution of the workflow.</p>
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
        <p>Flag used to enable/disable the visibility of the workflow&apos;s configuration
          and runs in Tailer&#xA0;Studio.</p>
        <p>If not specified, the default value will be &quot;false&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>authorized_job_ids</b>
        </p>
        <p>type: string array</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Data operations that need to be executed and successful for the current
        workflow to be triggered.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>target_dag</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Data operation to trigger.</p>
        <p>For Tables to Tables data operations, you need to specify the data operation <b>configuration_id</b>,
          concatenated with the environment (&quot;_PROD&quot; or &quot;_DEV&quot;).</p>
        <p>Example: &quot;000099_load_stores_details_PROD&quot;</p>
        <p>For Storage to Storage, Storage to Tables et Table to Storage data operations,
          you only specify the type of data operation with one of the following values:</p>
        <ul>
          <li>storage-to-storage</li>
          <li>storage-to-tables</li>
          <li>table-to-storage</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>extra_parameters</b>
        </p>
        <p>type: dict</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>This array contains the list of data operations to trigger.</p>
        <p>Leave it empty for Tables to Tables data operations, as the <b>target_dag</b> parameter
          already contains this information.</p>
        <p>For other data operations, list one or several <b>firestore_conf_doc_id</b> parameters.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>firestore_conf_doc_id</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>List of data operations to trigger.</p>
        <p>Specify the <b>configuration_id</b> of the data operations to trigger.</p>
      </td>
    </tr>
  </tbody>
</table>

