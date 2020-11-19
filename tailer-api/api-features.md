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
TAILER_API_API=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_API}" \
--data '{ "action": "launch_job",
          "account": "000110",
          "environment": "DEV",
          "configuration_type": "gbq-to-gbq",
          "configuration_id": "000110-tailer-TTT-POC-ETL-VTE_DEV",
          "job_id": "gbq-to-gbq|000110-tailer-TTT-POC-ETL-VTE_DEV",
          "execution_date": "2020-11-09"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/launch"
```

As a result, the job is launched and you get a unique run ID in the following format: 

```bash
"run_id": "20201105-1588f12a-73f0-4f5c-8592-5cd3ce793a29"
```

## 🏃♂ Checking a run's status

Once you have a run ID, you can check its current status to see how the processing is going.

You need to provide the full identity of the job and the run ID as input:

```bash
TAILER_API_API=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_API}" \
--data '{"action": "check_run_status",
         "account": "000110",
         "environment": "DEV",
         "configuration_type": "gbq-to-gbq",
         "configuration_id": "000110-tailer-TTT-POC-ETL-VTE_DEV",
         "job_id": "gbq-to-gbq|000110-tailer-TTT-POC-ETL-VTE_DEV",
         "run_id": "20201105-1588f12a-73f0-4f5c-8592-5cd3ce793a29"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

## ⌛ Getting the last status of a job/data operation

You can check the current status for the latest run of a job/data operation.

You need to provide the full identity of the job/data operation as input.

If needed, you can specify some parameters for the job/data operation in order to target a specific one.

**Example with a job**

```bash
TAILER_API_API=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_API}" \
--data '{ "action": "get_last_status", 
          "account": "000110", 
          "environment": "DEV", 
          "configuration_type": "storage-to-storage",
          "configuration_id": "000110-tailer-STS-POC-ETL-VTE",
          "job_id": "storage-to-storage|000110-tailer-STS-POC-ETL-VTE"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

**Example with a data operation**

```bash
TAILER_API_API=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_API}" \
--data '{ "action": "get_last_status", 
          "account": "000110", 
          "environment": "DEV", 
          "configuration_type": "storage-to-storage",
          "configuration_id": "000110-tailer-STS-POC-ETL-VTE"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

**Example with a data operation with specific parameters**

```bash
TAILER_API_API=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_API}" \
--data '{ "action": "get_last_status", 
          "account": "000110", 
          "environment": "DEV", 
          "configuration_type": "storage-to-storage",
          "configuration_id": "000110-tailer-STS-POC-ETL-VTE"
          "execution_date": "2020/11/19",
          "limit": 10}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

## Resetting a workflow

## Disabling a data operation

## Enabling a data operation

