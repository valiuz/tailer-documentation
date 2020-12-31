---
description: >-
  This page describes the different actions that can be performed with the
  Tailer API.
---

# API features

{% hint style="warning" %}
Currently, the API features are still referencing the **table-to-table** configuration, job and runs as a **gbq-to-gbq** configuration type. It should change in the future.
{% endhint %}

{% hint style="warning" %}
For the table-to-table configurations, don't forget to add at the end of the configuration_id the environnement \(DEV, PROD, etc.\) before querying it throw the APIs._  
Example :  the table-to-table configuration\_id "000099\_iowa\_liquor\_prepare\_pda" in your json file in DEV environnement is referenced as the "000099\_iowa\_liquor\_prepare\_pda\_DEV" in the payload below.
{% endhint %}

## 🚀 Launching a job's execution

Tables to Tables or Tables to Storage data operations can be launched through the Tailer API.

You need to provide the full identity of the job as input \(the **execution\_date** parameter is optional\):

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "launch_job",
          "account": "000099",
          "environment": "DEV",
          "configuration_type": "gbq-to-gbq",
          "configuration_id": "000099_iowa_liquor_prepare_pda_DEV",
          "job_id": "gbq-to-gbq|000099_iowa_liquor_prepare_pda_DEV"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/launch"
```

As a result, the job is launched and you get a unique run ID in the following format: 

```bash
{"run_id":"20201230-113940-1fb692b0-3531-4578-9919-acf6fdb0a5b1"}
```

## 🏃♂ Checking a run's status

Once you have a run ID, you can check its current status to see how the processing is going.

You need to provide the full identity of the job and the run ID as input:

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{"action": "check_run_status",
         "account": "000099",
         "environment": "DEV",
         "configuration_type": "gbq-to-gbq",
         "configuration_id": "000099_iowa_liquor_prepare_pda_DEV",
         "job_id": "gbq-to-gbq|000099_iowa_liquor_prepare_pda_DEV",
         "run_id": "20201230-112837-0a795c70-2557-4a60-ba16-788aa2bea179"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

As a result, you get a json payload with information about the run in the following format: 

```typescript
{
	"results": [
		{
			"account": "000099",
			"configuration_id": "000099_iowa_liquor_prepare_pda_DEV",
			"configuration_type": "gbq-to-gbq",
			"duration": "0:01:24.002519",
			"environment": "DEV",
			"job_id": "gbq-to-gbq|000099_iowa_liquor_prepare_pda_DEV",
			"last_update_date": "2020-12-30T11:30:07.960767+00:00",
			"run_id": "20201230-112837-0a795c70-2557-4a60-ba16-788aa2bea179",
			"start_execution_date": "2020-12-30T11:28:43.861340+00:00",
			"status": "SUCCESS"
		}
	]
}
```

## ⌛ Getting the last status of a job/data operation

You can check the current status for the latest run of a job/data operation.

You need to provide the full identity of the job/data operation as input. If needed, you can specify some parameters for the job/data operation in order to target a specific one:

**Example with a job**

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "get_last_status",
          "account": "000099",
          "environment": "DEV",
          "configuration_type": "gbq-to-gbq",
          "configuration_id": "000099_iowa_liquor_prepare_pda_DEV",
          "job_id": "gbq-to-gbq|000099_iowa_liquor_prepare_pda_DEV"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

As a result, you get a json payload with information about the last runs of this job in the following format: 

```bash
{
	"results": [
		{
			"account": "000099",
			"configuration_id": "000099_iowa_liquor_prepare_pda_DEV",
			"configuration_type": "gbq-to-gbq",
			"duration": "0:01:24.002519",
			"environment": "DEV",
			"job_id": "gbq-to-gbq|000099_iowa_liquor_prepare_pda_DEV",
			"last_update_date": "2020-12-30T11:30:07.960767+00:00",
			"run_id": "20201230-112837-0a795c70-2557-4a60-ba16-788aa2bea179",
			"start_execution_date": "2020-12-30T11:28:43.861340+00:00",
			"status": "SUCCESS"
		},
		{
			"account": "000099",
			"configuration_id": "000099_iowa_liquor_prepare_pda_DEV",
			"configuration_type": "gbq-to-gbq",
			"duration": "0:01:35.345232",
			"environment": "DEV",
			"job_id": "gbq-to-gbq|000099_iowa_liquor_prepare_pda_DEV",
			"last_update_date": "2020-12-30T11:10:49.097747+00:00",
			"run_id": "20201230-110907-47b31b2d-a54a-4a95-a3a6-b4fbc2168333",
			"start_execution_date": "2020-12-30T11:09:13.665798+00:00",
			"status": "SUCCESS"
		},
		{
			"account": "000099",
			"configuration_id": "000099_iowa_liquor_prepare_pda_DEV",
			"configuration_type": "gbq-to-gbq",
			"duration": "0:01:39.201091",
			"environment": "DEV",
			"job_id": "gbq-to-gbq|000099_iowa_liquor_prepare_pda_DEV",
			"last_update_date": "2020-12-10T14:07:56.739284+00:00",
			"run_id": "20201210-140612-19e547a1-de6a-47f0-b563-b8a3d5754118",
			"start_execution_date": "2020-12-10T14:06:17.415294+00:00",
			"status": "SUCCESS"
		}
	]
}

