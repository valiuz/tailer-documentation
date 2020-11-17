---
description: "This page contains instructions for installing and configuring Tailer\_SDK, and logging in to Tailer\_Platform."
---

# Install Tailer SDK

## 📦 Install the Tailer SDK package

To install the Tailer SDK package:

1. Run the following command in Command Prompt \(Windows\) or in a terminal \(macOS/Linux\):

   ```text
   pip3 install tailer-sdk
   ```

2. When the installation is done, make sure it was successful by checking Tailer SDK version:

   ```text
   tailer version
   ```

{% hint style="success" %}
The Tailer SDK version and help should display.
{% endhint %}

{% hint style="info" %}
At any time, you can now display the Tailer SDK help with the following command:

```text
tailer help
```
{% endhint %}

## ⚙ Configure Tailer SDK

When the installation is complete, you can proceed with the configuration of Tailer SDK:

1. To launch the configuration, run the following command:

   ```text
   tailer config
   ```

2. You'll be prompted to answer a series of questions. Use the values listed in the table below. Directly press **Enter** if the suggested value is right.

   | Question | Value |
   | :--- | :--- |
   | Email address | Provide the email address you want to use to connect to Tailer SDK. |
   | Tailer Firebase API key | **AIzaSyA\_FFZmy9jZaRGVTNQuOK0mv4wDxWOKScQ** |
   | Tailer Firebase authentication domain | **fd-io-jplatform.firebaseapp.com** |
   | Tailer API endpoint | **https://fd-io-jarvis-platform-api-proxy-a7nkzexitq-uc.a.run.app/** |
   | SSL verification | Specify if you want to enable SSL verification \(**y**\) or not \(**n**\). Default is **y**. |
   | Custom client SSL verification | Specify if you want to use custom client SSL verification \(**y**\) or not \(**n**\). Default is **n**. |
   | Full path to the SSL certificate | This question only displays if you answered **y** at the previous question. Provide your custom SSL certificate in PEM format \(e.g. "C:\My Certificates\cert\_XYZ.pem"\). |

3. Once you've answered all the questions, close and restart your terminal for changes to be implemented.

## 👤 Log in to Tailer Platform

Once the installation and the configuration are done, you need to log in to Tailer Platform to be able to access the Tailer API.

1. To log in, run the following command:

   ```text
   tailer auth login
   ```

2. Provide the same email address as before as suggested, and the password of your Tailer Platform account. If you don't have an account yet, just press **Enter**. If you already have a Tailer Platform account, your login is successful and you can stop here. If not, follow the steps below to sign in.
3. Confirm that you want to create a Tailer Platform account by entering **y**.
4. Provide again the same email address and the password of your choice.
5. Confirm your email address by clicking on the link sent to your email address.

{% hint style="success" %}
The account is successfully created and you're now logged in to Tailer Platform! 🎉 

You can now proceed with the [configuration of Google Cloud Platform](set-up-google-cloud-platform.md).
{% endhint %}

