---
description: Learn how to use a VM Launcher data operation to encrypt and decrypt data.
---

# Encrypt/Decrypt data with VM Launcher

## :bulb: What is the VM Launcher data operation for data encryption/decryption?

The VM Launcher data operation allows you to start a Google Compute Engine VM where you can encrypt and decrypt data using Pretty Good Privacy (PGP), and then to stop the VM automatically to save resources. PGP is a popular solution providing cryptographic privacy and authentication for data communication.

## :gear: How **it** works

Every time the VM Launcher data operation is launched:

* A VM with the specified characteristics is started on GCE.
* The files placed in the bucket are either encrypted or decrypted using PGP.
* Once the execution is complete, the VM is stopped automatically.

## Pretty Good Privacy (PGP)

PGP is a protocol used for encrypting, decrypting and signing messages or files using a key pair. Every PGP user has both a public and private key. A public key is the key that other people use to encrypt a message that only you can open. A private key is the key that allows you to decrypt the messages sent to you based on your public key. A public key can be shared, but a private key should never be shared.

![](../../../.gitbook/assets/private\_public\_key\_perso2.PNG)

## ****:clipboard: **How to deploy a** VM Launcher **data operation for data encryption/decryption**

1. Access your **tailer** folder (created during [installation](https://app.gitbook.com/s/-MIIsP\_DvP2J-c1szWrQ/getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want.
3. Create a JSON file for your data operation in your working folder. Refer to this page to learn about all the [parameters](https://app.gitbook.com/s/-MIIsP\_DvP2J-c1szWrQ/xml-conversion/untitled-1.md).
4.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
5.  To deploy the data operation, run the following command:

    ```
    tailer deploy configuration your-configuration.json
    ```
6. Log in to [Tailer Studio](http://studio.tailer.ai) to check the status and details of your data operation.
7. Execute your VM Launcher data operation.
