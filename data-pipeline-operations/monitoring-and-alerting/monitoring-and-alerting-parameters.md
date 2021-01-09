---
description: >-
  Learn how to add monitoring and alerting information to your data operation
  configurations.
---

# Monitoring and alerting parameters

The monitoring and alerting parameters are defined in a JSON object added at the root level of the JSON data operation configuration. It contains the following sections:

* Monitoring parameters: General information about the criticality of the data operation.
* Alerting parameters: One or several alert messages and channels, containing information about how to alert the right people throw which system or application.

{% hint style="warning" %}
Currently, you can only send on alert message, and only by email. But we are thinking about adding more alert systems like Pagerduty, Datadog, or throw a generic web-hook. Feel free to suggest to us your preferred alerting platform.
{% endhint %}

##  Example

Here is an example in a TTT configuration file:

```text
{
	"configuration_type": "table-to-table",
	"configuration_id": "000001_append_tailer_activities_runs",
	"short_description": "Append the Tailer runs from the firestore streamed view to a partitioned table",
	"account": "000001",
	"environment": "DEV",
	"activated": true,
	"archived": false,
	"start_date": "2019, 1, 23",
	"schedule_interval": "*/5 * * * *",
	"max_active_runs": 1,
	"task_concurrency": 3,
	"default_gcp_project_id": "fd-jarvis-datalake",
	"default_bq_dataset": "jarvis_activities",
	"default_write_disposition": "WRITE_TRUNCATE",
	"direct_execution": true,
	"task_dependencies": [
		"create_tailer_activities_runs >> merge_table_with_last_data"
	],
	"workflow": [
		{
			"task_type": "create_gbq_table",
			"id": "create_tailer_activities_runs",
			"short_description": "Create the destination table with partitioning on date and clustering",
			"bq_table": "all_jarvis_runs_raw_latest_table",
			"ddl_file": "create_table_tailer_activities_runs.json",
			"force_delete": false
		},
		{
			"task_type": "run_gbq_script",
			"id": "merge_table_with_last_data",
			"sql_file": "merge_table_with_last_data.sql"
		}
	],
  "monitoring": { 
    "impact": 2, 
    "urgency": 2, 
    "alert_enabled": true, 
    "alert_environment": ["PROD","DEV"],
    "alert_info": "Put here information about the alert", 
    "alert": { 
      "email" :  { 
          "email_from": "alert@brand.com", 
          "email_to": "toto@tailer.ia;titi@tailer.ia", 
          "email_reply_to": "support@brand.com", 
          "email_subject": "Data Operation Alert : @configuration_id has just failed", 
          "email_body_type": "txt",
          "email_body": "Type : @configuration_type\nID : @configuration_id\nImpact : @impact\nEnvironnement : @environnement"
        }
      }
  }
}

```

## 🌐 Global monitoring parameters

General parameters about the monitoring.

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
        <p><b>impact</b>
        </p>
        <p>type: integer</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>&apos;Impact&apos; is an ITIL measure of the extent of the Incident and
          of the potential damage caused by the Incident before it can be resolved.
          <a
          href="https://wiki.en.it-processmaps.com/index.php/Checklist_Incident_Priority">Learn more</a>
        </p>
        <p>If not specified, the default value will be 2.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>urgency</b>
        </p>
        <p>type: integer</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>&apos;Urgency&apos; is a measure of how quickly a resolution of the Incident
          is required. <a href="https://wiki.en.it-processmaps.com/index.php/Checklist_Incident_Priority">Learn more</a>
        </p>
        <p>If not specified, the default value will be 2.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>alert_enabled</b>
        </p>
        <p>type: boolean</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">Flag used to enable/disable the execution of the alerting (i.e. send an
        alert to a recipient when the run failed)</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>alert_environment</b>
        </p>
        <p>type: array</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>Specifies the environments that will trigger the alert.</p>
        <p>Possible values: PROD, PREPROD, STAGING, DEV.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>alert_info</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">Short information describing the alert.
        <br />You can refer it as a variable in your triggering message (as email) with
        the @alert_info parameter.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>alert</b>
        </p>
        <p>type: array of maps</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>List of alert messages the data operation will trigger if it fails.</p>
        <p>Check the section below for detailed information on their parameters.</p>
      </td>
    </tr>
  </tbody>
</table>

## ⚠ Alert parameters

An alert will be able to trigger different types of messages. Currently, only an email alert could be sent.

For each alert message, parameters will differ depending on the message type.

### 📨 Email Alert

An email alert will send an email with specific parameters each time the data operation fails in the specified environments. 

By default, Tailer provides an email template with detailed information. Thus, you don't have to fill in all the parameters such as subject, body, etc. But you can also personalize all the parameters to set precisely how the alert email should look like. For that, you can use alert variables described below.

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
        <p><b>email_from</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Surcharge the &quot;email from&quot; attribute.</p>
        <p>Default value: &quot;no_reply@tailer.ai&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>email_to</b>
        </p>
        <p>type: string</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p>List of email recipients</p>
        <p>You can specify more than one recipient by separating the email addresses
          with a <b>;</b>
        </p>
        <p><em>example: &quot;steve@apple.com;support@amazon.com&quot;</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>email_reply_to</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Surcharge the &quot;email reply to&quot; attribute.</p>
        <p>Default value: &quot;no_reply@tailer.ai&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>email_subject</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>The subject of the triggered alert email.</p>
        <p>Default value: &quot;TAILER RUN ALERT: @job<em>id FAILED at </em>@execution<em>_</em>date</p>
        <p>You can personalize the subject with the alert variables described below.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>email_body_type</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Format type of the email body.</p>
        <p>Default value: &quot;html&quot;</p>
        <p>Other possible value: &quot;txt&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>email_body</b>
        </p>
        <p>type: string</p>
        <p>optional</p>
      </td>
      <td style="text-align:left">
        <p>Body of the alert email.</p>
        <p>Default value: See attached html file below</p>
        <p>You can personalize the body with the alert variables described below.
          Be careful to use the right body format according to the email_body_type
          parameter.</p>
      </td>
    </tr>
  </tbody>
</table>

{% file src="../../.gitbook/assets/tailer-alert-failed-email-template.html" caption="Default Alert Failed Email Template.html" %}

### 🧩 Alert message variables

Alert messages can be personalized with alert message variables. Those variables are contextualized during the data operation run.

| Variable | Description |
| :--- | :--- |
| **@url\_to\_tailer\_studio\_run\_id** | The Tailer Studio URL of the Run |
| **@url\_to\_tailer\_studio\_conf\_id** | The Tailer Studio URL of the run current configuration id |
| **@account** | Account of the data operation's run |
| **@environment** | Environnement of the data operation's run |
| **@configuration\_type** | Configuration Type of the data operation's run |
| **@execution\_date** | Executation Date of the data operation's run |
| **@short\_description** | Short Description of the data operation |
| **@job\_id** | Job Id of the data operation |
| **@run\_id** | Run Id of the data operation's run |
| **@updated\_by** | The email of the user who last updates the configuration |
| **@update\_date** | The Date Tile of the last configuration update |
| **@impact** | The Monitoring Impact Level parameter |
| **@urgency** | The Monitoring Urgency Level parameter |
| **@alert\_info** | The alert info set in the monitoring parameter |

