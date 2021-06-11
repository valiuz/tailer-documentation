---
description: Learn how to use a VM Launcher data operation to encrypt and decrypt data.
---

# Encrypt/Decrypt data with VM Launcher

## 💡 What is the VM Launcher data operation for data encryption/decryption?

The VM Launcher data operation allows you to start a Google Compute Engine VM where you can encrypt and decrypt data using PGP \(Pretty Good Privacy\), and then to stop the VM automatically to save resources.

## ⚙ How it works

Every time a script file appears in a given directory of a Google Cloud Storage bucket:

* A VM with the specified characteristics is started on GCE.
* The files placed in the bucket are either encrypted or decrypted using PGP.
* Once the execution is complete, the VM is stopped automatically.

## \*\*\*\*📋 **How to deploy a** VM Launcher **data operation for data encryption/decryption**

1. Access your **tailer** folder \(created during [installation](../../getting-started/install-tailer-sdk.md)\).
2. Create a working folder as you want.
3. Create a JSON file for your data operation in your working folder. Refer to this page to learn about all the [parameters](../../xml-conversion/untitled-1.md).
4. Access your working folder by running the following command:

   ```text
   cd "[path to your working folder]"
   ```

5. To deploy the data operation, run the following command:

   ```text
   tailer deploy configuration your-configuration.json
   ```

6. Log in to [Tailer Studio](http://studio.tailer.ai/) to check the status and details of your data operation.
7. For execute your VM Launcher data operation, place a file in the working folder.