```

**Example with a data operation \(configuration level\)**

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "get_last_status",
          "account": "000099",
          "environment": "DEV",
          "configuration_type": "storage-to-storage",
          "configuration_id": "000099-jarvis-demo-iowa-liquor-storage-to-storage"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

As a result, you get a json payload with information about the last runs of this configuration in the following format: 

```bash
{
	"results": [
		{
			"account": "000099",
			"configuration_id": "000099-jarvis-demo-iowa-liquor-storage-to-storage",
			"configuration_type": "storage-to-storage",
			"duration": "0:00:03.885654",
			"environment": "DEV",
			"job_id": "storage-to-storage|000099|000099-jarvis-demo-iowa-liquor-storage-to-storage|DEV|stores-{{FD_DATE}}-{{FD_TIME}}.csv",
			"run_id": "20201209-cf026ae9-c79c-4cf9-8ffb-0bb63ad39469",
			"start_execution_date": "2020-12-09T17:17:23+00:00",
			"status": "SUCCESS"
		},
		{
			"account": "000099",
			"configuration_id": "000099-jarvis-demo-iowa-liquor-storage-to-storage",
			"configuration_type": "storage-to-storage",
			"duration": "0:00:04.373931",
			"environment": "DEV",
			"job_id": "storage-to-storage|000099|000099-jarvis-demo-iowa-liquor-storage-to-storage|DEV|stores-{{FD_DATE}}-{{FD_TIME}}.csv",
			"run_id": "20201209-7b7b7a66-1cc0-4f09-a893-ce4a35adf09b",
			"start_execution_date": "2020-12-09T17:17:18+00:00",
			"status": "SUCCESS"
		},
		{
			"account": "000099",
			"configuration_id": "000099-jarvis-demo-iowa-liquor-storage-to-storage",
			"configuration_type": "storage-to-storage",
			"duration": "0:00:04.295537",
			"environment": "DEV",
			"job_id": "storage-to-storage|000099|000099-jarvis-demo-iowa-liquor-storage-to-storage|DEV|stores-{{FD_DATE}}-{{FD_TIME}}.csv",
			"run_id": "20201209-4d76a90a-b092-4c69-a87f-b0cd79305428",
			"start_execution_date": "2020-12-09T17:17:18+00:00",
			"status": "SUCCESS"
		}
	]
}

