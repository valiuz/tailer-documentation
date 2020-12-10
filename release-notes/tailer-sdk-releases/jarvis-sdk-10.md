# jarvis-sdk

* [ Project description](jarvis-sdk-10.md#description)
* [ Project details](jarvis-sdk-10.md#data)
* [ Release history](jarvis-sdk-10.md#history)
* [ Download files](jarvis-sdk-10.md#files)

### Project description

## Jarvis SDK by Fashiondata.io

### Changelog

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

| Files for jarvis-sdk, version 0.0.15 |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| Filename, size | File type | Python version | Upload date | Hashes |
|  Filename, size [jarvis\_sdk-0.0.15-py3-none-any.whl](https://files.pythonhosted.org/packages/04/be/2639939184225f671471c168c47190904f717c88b1d323bec79f56023b6b/jarvis_sdk-0.0.15-py3-none-any.whl) \(24.4 kB\) |  File type Wheel |  Python version py3 |  Upload date Dec 19, 2019 |  Hashes [View](jarvis-sdk-10.md#copy-hash-modal-e3baf8ac-2ffc-4ac7-817f-199bf0d5c16c) |

[ Close](jarvis-sdk-10.md#modal-close)

####  [Hashes](https://pip.pypa.io/en/stable/reference/pip_install/#hash-checking-mode) for jarvis\_sdk-0.0.15-py3-none-any.whl

| Hashes for jarvis\_sdk-0.0.15-py3-none-any.whl |  |  |
| :--- | :--- | :--- |
| Algorithm | Hash digest |  |
| SHA256 | `1bd9747bc41dd554e0a9f5cba47978fed5084437d47aa2eb8d8dc92b816293ea` |  |
| MD5 | `8cbd62118e83bf51430d1a363b622839` |  |
| BLAKE2-256 | `04be2639939184225f671471c168c47190904f717c88b1d323bec79f56023b6b` |  |

