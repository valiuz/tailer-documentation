# jarvis-sdk

* [ Project description](jarvis-sdk-9.md#description)
* [ Project details](jarvis-sdk-9.md#data)
* [ Release history](jarvis-sdk-9.md#history)
* [ Download files](jarvis-sdk-9.md#files)

### Project description

## Jarvis SDK by Fashiondata.io

### Changelog

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

| Files for jarvis-sdk, version 1.0.0 |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| Filename, size | File type | Python version | Upload date | Hashes |
|  Filename, size [jarvis\_sdk-1.0.0-py3-none-any.whl](https://files.pythonhosted.org/packages/a9/d1/87070777c5b48a2dbb0b2605f0ee4b488a89567ef3ed24788d9358c1051d/jarvis_sdk-1.0.0-py3-none-any.whl) \(25.0 kB\) |  File type Wheel |  Python version py3 |  Upload date Feb 6, 2020 |  Hashes [View](jarvis-sdk-9.md#copy-hash-modal-3e504e57-3264-46c1-a482-02294e095978) |

[ Close](jarvis-sdk-9.md#modal-close)

####  [Hashes](https://pip.pypa.io/en/stable/reference/pip_install/#hash-checking-mode) for jarvis\_sdk-1.0.0-py3-none-any.whl

| Hashes for jarvis\_sdk-1.0.0-py3-none-any.whl |  |  |
| :--- | :--- | :--- |
| Algorithm | Hash digest |  |
| SHA256 | `d6e5ba16b609db1d79efe928082417c8e33c36ed27131d03fabb70375130c56f` |  |
| MD5 | `d876fef399e21940bea7fe585d5e236e` |  |
| BLAKE2-256 | `a9d187070777c5b48a2dbb0b2605f0ee4b488a89567ef3ed24788d9358c1051d` |  |

