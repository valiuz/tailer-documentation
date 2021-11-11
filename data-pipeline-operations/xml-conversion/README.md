---
description: >-
  Learn how to convert XML files into CSV files using a Convert XML to CSV data
  operation.
---

# Convert XML to CSV

## 💡 What is the Convert XML to CSV operation?

The Convert XML to CSV data operation allows you to retrieve all the information contained in a possibly complex XML file into a set of CSV files, all located in a Google Cloud Storage bucket. You can later convert your CSV files into database tables using a [Storage to Tables](../load-data-with-storage-to-tables/) data operation.

{% hint style="warning" %}
Note that the XML file provided must be well-formed and valid against a matching XSD file (defining its elements and attributes, and the rules that apply to them).

The XML and XSD file must have the same name (suffix excluded). Refer to [this page](untitled-1.md) for more details.

If the XML file contains entities not set in the XSD, then the corresponding data will not be exported to CSV files.
{% endhint %}

## ✅ Supported file types

### **Source files**

* XML + XSD file pair(s)

### **Export files**

* Multiple TSV (_Tab_ Separated Values) + DDL file pairs

## ⚙️ How it works

Every time a new file matching the specified XML file name pattern appears in a given directory of a Google Cloud Storage bucket:

* The XML file is checked against the matching XSD file.
* If the XML file is valid, the conversion process is launched.
* A set of CSV files with their matching DDL files (describing their schema) is generated in the working directory.
* The source XML files are deleted from the working directory.
* If set, a filtering occurs at the end of the process to remove unwanted CSV files.

## **📋 How to deploy a **Convert XML to CSV** data operation**

1. Access your **tailer** folder (created during [installation](https://app.gitbook.com/s/getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want, and create a JSON file for your data operation inside.
3. Place the XSD file in the same location as your JSON file.
4. Prepare your JSON configuration file. Refer to this page to learn about all the [parameters](untitled-1.md).
5.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
6.  To deploy the data operation, run the following command:

    ```
    tailer deploy configuration your-configuration.json
    ```
7. Log in to [Tailer Studio](http://studio.tailer.ai) to check the status and details of your data operation.
8. For your Convert XML to CSV data operation to be executed, you need to place a file into the source folder.
9. Access the GCS bucket to check your output files (CSV and DDL files).
