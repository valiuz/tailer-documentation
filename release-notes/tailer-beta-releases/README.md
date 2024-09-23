---
description: >-
  DANGEROUS ZONE : On this page, you will find the Tailer Components' beta
  releases
---

# Tailer Beta Releases

## Tailer API Endpoint Staging (Europe)

https://fd-io-jarvis-platform-api-proxy-staging-a7nkzexitq-ew.a.run.app/

## Tailer SDK Beta releases

{% hint style="danger" %}
Beta versions of Tailer SDK are available here for testing purposes.

INSTALL ONLY IF YOU KNOW WHAT YOU ARE DOING !
{% endhint %}

### Latest BETA package : 1.3.19

#### Beta 1.3.19 Release Note

* auth : fixed a bug during login where default value would not be used properly
* TTT : added usage of FD\_DATE templates on table name for sql tasks and delete table tasks

Last updated : 2024-09-23 09:22



{% file src="../../.gitbook/assets/package (24).zip" %}

### Install Tailer SDK Package using PIP

1. Download the package above on your computer
2.  Unzip the zip file. You should get a file named : **tailer\_sdk-X.Y.Z-py3-none-any.whl**

    X.Y.Z is the SDK version number.
3.  Install using PIP :

    `pip3 install tailer_sdk-X.Y.Z-py3-none-any.whl --force-reinstall`

### Go back to the latest RELEASE version

From a terminal, use PIP to go back to the latest Tailer SDK RELEASE:

`pip3 install tailer-sdk --force-reinstall`
