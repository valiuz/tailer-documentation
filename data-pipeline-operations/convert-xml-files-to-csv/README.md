---
description: >-
  Learn how to convert XML located in a Google Cloud Storage bukcet into CSV files
  using a Convert-XML operation.

---

# Convert XML to CSV

## 💡 What is Convert-xml-to-csv Operation?

Extensible Markup Language (XML) is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable (Wikipedia). 

The design goals of XML emphasize simplicity, generality, and usability across the Internet. It is a textual data format with strong support via Unicode for different human languages. Although the design of XML focuses on documents, the language is widely used for the representation of arbitrary data structures such as those used in web services.

In addition to being well-formed, an XML document may be valid. This means that it contains a reference to a document (usually XSD which stands for XML Schema Definition) that defines its elements and attributes and also describes the grammatical rules that apply to elements and attributes. 

A conversion to CSV files requires a XML and the associated XSD file.


## ✅ Supported file types

### **Source files**

* XML file in Google Cloud Storage
* XSD file in Google Cloud Storage

### **Export files**

* CSV file in Google Cloud Storage
* DDL file in Google Cloud Storage

## ⚙️ How it works

After deploying a configuration for a specific xml file pattern, when a file is uploaded
in a dedicated bucket: 
* The xml file will be checked for a matching xsd file
* A conversion process will start with the launch of a dedicated Virtual Machine
* A set of CSV files and DDL files (that describes the schema of all CSV) will be generated into the destination bucket
* A filtering might occurs at the end of the process to remove unwanted files.


## **📋 How to deploy a Convert-xml-to-csv data operation**

1. Access your **tailer** folder \(created during [installation](../../getting-started/install-tailer-sdk.md)\).
2. Create a working folder as you want, and create a JSON file for your data operation inside.
3. Place the XSD file at the same location than your JSON configuration file.
4. Prepare your JSON configuration file. Refer to this page to learn about all the [parameters](tconvert-xml-configuration-file.md).
5. Access your working folder by running the following command:

   ```text
   cd "[path to your working folder]"
   ```

6. To deploy the data operation, run the following command:

   ```text
   tailer deploy configuration your-configuration.json
   ```

7. Log in to [Tailer Studio](https://jarvis-platform.io/sign-in?redirect=%2F&__hstc=57968821.199e85015347f5cf00c120e5932c4c81.1601276395705.1601454251429.1601460096069.15&__hssc=57968821.4.1601460096069&__hsfp=649433320) to check the status and details of your data operation.
8. For your Convert-xml-to-csv to be executed, you either need to copy a file into the source folder.
10. Access the GCS bucket to check your output file \(CSV and DDL files\).
