---
description: >-
  Learn how to modify your raw data files across buckets using the File
  Utilities operation.
---

# Modify files with File Utilities

## :bulb: What is File Utilities?

A File Utilities data pipeline operation allows you to modify raw data files from a Google Cloud Storage bucket: encrypt, decrypt, concat or split files.

## ✅ Supported operations

### **PGP encryption**

* Encrypt a file to .gpg
* Decrypt a file from .gpg

More operations will be implemented and released in beta version.

## ⚙️ How it works

Every time a new file matching the specified rule appears in the source directory, it will be:

* removed from the source directory,
* transferred to another directory located in the same storage, inside a folder named with the operation on the file in success,
* if options have been set accordingly, copied the modified file to an archive directory located in the same storage, inside a folder named as the filename date.

## **📋 How to deploy a File Utilities data operation**

1. Access your **tailer** folder (created during [installation](../../getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want, and create a JSON file for your data operation inside.
3. Prepare your JSON configuration file. Refer to this page to learn about all the [parameters](../move-files-with-storage-to-storage/storage-to-storage-configuration-file.md).
4.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
5.  To deploy the data operation, run the following command:

    ```
    tailer deploy configuration your-file.json
    ```
6. Log in to [Tailer Studio](http://studio.tailer.ai) to check the status and details of your data operation.
7. Add a file with the proper name template in the source folder.
8. Access your output folder(s), and archive folder, if any, to check the result of the data operation.