```

**Example with a data operation \(configuration\) with specific parameters**

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "get_last_status",
          "account": "000099",
          "environment": "DEV",
          "configuration_type": "storage-to-storage",
          "configuration_id": "000099-jarvis-demo-iowa-liquor-storage-to-storage",
          "execution_date": "2020-12-09",
          "status": "SUCCESS",
          "limit": 2}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

As a result, you get a json payload with information about the last 2 runs ins success, with a start date the 2020/12/09 of this configuration in the following format: 

```bash
{
	"results": [
		{
			"account": "000099",
			"configuration_id": "000099-jarvis-demo-iowa-liquor-storage-to-storage",
			"configuration_type": "storage-to-storage",
			"duration": "0:00:03.885654",
			"environment": "DEV",
			"job_id": "storage-to-storage|000099|000099-jarvis-demo-iowa-liquor-storage-to-storage|DEV|stores-{{FD_DATE}}-{{FD_TIME}}.csv",
			"run_id": "20201209-cf026ae9-c79c-4cf9-8ffb-0bb63ad39469",
			"start_execution_date": "2020-12-09T17:17:23+00:00",
			"status": "SUCCESS"
		},
		{
			"account": "000099",
			"configuration_id": "000099-jarvis-demo-iowa-liquor-storage-to-storage",
			"configuration_type": "storage-to-storage",
			"duration": "0:00:04.373931",
			"environment": "DEV",
			"job_id": "storage-to-storage|000099|000099-jarvis-demo-iowa-liquor-storage-to-storage|DEV|stores-{{FD_DATE}}-{{FD_TIME}}.csv",
			"run_id": "20201209-7b7b7a66-1cc0-4f09-a893-ce4a35adf09b",
			"start_execution_date": "2020-12-09T17:17:18+00:00",
			"status": "SUCCESS"
		}
	]
}
```

## 0⃣ Resetting a workflow

The Tailer API allows you to reset a Workflow data operation. The reset feature deletes all the triggered jobs so the workflow can start from scratch, as when the it was just deployed. \(This feature is also available in [Tailer Studio](../tailer-studio/reset-workflow-data-operations.md).\)

**Example of a case requiring a workflow reset**

We have three jobs, named JA, JB, and JC which trigger a job named JT when they are all successfully executed.

If a situation happens where JA and JB are OK, but JC is not, JT is not triggered. You fix and relaunch JC, which becomes OK, and JT is triggered. The next morning, you launch JC again to make sure it works: you get JA\(0\), JB\(0\) and JC\(1\). When JA and JB are automatically started a few hours later, JC is already considered as OK, which creates an unbalanced situation. A reset is necessary.

You need to provide the full identity of the Workflow data operation as input:

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "reset_workflow_status", 
          "account": "000099", 
          "environment": "DEV", 
          "configuration_type": "workflow",
          "configuration_id": "000099-iowa-liquor-load-pda"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/workflow/status"
```

As a result, you get a response \(OK, or KO\) in the following format: 

```bash
{"message":"ok"}
```

## ⏹ Disabling a data operation

You can disable a data operation \(configuration\) using the API. \(This feature is also available in [Tailer Studio](../tailer-studio/monitor-data-operations-status.md#display-or-edit-the-status-of-a-data-operation-execution).\)

You need to provide the full identity of the data operation as input:

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "set_configuration_status",
          "activated" : false,
          "archived" : false,
          "account": "000099", 
          "environment": "DEV", 
          "configuration_type": "gbq-to-gbq",
          "configuration_id": "000099_Load_PDA_f_traffic_ma_DEV"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/configuration/status"
```

As a result, you get a response \(OK, or KO\) in the following format: 

```bash
{"message":"ok"}
```

## ▶ Enabling a data operation

You can enable a data operation that had been disabled using the API. \(This feature is also available in [Tailer Studio](../tailer-studio/monitor-data-operations-status.md#display-or-edit-the-status-of-a-data-operation-execution).\)

You need to provide the full identity of the data operation as input:

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "set_configuration_status",
          "activated" : true,
          "archived" : false,
          "account": "000099", 
          "environment": "DEV", 
          "configuration_type": "gbq-to-gbq",
          "configuration_id": "000099_Load_PDA_f_traffic_ma_DEV"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/configuration/status"
```

As a result, you get a response \(OK, or KO\) in the following format: 

```bash
{"message":"ok"}
```

## ▶ Archive a data operation

You can archive a data operation using the API. \(This feature is also available in [Tailer Studio](../tailer-studio/monitor-data-operations-status.md#display-or-edit-the-status-of-a-data-operation-execution).\)

You need to provide the full identity of the data operation as input:

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "set_configuration_status",
          "activated" : false,
          "archived" : true,
          "account": "000099", 
          "environment": "DEV", 
          "configuration_type": "gbq-to-gbq",
          "configuration_id": "000099_Load_PDA_f_traffic_ma_DEV"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/configuration/status"
```

As a result, you get a response \(OK, or KO\) in the following format: 

```bash
{"message":"ok"}
```



