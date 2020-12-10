# jarvis-sdk

### Project description

## Jarvis SDK by Fashiondata.io

### Changelog

#### Release 1.1.5 : 2020-11-04

* TTT : the DAG can be deployed in direct execution mode.
* TTT : the user is asked if he wants to execute teh DAG upon successful upload.
* TTT : the user is not asked for the project profile. The configuration attribute "default\_gcp\_project\_id" is used automatically.
* Updated "setup.py"
* Added file encoding verification upon reading. Everything will be converted to UTF-8
* Added specific Google Drive scope to generated DAG to allow Google Sheet as GBQ source
* When a "workflow" type configuration is uploaded, the latest workflow status \(if it exists\) is deleted.
* The used is asked if he wants to deploy associated cloud functions upon STS and STT deployment.
* STT : DDL infos can be processed using different methods

#### Release 1.1.4 : 2020-07-03

* Added Notification upon Jarvis SDK new release on PyPi
* TTT : added "client\_type" and "client\_version" to the stored configuration

#### Release 1.1.3 : 2020-06-18

* TTT : fixed another regression on function call.

#### Release 1.1.2 : 2020-06-18

* TTT : fixed a regression impacting SQL type tasks.

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

| Files for jarvis-sdk, version 1.1.5 |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| Filename, size | File type | Python version | Upload date | Hashes |
|  Filename, size [jarvis\_sdk-1.1.5-py3-none-any.whl](https://files.pythonhosted.org/packages/93/73/ccd2c3ef72a89ea609d4236310978b874bc6e567715155e3f5442a0283d7/jarvis_sdk-1.1.5-py3-none-any.whl) \(36.7 kB\) |  File type Wheel |  Python version py3 |  Upload date Nov 4, 2020 |  Hashes [View](jarvis-sdk-1.md#copy-hash-modal-71c44f30-7021-41c9-a7b2-8381c3a29c3d) |

