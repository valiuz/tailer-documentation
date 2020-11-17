---
description: "When transferring files from one storage location to another using Tailer\_Platform, you need to provide credentials. A number of steps are required to encrypt them and use them safely."
---

# Encrypt your credentials

## 🗺 Overview

The credentials that you have at your disposal \(GCP JSON private key file, Amazon S3 private key value, or SFTP password\) are unencrypted. To use them safely with Tailer Platform, you first need to encrypt them using Tailer public key. This key consists of a **tailer\_public\_key.pem** file that you need to **request from your Tailer Platform administrator** before proceeding with this procedure.

## 🔑 Install the Tailer public key on your local computer



Once you have the **tailer\_public\_key.pem** file, you need to copy it to the Tailer Home directory of your local computer:

1. First, determine where the Jarvis Home is:  
   ◾ On macOS/Linux, run the following command in a terminal:

   ```text
   echo $TAILER_HOME
   ```

    ◾ On Windows, run the following command in Command Prompt:

   ```text
   echo %TAILER_HOME%
   ```

2. Copy the **tailer\_public\_key.pem** file to the Tailer Home directory you got as a result of the previous command.

## 🛡 Encrypt the credentials

Once the Tailer public key has been installed, you can encrypt your credentials.

To do so, run one of the following commands:

* For a GCP JSON private key file:

  ```text
  tailer encrypt your-credentials.json
  ```

* For an Amazon S3 key value \(do not forget the quotes\):

  ```text
  tailer encrypt "contents of the private key"
  ```

* For an SFTP password \(do not forget the quotes\):

  ```text
  tailer encrypt "password value"
  ```

{% hint style="success" %}
You obtain a JSON output with the following format:

```text
{
  "cipher_aes": "xxx",
  "tag": "xxx",
  "ciphertext": "xxx",
  "enc_session_key": "xxx"
}
```

where **xxx** is a base64-encoded string representation of the key value.

You can now use it in your JSON configuration files for data operations that require authentication.
{% endhint %}

