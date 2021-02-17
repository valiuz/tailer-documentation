---
description: >-
  This is the description of the JSON configuration file of a Tables to Tables
  data operation.
---

# Tables to Tables configuration file

The configuration file is in JSON format. It contains the following sections:

* Global parameters: General information about the data operation.
* Workflow task parameters: Information about the different tasks of the workflow.

## 👁🗨 Example

Here is an example of TTT configuration file:

```text
{
  "configuration_type" : "table-to-table",
  "configuration_id" : "000099_test_sql_dag_v1",
  "short_description" : "Short description of the job",
  "account" : "000099",
  "environment" : "PROD",
  "activated": true,
  "archived": false,
  "start_date" : "2019, 1, 23",
  "schedule_interval" : "None",
  "max_active_runs" : 2,
  "task_concurrency" : 3,
  "default_gcp_project_id" : "fd-tailer-datalake",
  "default_bq_dataset" : "rmp152_datalake",
  "default_write_disposition" : "WRITE_TRUNCATE",
  "task_dependencies" : [
    "create_collection_plan_table >> customer_value_step1 >> pda_dmp_event",
    "pda_dmp_event >> pda_customers"
  ],
  "workflow" : [
    {
      "id" : "customer_value_step1",
      "gcp_project_id" : "Project_A",
      "bq_dataset" : "Dataset_Z", 
      "table_name" : "customer_value",
      "write_disposition" : "WRITE_TRUNCATE",
      "sql_file" : "customer_value_step1.sql"
    },
    {
      "id" : "pda_dmp_event",
      "gcp_project_id" : "Project_A",
      "bq_dataset" : "Dataset_Y",
      "table_name" : "dmp_event",
      "write_disposition" : "WRITE_TRUNCATE",
      "sql_file" : "pda_dmp_event.sql"
    },
    {
      "id" : "pda_customers",
      "gcp_project_id" : "Project_A",
      "bq_dataset" : "Dataset_X", 
      "table_name" : "customers",
      "write_disposition" : "WRITE_TRUNCATE",
      "sql_file" : "pda_customers.sql"
    },
    {
      "id": "create_collection_plan_table",
      "short_description": "Create fd-io-dlk-pimkie.dlk_pim_pda.collection_plan",
      "task_type": "create_gbq_table",
      "bq_table": "collection_plan",
      "force_delete": true,
      "ddl_file" : "000020_Load_PSA_to_PDA_collection_plan_DDL.json"
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
        <p>For a TTT data operation, the value is always &quot;table-to-table&quot;.</p>
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
          <li>the word &quot;load&quot;,</li>
          <li>and the target dataset or table.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>short_description</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Short description of the context of the data operation.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>doc_md</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Path to a file containing a detailed description of the data operation.
        The file must be in Markdown format.</td>
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
      <td style="text-align:left">Your account ID is a 6-digit number assigned to you by your Tailer&#xA0;Platform
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
          configuration and runs in Jarvis Studio.</p>
        <p>If not specified, the default value will be &quot;false&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>start_date</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>Start date of the data operation.</p>
        <p>The format must be:</p>
        <p>&quot;YYYY, MM, DD&quot;</p>
        <p>Where:</p>
        <ul>
          <li>YYYY &gt;= 1970</li>
          <li>MM = [1, 12]</li>
          <li>DD = [1, 31]</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>schedule_interval</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>A Tables to Tables data operation can be launched in two different ways:</p>
        <ul>
          <li>If <b>schedule_interval</b> is set to &quot;None&quot;, the data operation
            will need to be started with a <a href="../orchestrate-processings-with-workflow/">Workflow</a>,
            when a given condition is met. (This solution is recommended.)</li>
          <li>If you want the data operation to start at regular intervals, you can
            define this in the <b>schedule_interval</b> parameter with a Cron expression.</li>
        </ul>
        <p><b>Example</b>
        </p>
        <p>For the data operation to start everyday at 7:00, you need to set it as
          follows:</p>
        <p><code>&quot;schedule_interval&quot;: &quot;0 7 * * *&quot;,</code>
        </p>
        <p></p>
        <p>You can find online tools to help you edit your Cron expression (for example,
          <a
          href="https://crontab.guru/">crontab.guru</a>).</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>max_active_runs</b>
        </p>
        <p>type: integer</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>This parameter limits the number of concurrent runs for this data operation.
          As most data operations are run once daily, there is usually no need to
          set a value higher than 1 here.</p>
        <p>If not set, the default value is 1.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>task_concurrency</b>
        </p>
        <p>type: integer</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>This parameter limits the number of tasks that might run concurrently.</p>
        <p>As a great volume of data might be handled by each task, it is important
          to make sure to avoid consuming too many resources for one data operation.</p>
        <p>Make sure also that the value you set here is high enough for concurrent
          tasks set in the <b>task_dependencies</b> parameter to run properly.</p>
        <p>If not set, the default value is 5.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>catchup</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>This parameter allows you to specify if you want to execute the data operation
          runs that were supposed to happen between <b>start_date</b> and the actual
          deployment date.</p>
        <p></p>
        <p><b>Example</b>
        </p>
        <p>If you start receiving data from September 1st, but you only finish writing
          your code on September 7th, you might want to run your data operation from
          a date in the past: September 1st.</p>
        <p></p>
        <p>The <b>catchup</b> parameter can have two values:</p>
        <ul>
          <li>If it<b> </b>is set to &quot;true&quot; AND a <b>scheduling_interval</b> is
            set AND <b>start_date</b> is set in the past, Composer/Airflow will execute
            every run of the data operation scheduled from the start date until the
            current date.</li>
          <li>If it is set to &quot;false&quot;, the data operation will only be executed
            starting from the current date.</li>
        </ul>
        <p>&#x26A0; If the data operation is scheduled to happen frequently and/or
          the missed execution period is long, the amount of runs might be important.
          Make sure you have enough resources to handle all the executions when deploying
          a data operation with <b>catchup</b> set to &quot;true&quot;.</p>
        <p></p>
        <p>If not set, the default value is &quot;false&quot;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>task_dependencies</b>
        </p>
        <p>type: array of strings</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>The <b>task_dependencies </b>parameter allows you to create dependencies
          between the different tasks specified in the <b>workflow</b> parameter (see
          below). It will define in which order the workflow tasks will run, some
          of them running concurrently, others sequentially.</p>
        <p>Usually workflow tasks will be run in the following order:</p>
        <p></p>
        <ol>
          <li><b>create_gbq_table</b>
          </li>
          <li><b>sql</b>
          </li>
          <li><b>copy_gbq_table</b>
          </li>
        </ol>
        <p><b>Syntax</b>
        </p>
        <ul>
          <li>The double chevron <code>&gt;&gt;</code> means that the first task needs
            to be completed before the next one can start.</li>
          <li>The comma <code>,</code> means that the tasks will run concurrently.</li>
          <li>The square brackets <code>[</code> and <code>]</code> allow you to define
            a set of tasks that will run together.</li>
        </ul>
        <p>&#x26A0; If there are <b>no dependencies</b>, or only one task in you workflow,
          specify an <b>empty array</b>: <code>[]</code>
        </p>
        <p>For example, <code>[ &quot; taskA &quot; ]</code>would raise an error</p>
        <p></p>
        <p>For detailed information about the syntax, refer to the Airflow documentation:
          <a
          href="https://airflow.apache.org/concepts.html#bitshift-composition">https://airflow.apache.org/concepts.html#bitshift-composition</a>
        </p>
        <p></p>
        <p><b>Example 1</b>
        </p>
        <p>We have the following tasks that we want to run sequentially: taskA (create_gbq_table),
          taskB (sql) and taskC (copy_gbq_table).
          <br />The <b>task_dependencies</b> parameter will be as follows: <code>&quot;task_dependencies&quot;: [ &quot; taskA &gt;&gt; taskB &gt;&gt; taskC &quot; ],</code>
        </p>
        <p></p>
        <p><b>Example 2</b>
        </p>
        <p>We have the following copy_gbq_table tasks that we want to run concurrently:
          taskA, taskB and taskC.</p>
        <p>The <b>task_dependencies</b> parameter will be as follows: <code>&quot;task_dependencies&quot;: [ &quot; taskA, taskB, taskC &quot; ],</code>
          <br
          />
        </p>
        <p><b>Example 3</b>
        </p>
        <p>We have the following 9 tasks we want to order: taskA, taskD, taskG (create_gbq_table),
          taskB, taskE, taskH (sql), taskC, taskF, taskI (copy_gbq_table).
          <br />The <b>task_dependencies</b> parameter will be as follows:<code>&quot;task_dependencies&quot;: [ &quot; [taskA, taskD, taskG] &gt;&gt; [taskB, taskE, taskH] &gt;&gt; [taskC, taskF, taskI] &quot; ],</code> 
        </p>
        <p><b>&lt;code&gt;&lt;/code&gt;</b>
        </p>
        <p><b>Example 4</b>
        </p>
        <p>In the example above, we want taskH to run before taskE so we can use
          its SQL code for taskE.</p>
        <p>The <b>task_dependencies</b> parameter will be as follows:</p>
        <p><code>&quot;task_dependencies&quot;: [ &quot; [taskA, taskD, taskG] &gt;&gt; taskH &gt;&gt; [taskB, taskE] &gt;&gt; [taskC, taskF, taskI] &quot; ],</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>workflow</b>
        </p>
        <p>type: array of maps</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>List of tasks the data operations will execute.</p>
        <p>Check the section below for detailed information on their parameters.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>default_gcp_project_id</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Default GCP Project ID.</p>
        <p>This parameter can be set for each workflow task sub-object, and will
          be overridden by that value if it is different.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>default_bq_dataset</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Default BigQuery dataset ID.</p>
        <p>This parameter can be set for each workflow task sub-object, and will
          be overridden by that value if it is different.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>default_write_disposition</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Action that occurs if the destination table already exists (see <a href="https://googleapis.github.io/google-cloud-python/latest/bigquery/generated/google.cloud.bigquery.job.WriteDisposition.html#google.cloud.bigquery.job.WriteDisposition">Google BigQuery documentation</a>).</p>
        <p></p>
        <p>Possible values:</p>
        <ul>
          <li>&quot;WRITE_TRUNCATE&quot; (default): The run will write table data from
            the beginning. If the table already contained lines, they will all be deleted
            and replaced by the new lines. This option is used most of the time for
            daily runs to avoid duplicates.</li>
          <li>&quot;WRITE_APPEND&quot;: The run will append new lines to the table.
            When using this option, make sure not to run the data operation several
            times.</li>
          <li>&quot;WRITE_EMPTY&quot;: This option only allows adding data to an empty
            table. If the table already contains data, it returns an error. It is hardly
            ever used as data operations are usually run periodically, so they will
            always contain data after the first run.</li>
        </ul>
        <p>This parameter can be set for each workflow task sub-object, and will
          be overridden by that value if it is different.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>direct_execution</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Tailer&apos;s execution engine has been rewritten to switch from Airflow/Composer
          to a Kubernetes severless architecture, improving its speed, stability,
          security and scalability.</p>
        <p></p>
        <p>To use the new execution mode, the <b>direct_execution</b> parameter must
          be set to &quot;true&quot;.</p>
        <p></p>
        <p>If not set, the default value will be &quot;true&quot; from January 5,
          2021</p>
      </td>
    </tr>
  </tbody>
</table>

## ➿ Workflow task parameters

A Tables to Tables workflow can include three types of tasks:

* **create\_gbq\_table**: This type of task allows you to create the skeleton of a table based on a DDL file. You could compare it to the baking pan of your table.
* **sql**: This type of task allows you to fill that baking pan using a request from a SQL file.
* **copy\_gbq\_table**: This type of task allows you to duplicate a table named X into another table named Y.

For each workflow sub-object, parameters will differ depending on the task type.

{% hint style="info" %}
 Refer to [this page](table-to-table-sql-and-ddl-files.md) to know how to create the DDL and SQL files corresponding to these tasks.
{% endhint %}

### **SQL task parameters**

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
        <p><b>task_type</b>
        </p>
        <p>type : string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>The value has to be set to &quot;sql&quot; for this task type.</p>
        <p>As &quot;sql&quot; is the default type, this parameter can be omitted
          for this task type.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>id</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">ID of the task. It must be unique within the data operation.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>short_description</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Short description of what the task does.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>doc_md</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Path to a file containing a detailed description of the task. The file
        must be in Markdown format.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sql_file</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Path to the file containing the actual SQL query. This file is going to
        be read and its content uploaded to Firestore upon deployment of the data
        operation.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcp_project_id</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>GCP Project ID.</p>
        <p>Overrides <b>default_gcp_project_id</b>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>bq_dataset</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Name of the BigQuery destination dataset.</p>
        <p>Overrides <b>default_bq_dataset</b>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>write_disposition</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Action that occurs if the destination table already exists (see <a href="https://googleapis.github.io/google-cloud-python/latest/bigquery/generated/google.cloud.bigquery.job.WriteDisposition.html#google.cloud.bigquery.job.WriteDisposition">Google BigQuery documentation</a>).
          <br
          />Possible values:</p>
        <ul>
          <li>&quot;WRITE_TRUNCATE&quot; (default): The run will write table data from
            the beginning. If the table already contained lines, they will all be deleted
            and replaced by the new lines. This option is used most of the time for
            daily runs to avoid duplicates.</li>
          <li>&quot;WRITE_APPEND&quot;: The run will append new lines to the table.
            When using this option, make sure not to run the data operation several
            times.</li>
          <li>&quot;WRITE_EMPTY&quot;: This option only allows adding data to an empty
            table. If the table already contains data, it returns an error. It is hardly
            ever used as data operations are usually run periodically, so they will
            always contain data after the first run.</li>
        </ul>
        <p>Overrides <b>default_write_disposition</b>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>table_name</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Target table used upon SQL query execution.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>sql_query_template</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>If you want to use variables in your SQL query, you need to set this parameter
          to &quot;TEMPLATE_CURRENT_DATE&quot; (only supported value). This variable
          will be set to the execution date of the data operation (and not today&apos;s
          date).</p>
        <p>For example, if you want to retrieve data corresponding to the execution
          date, you can use the following instruction:</p>
        <p><code>WHERE sale_date = DATE(&apos;</code>{{TEMPLATE_CURRENT_DATE}}<code>&apos;)</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

### **Table creation task parameters**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Parameter</b>
      </th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>task_type</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">In this case, the value has to be set to &quot;create_gbq_table&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>id</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">ID of the task. It must be unique within the data operation.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>short_description</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Short description of what the task does.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>doc_md</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Path to a file containing a detailed description of the task. The file
        must be in Markdown format.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>bq_table</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Name of the BigQuery table name.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>ddl_file</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Path to the JSON file containing DDL information to create the table.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>force_delete</b>
        </p>
        <p>type: boolean</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">If set to &quot;true&quot;, this parameter will force the deletion of
        the table prior to its creation.</td>
    </tr>
  </tbody>
</table>

### **Table copy task parameters**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Parameter</b>
      </th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>task_type</b>
        </p>
        <p>type : string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">In this case, the value has to be set to &quot;copy_gbq_table&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>id</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">ID of the task. It must be unique within the data operation.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>short_description</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Short description of what the task does.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>doc_md</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Path to a file containing a detailed description. The file must be in
        Markdown format.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>source_gcp_project_id</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">GCP project ID for the source BigQuery table.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>source_bq_dataset</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">BigQuery dataset for the source table.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>source_bq_table</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Name of the source table.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>gcp_project_id</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>GCP project ID for the destination BigQuery table.</p>
        <p>Overrides <b>default_gcp_project_id</b>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>bq_dataset</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Name of the BigQuery destination dataset.</p>
        <p>Overrides <b>default_bq_dataset</b>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>destination_bq_table</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Name of the BigQuery destination table.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>destination_bq_table_date_suffix</b>
        </p>
        <p>type: boolean</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Enables/Disables the date suffix to allow date partitioning in BigQuery.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>destination_bq_table_date_suffix_format</b>
        </p>
        <p>type: string</p>
        <p>mandatory if <b>destination_bq_table_date_suffix</b> is set to true</p>
      </td>
      <td style="text-align:left">
        <p>Date format.
          <br />fixed value: %Y%m%d
          <br />
        </p>
        <p><a href="https://docs.python.org/fr/3.6/library/datetime.html?highlight=strftime#strftime-strptime-behavior">https://docs.python.org/fr/3.6/library/datetime.html?highlight=strftime#strftime-strptime-behavior</a>
        </p>
      </td>
    </tr>
  </tbody>
</table>

