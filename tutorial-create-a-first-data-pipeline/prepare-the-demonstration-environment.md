---
description: >-
  Before starting with the actual tutorial, you need to prepare the buckets and
  folders where the magic will happen, and to import the demo files.
---

# Prepare the demonstration environment

## 🗂 Create buckets and folders

For the detailed procedure on how to create GCS buckets \(manually or using gsutil\), refer to this [page](https://cloud.google.com/storage/docs/creating-buckets).

1. Create a bucket in a first project. This bucket will contain the source files. As bucket names need to be unique globally, you can pick any name you want. Select the settings that you want.‌
2. In this first bucket, create 2 folders: **◾ tailer-demo-input-folder** ◾ **tailer-demo-archive-folder**
3. Go to a different project, and create a second bucket. This bucket will contain the output files. Again, pick the name and settings that you want.
4. In this second bucket, create 2 folders: **◾ tailer-demo-input-folder** **◾ tailer-demo-archive-folder**

## ⬆ Import demo files

Import the following demo files into the folder named **tailer-demo-input-folder** located in the first bucket:

{% file src="../.gitbook/assets/stores-20130701-062333.csv" caption="Demo file 1" %}

{% file src="../.gitbook/assets/sales\_20200228-20200228.csv" caption="Demo file 2" %}

{% file src="../.gitbook/assets/sales\_20200110-20200110.csv" caption="Demo file 3" %}

{% file src="../.gitbook/assets/sales\_20200108-20200108.csv" caption="Demo file 4" %}

{% file src="../.gitbook/assets/products-20180811-091503.csv" caption="Demo file 5" %}

