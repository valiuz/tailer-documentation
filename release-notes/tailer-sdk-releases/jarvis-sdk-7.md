# jarvis-sdk

* [ Project description](jarvis-sdk-7.md#description)
* [ Project details](jarvis-sdk-7.md#data)
* [ Release history](jarvis-sdk-7.md#history)
* [ Download files](jarvis-sdk-7.md#files)

### Project description

## Jarvis SDK by Fashiondata.io

### Changelog

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

| Files for jarvis-sdk, version 1.0.2 |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| Filename, size | File type | Python version | Upload date | Hashes |
|  Filename, size [jarvis\_sdk-1.0.2-py3-none-any.whl](https://files.pythonhosted.org/packages/31/21/c86785d3a16a856f1d65fe8eda798f9475e73a570f3cb49d6b4651bac6c2/jarvis_sdk-1.0.2-py3-none-any.whl) \(25.0 kB\) |  File type Wheel |  Python version py3 |  Upload date Feb 10, 2020 |  Hashes [View](jarvis-sdk-7.md#copy-hash-modal-d37996bc-480b-4b98-80b1-a4cb98523da6) |

[ Close](jarvis-sdk-7.md#modal-close)

####  [Hashes](https://pip.pypa.io/en/stable/reference/pip_install/#hash-checking-mode) for jarvis\_sdk-1.0.2-py3-none-any.whl

| Hashes for jarvis\_sdk-1.0.2-py3-none-any.whl |  |  |
| :--- | :--- | :--- |
| Algorithm | Hash digest |  |
| SHA256 | `4c9fb3056178e225d39a295f4e4d308afff7f1efc08136b1ae8c928c33f5bcc2` |  |
| MD5 | `88046317ad13d4aeeaf42ed3860c707f` |  |
| BLAKE2-256 | `3121c86785d3a16a856f1d65fe8eda798f9475e73a570f3cb49d6b4651bac6c2` |  |

