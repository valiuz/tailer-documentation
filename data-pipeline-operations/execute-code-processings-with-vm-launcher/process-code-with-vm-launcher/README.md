---
description: Learn how to use a VM Launcher data operation to process some code on a VM.
---

# Process code with VM Launcher

## 💡 What is the VM Launcher data operation for code processing?

The VM Launcher data operation allows you to start a Google Compute Engine VM, where you can execute a script in the language of your choice, and then to stop the VM automatically to save resources.

## ✅ Supported languages

All languages \(Python, R, JavaScript, etc.\)

## ⚙️ How it works

Every time a script file appears in a given directory of a Google Cloud Storage bucket:

* A VM with the specified characteristics is started on GCE.
* The instructions set in the JSON configuration file are executed, and the script is launched.
* Once the execution is complete, the VM is stopped automatically.

## **📋 How to deploy a** VM Launcher **data operation for code processing**

1. Access your **tailer** folder \(created during [installation](../../getting-started/install-tailer-sdk.md)\).
2. Create a working folder as you want, and create a JSON file for your data operation inside.
3. Prepare your JSON configuration file. Refer to this page to learn about all the [parameters](../../xml-conversion/untitled-1.md).
4. Access your working folder by running the following command:

   ```text
   cd "[path to your working folder]"
   ```

5. To deploy the data operation, run the following command:

   ```text
   tailer deploy configuration your-configuration.json
   ```

6. Log in to [Tailer Studio](http://studio.tailer.ai/) to check the status and details of your data operation.
7. For your VM Launcher data operation to be executed, you need to place a script in the working folder.

