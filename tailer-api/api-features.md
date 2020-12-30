---
description: >-
  This page describes the different actions that can be performed with the
  Tailer API.
---

# API features

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

As a result, you get a json payload with information about the last runs of this conf/job in the following format: 

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

**Example with a data operation**

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "get_last_status", 
          "account": "000110", 
          "environment": "DEV", 
          "configuration_type": "storage-to-storage",
          "configuration_id": "000110-tailer-STS-POC-ETL-VTE"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

**Example with a data operation with specific parameters**

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "get_last_status", 
          "account": "000110", 
          "environment": "DEV", 
          "configuration_type": "storage-to-storage",
          "configuration_id": "000110-tailer-STS-POC-ETL-VTE"
          "execution_date": "2020/11/19",
          "limit": 10}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
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
          "account": "000110", 
          "environment": "DEV", 
          "configuration_type": "workflow",
          "configuration_id": "000110-tailer-workflow-count"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/workflow/status"
```

## ⏹ Disabling a data operation

You can disable a data operation using the API. \(This feature is also available in [Tailer Studio](../tailer-studio/monitor-data-operations-status.md#display-or-edit-the-status-of-a-data-operation-execution).\)

You need to provide the full identity of the data operation as input:

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "deactivate_conf",
          "action": "get_last_status",
          "account": "000110", 
          "environment": "DEV", 
          "configuration_type": "gbq-to-gbq",
          "configuration_id": "000110-tailer-TTT-POC-ETL-VTE_DEV"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/configuration/status"
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
--data '{ "action": "activate_conf",
          "action": "get_last_status",
          "account": "000110", 
          "environment": "DEV", 
          "configuration_type": "gbq-to-gbq",
          "configuration_id": "000110-tailer-TTT-POC-ETL-VTE_DEV"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/configuration/status"
```

