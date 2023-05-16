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

## Example

Here is an example in a TTT configuration file:

```json
{
	"configuration_type": "table-to-table",
	"configuration_id": "000001_append_some_data",
	"short_description": "Append some data to a partitioned table",
	"account": "000099",
	"environment": "DEV",
	"activated": true,
	"archived": false,
	"start_date": "2023, 1, 23",
	"schedule_interval": "*/5 * * * *",
	"max_active_runs": 1,
	"task_concurrency": 3,
	"default_gcp_project_id": "my-project",
	"default_bq_dataset": "my_dataset",
	"default_write_disposition": "WRITE_TRUNCATE",
	"direct_execution": true,
	"task_dependencies": [
		"create_my_data_table >> merge_table_with_last_data"
	],
	"workflow": [
		{
			"task_type": "create_gbq_table",
			"id": "create_my_data_table",
			"short_description": "Create the destination table with partitioning on date and clustering",
			"bq_table": "my_data",
			"ddl_file": "my_data.json",
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
    "alert_status": ["FAILED","NO_MATCH"], 
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

## :globe\_with\_meridians: Global monitoring parameters

General parameters about the monitoring.

| Parameter                                                                                                                                            | Description                                                                                                                                                                                                                                                                                           |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>impact</strong></p><p>type: integer</p><p>optional</p>                                                                                    | <p>'Impact' is an ITIL measure of the extent of the Incident and of the potential damage caused by the Incident before it can be resolved. <a href="https://wiki.en.it-processmaps.com/index.php/Checklist_Incident_Priority">Learn more</a></p><p>If not specified, the default value will be 2.</p> |
| <p><strong>urgency</strong></p><p>type: integer</p><p>optional</p>                                                                                   | <p>'Urgency' is a measure of how quickly a resolution of the Incident is required. <a href="https://wiki.en.it-processmaps.com/index.php/Checklist_Incident_Priority">Learn more</a></p><p>If not specified, the default value will be 2.</p>                                                         |
| <p><strong>alert_enabled</strong></p><p>type: boolean</p><p>mandatory</p>                                                                            | Flag used to enable/disable the execution of the alerting (i.e. send an alert to a recipient when the run failed)                                                                                                                                                                                     |
| <p><strong>alert_status </strong><mark style="color:green;background-color:orange;"><strong>(beta)</strong></mark><br>type: array</p><p>optional</p> | <p>Specifies the Run Status that will trigger the alert.</p><p>Possible values: FAILED, SUCCESS, NO_MATCH, CHECKED.<br>Default value: "FAILED"</p>                                                                                                                                                    |
| <p><strong>alert_environment</strong></p><p>type: array</p><p>mandatory</p>                                                                          | <p>Specifies the environments that will trigger the alert.</p><p>Possible values: PROD, PREPROD, STAGING, DEV.</p>                                                                                                                                                                                    |
| <p><strong>alert_info</strong></p><p>type: string</p><p>optional</p>                                                                                 | <p>Short information describing the alert.<br>You can refer it as a variable in your triggering message (as email) with the @alert_info parameter.</p>                                                                                                                                                |
| <p><strong>alert</strong></p><p>type: array of maps</p><p>optional</p>                                                                               | <p>List of alert messages the data operation will trigger if it fails.</p><p>Check the section below for detailed information on their parameters.</p>                                                                                                                                                |

## :warning: Alert parameters

An alert will be able to trigger different types of messages. Currently, only an email alert can be sent.

For each alert message, parameters will differ depending on the message type.

### :incoming\_envelope: Email alert

An email alert will send an email with specific parameters each time the data operation fails in the specified environments.

By default, Tailer provides an email template with detailed information. Thus, you don't have to fill in all the parameters such as subject, body, etc. But you can also personalize all the parameters to set precisely how the alert email should look like. For that, you can use the alert variables described below.

| Parameter                                                                 | Description                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>email_from</strong></p><p>type: string</p><p>optional</p>      | <p>Surcharge the "email from" attribute.</p><p>Default value: "no_reply@tailer.ai"</p>                                                                                                                                                                                |
| <p><strong>email_to</strong></p><p>type: string</p><p>mandatory</p>       | <p>List of email recipients</p><p>You can specify more than one recipient by separating the email addresses with a <strong>;</strong></p><p><em>example: "steve@apple.com;support@amazon.com"</em></p>                                                                |
| <p><strong>email_reply_to</strong></p><p>type: string</p><p>optional</p>  | <p>Surcharge the "email reply to" attribute.</p><p>Default value: "no_reply@tailer.ai"</p>                                                                                                                                                                            |
| <p><strong>email_subject</strong></p><p>type: string</p><p>optional</p>   | <p>Subject of the triggered alert email.</p><p>Default value: "TAILER RUN ALERT: @job<em>id FAILED at</em> @execution<em>_</em>date"</p><p>You can personalize the subject with the alert variables described below.</p>                                              |
| <p><strong>email_body_type</strong></p><p>type: string</p><p>optional</p> | <p>Format type of the email body.</p><p>Default value: "html"</p><p>Other possible value: "txt"</p>                                                                                                                                                                   |
| <p><strong>email_body</strong></p><p>type: string</p><p>optional</p>      | <p>Body of the alert email.</p><p>Default value: "See attached html file below"</p><p>You can personalize the body with the alert variables described below. Be careful to use the right body format according to the <strong>email_body_type</strong> parameter.</p> |

{% file src="../../.gitbook/assets/Tailer Alert Failed Email Template.html" %}
Default Alert Failed Email Template.html
{% endfile %}

### :jigsaw: Alert message variables

Alert messages can be personalized with alert message variables. Those variables are contextualized during the data operation run.

| Variable                               | Description                                                  |
| -------------------------------------- | ------------------------------------------------------------ |
| **@url\_to\_tailer\_studio\_run\_id**  | Tailer Studio URL for the run                                |
| **@url\_to\_tailer\_studio\_conf\_id** | Tailer Studio URL for the run current configuration ID       |
| **@account**                           | Account of the data operation's run                          |
| **@environment**                       | Environment of the data operation's run                      |
| **@status**                            | Status of the data operation's run                           |
| @**configuration\_id**                 | Configuration ID of the data operation                       |
| **@configuration\_type**               | Configuration type of the data operation's run               |
| **@execution\_date**                   | Execution date of the data operation's run                   |
| **@short\_description**                | Short description of the data operation                      |
| **@job\_id**                           | Job ID of the data operation                                 |
| **@run\_id**                           | Run ID of the data operation's run                           |
| **@updated\_by**                       | Email address of the user who last updated the configuration |
| **@update\_date**                      | Date tile of the last configuration update                   |
| **@impact**                            | Monitoring impact level parameter                            |
| **@urgency**                           | Monitoring urgency level parameter                           |
| **@alert\_info**                       | Alert info set in the monitoring parameter                   |
