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

### Latest BETA package : 1.3.17

#### Beta 1.3.17 Release Note

* TTT : fixed an issue in the dependencies parser.
* TTT : added range partitioning for table creation
* TTT : bq\_table\_timepartitioning\_require\_partition\_filter flag is now DEPRECATED, please use bq\_table\_require\_partition\_filter instead.
* Misc : enhanced encoding detection when reading input files.
* TTT : added DDL information in the saved configuration

Last updated : 2024-02-19 07:55



{% file src="../../.gitbook/assets/package.zip" %}

### Install Tailer SDK Package using PIP

1. Download the package above on your computer
2.  Unzip the zip file. You should get a file named : **tailer\_sdk-X.Y.Z-py3-none-any.whl**

    X.Y.Z is the SDK version number.
3.  Install using PIP :

    `pip3 install tailer_sdk-X.Y.Z-py3-none-any.whl --force-reinstall`

### Go back to the latest RELEASE version

From a terminal, use PIP to go back to the latest Tailer SDK RELEASE:

`pip3 install tailer-sdk --force-reinstall`
