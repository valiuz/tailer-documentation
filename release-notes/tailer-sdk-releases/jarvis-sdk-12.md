# jarvis-sdk

* [ Project description](jarvis-sdk-12.md#description)
* [ Project details](jarvis-sdk-12.md#data)
* [ Release history](jarvis-sdk-12.md#history)
* [ Download files](jarvis-sdk-12.md#files)

### Project description

## Jarvis SDK by Fashiondata.io

### Changelog

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

| Files for jarvis-sdk, version 0.0.13 |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| Filename, size | File type | Python version | Upload date | Hashes |
|  Filename, size [jarvis\_sdk-0.0.13-py3-none-any.whl](https://files.pythonhosted.org/packages/e8/4f/21192991906fc03fe0d06c82c766b083d369db7cf6acb5429af20777440a/jarvis_sdk-0.0.13-py3-none-any.whl) \(23.9 kB\) |  File type Wheel |  Python version py3 |  Upload date Nov 26, 2019 |  Hashes [View](jarvis-sdk-12.md#copy-hash-modal-20bdb667-319f-4ce2-8909-32db32542830) |

[ Close](jarvis-sdk-12.md#modal-close)

####  [Hashes](https://pip.pypa.io/en/stable/reference/pip_install/#hash-checking-mode) for jarvis\_sdk-0.0.13-py3-none-any.whl

| Hashes for jarvis\_sdk-0.0.13-py3-none-any.whl |  |  |
| :--- | :--- | :--- |
| Algorithm | Hash digest |  |
| SHA256 | `3064943a77e82535f43daac10a2f186fd7172ec745a5549bb21046b4033e4463` |  |
| MD5 | `bf7315fe6d3b657b80cd2c70a7d77f4d` |  |
| BLAKE2-256 | `e84f21192991906fc03fe0d06c82c766b083d369db7cf6acb5429af20777440a` |  |

