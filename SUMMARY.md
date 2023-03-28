# Table of contents

* [What is Tailer Platform?](README.md)

## Getting Started

* [Prepare your local environment for Tailer](getting-started/prepare-your-local-environment-for-tailer.md)
* [Install Tailer SDK](getting-started/install-tailer-sdk.md)
* [Set up Google Cloud Platform](getting-started/set-up-google-cloud-platform.md)
* [Encrypt your credentials](getting-started/encrypt-your-credentials.md)

## \[Tutorial] Create a first data pipeline

* [Introduction](tutorial-create-a-first-data-pipeline/introduction.md)
* [Prepare the demonstration environment](tutorial-create-a-first-data-pipeline/prepare-the-demonstration-environment.md)
* [Copy files from one bucket to another](tutorial-create-a-first-data-pipeline/untitled.md)
* [Load files into BigQuery tables](tutorial-create-a-first-data-pipeline/load-files-into-bigquery-tables.md)
* [Prepare data](tutorial-create-a-first-data-pipeline/prepare-data.md)
* [Build predictions](tutorial-create-a-first-data-pipeline/build-predictions.md)
* [Export data](tutorial-create-a-first-data-pipeline/export-data.md)
* [Congratulations!](tutorial-create-a-first-data-pipeline/congratulations.md)
* [\[Video\] Automatic Script](tutorial-create-a-first-data-pipeline/transform-data-with-tables-to-tables/README.md)
  * [SQL script file](tutorial-create-a-first-data-pipeline/transform-data-with-tables-to-tables/table-to-table-sql-and-ddl-files.md)
  * [DDL script file](tutorial-create-a-first-data-pipeline/transform-data-with-tables-to-tables/table-to-table-sql-and-ddl-files-1.md)
  * [Tables to Tables script file](tutorial-create-a-first-data-pipeline/transform-data-with-tables-to-tables/tables-to-tables-configuration-file.md)
  * [Launch configuration and furthermore](tutorial-create-a-first-data-pipeline/transform-data-with-tables-to-tables/transform-data-with-tables-to-tables.md)

## Data Pipeline Operations

* [Overview](data-pipeline-operations/untitled.md)
* [Set constants with Context](data-pipeline-operations/set-constants-with-context/README.md)
  * [Context configuration file](data-pipeline-operations/set-constants-with-context/context-configuration-file.md)
* [Move files with Storage to Storage](data-pipeline-operations/move-files-with-storage-to-storage/README.md)
  * [Storage to Storage configuration file](data-pipeline-operations/move-files-with-storage-to-storage/storage-to-storage-configuration-file.md)
* [Load data with Storage to Tables](data-pipeline-operations/load-data-with-storage-to-tables/README.md)
  * [Storage to Tables configuration file](data-pipeline-operations/load-data-with-storage-to-tables/storage-to-tables-configuration-file.md)
  * [Storage to Tables DDL files](data-pipeline-operations/load-data-with-storage-to-tables/storage-to-tables-ddl-files.md)
* [Stream incoming data with API To Storage](data-pipeline-operations/stream-incoming-data-with-api-to-storage/README.md)
  * [API To Storage configuration file](data-pipeline-operations/stream-incoming-data-with-api-to-storage/api-to-storage-configuration-file.md)
  * [API To Storage usage examples](data-pipeline-operations/stream-incoming-data-with-api-to-storage/api-to-storage-usage-examples.md)
* [Transform data with Tables to Tables](data-pipeline-operations/transform-data-with-tables-to-tables/README.md)
  * [Tables to Tables configuration file](data-pipeline-operations/transform-data-with-tables-to-tables/tables-to-tables-configuration-file.md)
  * [Table to Table SQL and DDL files](data-pipeline-operations/transform-data-with-tables-to-tables/table-to-table-sql-and-ddl-files.md)
* [Export data with Tables to Storage](data-pipeline-operations/export-data-with-tables-to-storage/README.md)
  * [\[V3\] Table to Storage configuration file](data-pipeline-operations/export-data-with-tables-to-storage/table-to-storage-configuration-file.md)
  * [Table to Storage SQL file](data-pipeline-operations/export-data-with-tables-to-storage/table-to-storage-sql-file.md)
  * [\[V1-V2: deprecated\] Table to Storage configuration file](data-pipeline-operations/export-data-with-tables-to-storage/table-to-storage-configuration-file-1.md)
* [Orchestrate processings with Workflow](data-pipeline-operations/orchestrate-processings-with-workflow/README.md)
  * [\[V2\] Workflow configuration file](data-pipeline-operations/orchestrate-processings-with-workflow/workflow-configuration-file.md)
  * [\[V1: deprecated\] Workflow configuration file](data-pipeline-operations/orchestrate-processings-with-workflow/workflow-configuration-file-1.md)
