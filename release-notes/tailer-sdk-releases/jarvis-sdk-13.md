# jarvis-sdk

* [ Project description](jarvis-sdk-13.md#description)
* [ Project details](jarvis-sdk-13.md#data)
* [ Release history](jarvis-sdk-13.md#history)
* [ Download files](jarvis-sdk-13.md#files)

### Project description

## Jarvis SDK by Fashiondata.io

### Changelog

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

| Files for jarvis-sdk, version 0.0.12 |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| Filename, size | File type | Python version | Upload date | Hashes |
|  Filename, size [jarvis\_sdk-0.0.12-py3-none-any.whl](https://files.pythonhosted.org/packages/b9/24/f7cb5a4d466cfde440460b8b19d4cab54154a2cc1fea1e259eacb955805e/jarvis_sdk-0.0.12-py3-none-any.whl) \(23.9 kB\) |  File type Wheel |  Python version py3 |  Upload date Nov 18, 2019 |  Hashes [View](jarvis-sdk-13.md#copy-hash-modal-b52ab8a1-2130-487e-b201-32ccde9ecd3e) |

[ Close](jarvis-sdk-13.md#modal-close)

####  [Hashes](https://pip.pypa.io/en/stable/reference/pip_install/#hash-checking-mode) for jarvis\_sdk-0.0.12-py3-none-any.whl

| Hashes for jarvis\_sdk-0.0.12-py3-none-any.whl |  |  |
| :--- | :--- | :--- |
| Algorithm | Hash digest |  |
| SHA256 | `6e9aa4bc95560da3dc525cb86e59f8ed6471ac65c0fd73bdc0b786de77eaff9b` |  |
| MD5 | `b48817d56bfb4d2d41d7bc32377de9e6` |  |
| BLAKE2-256 | `b924f7cb5a4d466cfde440460b8b19d4cab54154a2cc1fea1e259eacb955805e` |  |

