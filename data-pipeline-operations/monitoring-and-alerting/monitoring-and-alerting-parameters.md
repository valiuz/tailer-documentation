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
Currently, you can only send on alert message, and only by email. But we are thinking about adding more alert system like Pagerduty, Datadog or throw a generic web-hook. Feel free to suggest us your preferred alerting platform.
{% endhint %}

##  Example

Here is an example of TTT configuration file:

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
    "alert_environnement": ["PROD","DEV"],
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
        <p>&apos;Impact&apos; is a ITIL measure of the extent of the Incident and
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
        <p>&apos;Urgency&apos; is a measure how quickly a resolution of the Incident
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
  </tbody>
</table>

