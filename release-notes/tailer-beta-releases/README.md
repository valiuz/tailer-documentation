---
description: >-
  DANGEROUS ZONE : On this page, you will find the Tailer Components' beta
  releases
---

# Tailer Beta Releases

## Tailer SDK Beta releases

{% hint style="danger" %}
Beta versions of Tailer SDK are available here for testing purposes.

INSTALL ONLY IF YOU KNOW WHAT YOU ARE DOING !
{% endhint %}

### Latest BETA package : 1.3.5

#### Beta 1.3.5 Release Note



* TTS : added support for new version (v3) of Table To Storage configuration.
* TTT : added SQL query details in run infos for "expectation" task type.
* TTT : fixed an issue with how expectations failures are handled.
* Context : added new template for context : {{FD\_CONTEXT}}. This template will insert the Context ID.
* Context : context will always be applied, disregarding the version number.

{% file src="../../.gitbook/assets/package (12).zip" %}

### Install Tailer SDK Package using PIP

1. Download the package above on your computer
2.  Unzip the zip file. You should get a file named : **tailer\_sdk-X.Y.Z-py3-none-any.whl**

    X.Y.Z is the SDK version number.
3.  Install using PIP :

    `pip3 install tailer_sdk-X.Y.Z-py3-none-any.whl --force-reinstall`

### Go back to the latest RELEASE version

From a terminal, use PIP to go back to the latest Tailer SDK RELEASE:

`pip3 install tailer-sdk --force-reinstall`

###
