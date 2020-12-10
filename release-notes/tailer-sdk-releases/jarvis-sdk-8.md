# jarvis-sdk

* [ Project description](jarvis-sdk-8.md#description)
* [ Project details](jarvis-sdk-8.md#data)
* [ Release history](jarvis-sdk-8.md#history)
* [ Download files](jarvis-sdk-8.md#files)

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

| Files for jarvis-sdk, version 1.0.1 |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| Filename, size | File type | Python version | Upload date | Hashes |
|  Filename, size [jarvis\_sdk-1.0.1-py3-none-any.whl](https://files.pythonhosted.org/packages/8e/6a/f0f286d512b808607bfe10c18be8ca41d658f3068e544773569e9ad25df2/jarvis_sdk-1.0.1-py3-none-any.whl) \(25.0 kB\) |  File type Wheel |  Python version py3 |  Upload date Feb 10, 2020 |  Hashes [View](jarvis-sdk-8.md#copy-hash-modal-434468e1-b4dc-4649-b4b8-7f45a4a4ee93) |

[ Close](jarvis-sdk-8.md#modal-close)

####  [Hashes](https://pip.pypa.io/en/stable/reference/pip_install/#hash-checking-mode) for jarvis\_sdk-1.0.1-py3-none-any.whl

| Hashes for jarvis\_sdk-1.0.1-py3-none-any.whl |  |  |
| :--- | :--- | :--- |
| Algorithm | Hash digest |  |
| SHA256 | `63d6b43d5485eadf5ef9b75e407de8892c1e2cad8d9b8a42a7dcd79702dc9068` |  |
| MD5 | `4368b24e07ba22aa4637742af619fe33` |  |
| BLAKE2-256 | `8e6af0f286d512b808607bfe10c18be8ca41d658f3068e544773569e9ad25df2` |  |

