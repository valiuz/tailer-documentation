# jarvis-sdk

### Project description

## Jarvis SDK by Fashiondata.io

### Changelog

#### Release 1.1.1 : 2020-05-18

* Added support for ZSH under Max OS X
* TTT : Added task status
* TTT : added special check for tasks declared in "task\_dependencies" but not in "workflow"
* TTT : check if task IDs are named properly
* TTT : You can run tasks locally with : jarvis configuration run YOUR-CONF.json \[task1 task2 .... taskN\]
* Project Profiles list is now sorted

#### Release 1.1.0 : 2020-02-19

* Added TTT DAG file checking upon deployment. The user must validate if he wants to overwrite TTT DAG.
* Removed Project selection validation.

#### Release 1.0.1 : 2020-02-10

* Fixed seek\(\) error on file read

#### Release 0.0.16 : 2020-01-10

* Removed ASCII art
* Storage-to-tables : check for JSON syntax for DDL files as well

#### Release 0.0.15 : 2019-xx-xx

* Table-To-Table : GBQ table schema will be preserved upon WRITE\_TRUNCATE
* Minor fixes

#### Release 0.0.14 : 2019-11-26

* Table-To-Table : added nested RECORD fields support for Bigquery table creation.

#### Release 0.0.13 : 2019-11-15

* Storage-To-Tables : "jarvis create configuration" support

#### Release 0.0.12 : 2019-11-04

* Added Storage-To-Tables configuration support.
* Table To Table \(gbq to gbq\) : "create\_gbq\_table" tasks will now use an external DDL file to describe the table schema.

#### Release 0.0.11 : 2019-10-23

* Table To Table \(gbq to gbq\) configuration will now use "configuration\_type" and "configuration\_id".
* Table To Table \(gbq to gbq\) configuration DOES NOT need "dag\_name" parameter anymore.
* Storage To Storage configuration accepts "gcp\_project\_id" in the "source" section if the "source\_type" is "gcs".

#### Release 0.0.10 : 2019-09-20

* Added auto creation of ".bash\_profile" for Max OS X
* Added password double check upon user creation
* Added SQL file path management for "table-to-storage" deploy command
* Added : jarvis create configuration CONFIGURATION\_TYPE command
* Added : jarvis check configuration CONFIG.json command
* Added : project profiles management

### Download files

Download the file for your platform. If you're not sure which to choose, learn more about [installing packages](https://packaging.python.org/installing/).

| Files for jarvis-sdk, version 1.1.1 |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| Filename, size | File type | Python version | Upload date | Hashes |
|  Filename, size [jarvis\_sdk-1.1.1-py3-none-any.whl](https://files.pythonhosted.org/packages/3f/a9/0996d885fda8bfdf486544b9b1d4b1e5438a4c86010e8e56fc52531b5ce0/jarvis_sdk-1.1.1-py3-none-any.whl) \(28.3 kB\) |  File type Wheel |  Python version py3 |  Upload date Jun 15, 2020 |  Hashes [View](jarvis-sdk-5.md#copy-hash-modal-92e93694-e48e-486b-bf66-d858450673bd) |

