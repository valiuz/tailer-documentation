---
description: >-
  To use the Tailer API, you first need to generate a JWT token from Google
  credentials in order to authenticate. Then you can send a first POST request
  to it using cURL.
---

# Getting started

## 🔐 Generate a JWT token from Google credentials

{% file src="../.gitbook/assets/google-jwt-generator.zip" caption="Google JWT generator" %}

To generate a Google JWT token:

1. Download and unzip the ZIP file above. You get a Python file named "google-jwt-generator.py".
2. Generate a Google JSON credentials file \(see [Generate JSON credentials](../getting-started/set-up-google-cloud-platform.md#generate-json-credentials)\).
3. Run the following script using Python:

   ```text
   python3 google-jwt-generator.py your-credentials.json
   ```

4. You can save the token in a shell variable:

   ```text
   TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json`
   ```

## 1 Send a first request to the Tailer API

To communicate with the Tailer API, you can use the cURL CLI.

Here is an example of request allowing you to check the status of a Tables to Tables data operation run:

```bash
TAILER_API_JWT=`python3 google-jwt-generator.py your-credentials.json` \
&& curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TAILER_API_JWT}" \
--data '{ "action": "check_run_status", 
          "account": "000110", 
          "environment": "DEV", 
          "run_id": "20201104-050111-cac8abe4-449f-4b9a-98d5-114bfc848da1",
          "job_id": "gbq-to-gbq|000110-tailer-TTT-POC-ETL-VTE_DEV",
          "configuration_type": "gbq-to-gbq",
          "configuration_id": "000110-tailer-TTT-POC-ETL-VTE_DEV"}' \
"https://tailer-api-nqonovswsq-ew.a.run.app/v1/dag/status"
```