* [Convert XML to CSV](data-pipeline-operations/xml-conversion/README.md)
  * [Convert XML to CSV configuration file](data-pipeline-operations/xml-conversion/untitled-1.md)
* [Use advanced features with VM Launcher](data-pipeline-operations/execute-code-processings-with-vm-launcher/README.md)
  * [Process code with VM Launcher](data-pipeline-operations/execute-code-processings-with-vm-launcher/process-code-with-vm-launcher/README.md)
    * [VM Launcher configuration file for code processing](data-pipeline-operations/execute-code-processings-with-vm-launcher/process-code-with-vm-launcher/vm-launcher-code-processing-configuration-file.md)
  * [Encrypt/Decrypt data with VM Launcher](data-pipeline-operations/execute-code-processings-with-vm-launcher/encrypt-decrypt-data-with-vm-launcher/README.md)
    * [VM Launcher configuration file for data encryption](data-pipeline-operations/execute-code-processings-with-vm-launcher/encrypt-decrypt-data-with-vm-launcher/vm-launcher-data-encryption-configuration-file.md)
    * [VM Launcher configuration file for data decryption](data-pipeline-operations/execute-code-processings-with-vm-launcher/encrypt-decrypt-data-with-vm-launcher/vm-launcher-data-decryption-configuration-file.md)
* [Monitoring and Alerting](data-pipeline-operations/monitoring-and-alerting/README.md)
  * [Monitoring and alerting parameters](data-pipeline-operations/monitoring-and-alerting/monitoring-and-alerting-parameters.md)
* [Asserting Data quality with Expectations](data-pipeline-operations/expectations/README.md)
  * [List of Expectations](data-pipeline-operations/expectations/list-of-expectations.md)
* [Modify files with File Utilities](data-pipeline-operations/modify-files-with-file-utilities/README.md)
  * [Encrypt/Decrypt data with File Utilities](data-pipeline-operations/modify-files-with-file-utilities/encrypt-decrypt-data-with-vm-launcher/README.md)
    * [Configuration file for data encryption](data-pipeline-operations/modify-files-with-file-utilities/encrypt-decrypt-data-with-vm-launcher/vm-launcher-data-encryption-configuration-file.md)
    * [Configuration file for data decryption](data-pipeline-operations/modify-files-with-file-utilities/encrypt-decrypt-data-with-vm-launcher/vm-launcher-data-encryption-configuration-file-1.md)
* [Transfer data with GBQ to Firestore](data-pipeline-operations/transfer-data-with-gbq-to-firestore/README.md)
  * [Table to Storage: configuration file](data-pipeline-operations/transfer-data-with-gbq-to-firestore/table-to-storage-configuration-file.md)
  * [Table to Storage: SQL file](data-pipeline-operations/transfer-data-with-gbq-to-firestore/table-to-storage-sql-file.md)
  * [VM Launcher: configuration file](data-pipeline-operations/transfer-data-with-gbq-to-firestore/vm-launcher-code-processing-configuration-file.md)
  * [File-to-firestore python file](data-pipeline-operations/transfer-data-with-gbq-to-firestore/table-to-storage-sql-file-1.md)

## Tailer Studio

* [Overview](tailer-studio/untitled.md)
* [Check data operations' details](tailer-studio/check-data-operations-details.md)
* [Monitor data operations' status](tailer-studio/monitor-data-operations-status.md)
* [Execute data operations](tailer-studio/execute-data-operations.md)
* [Reset Workflow data operations](tailer-studio/reset-workflow-data-operations.md)
* [Archive data operations](tailer-studio/archive-data-operations.md)
* [Add notes to data operations and runs](tailer-studio/add-notes-to-data-operations-and-runs.md)
* [View your data catalog](tailer-studio/view-your-data-catalog.md)

## Tailer API

* [Overview](tailer-api/overview.md)
* [Getting started](tailer-api/getting-started-1.md)
* [API features](tailer-api/api-features.md)

## Release Notes

* [Tailer SDK Stable Releases](https://pypi.org/project/tailer-sdk/)
* [Tailer Beta Releases](release-notes/tailer-beta-releases/README.md)
  * [Beta features](release-notes/tailer-beta-releases/beta-features.md)
  * [Beta configuration](release-notes/tailer-beta-releases/beta-configuration.md)
  * [Tailer SDK API](release-notes/tailer-beta-releases/tailer-sdk-api.md)
* [Tailer Status](release-notes/tailer-status.md)
