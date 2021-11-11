---
description: >-
  To get started with Tailer, you can follow this tutorial to learn how to
  create a first data pipeline.
---

# Introduction

## :map: Overview

In this example, we want to process and analyze data from a fictional retailer called "Iowa Liquor". These data are organized in CSV files named according to a specific pattern including a timestamp, and are streamed at regular intervals into a Google Cloud Storage bucket. We'll first transfer them to a bucket located in a different Google Cloud project, load the data into a BigQuery table, and then prepare the data in order to analyze them with an AI model.

## &#x20;:man\_student: What you'll learn

* How data flows through a Tailer Platform data pipeline
* Creating JSON configuration files for data operations
* Deploying data operations with Tailer SDK
* Checking information in Tailer Studio

## &#x20;:tools: What you'll need

* Tailer SDK installed on your local machine (see [Prepare your local environment for Tailer](../getting-started/prepare-your-local-environment-for-tailer.md) and [Install Tailer SDK](../getting-started/install-tailer-sdk.md))
* GCP configured for use with Tailer SDK (see [Set up Google Cloud Platform](../getting-started/set-up-google-cloud-platform.md)) and credentials required to transfer files safely (see [Encrypt your credentials](../getting-started/encrypt-your-credentials.md))
* Access to two projects in GCP on which you have the appropriate permissions
* A terminal to run commands
* CSV files to process (provided at next step)
