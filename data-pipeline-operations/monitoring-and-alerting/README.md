---
description: Learn how to monitoring and alerting data operation runs
---

# Monitoring and Alerting

## ✅ Compatible data operations

All data operation runs can be monitoring and send an alert when it failed

## ⚙️ How it works

You can enriched the configuration of any data operation with monitoring and alerting information in order to define criteria about the data operation criticality when it failed. Those information also determine if Tailer should send an alert, when to who and with which specific content. 

## **📋 How to monitor data operation runs and send an alert**

The monitoring and alerting parameters of a data operation are defined inside the data operation configuration, at the root level under the `monitoring {}` json object. that's mean you have to redeploy a data operation configuration to activate monitoring and alerting triggers.

You can use specific variables in your alerting messages in order to generate, for instance,  an email following your monitoring requirements. But, you can also use the default tailer alerting templates so you don't have to build your own message.

To learn about the Monitoring and Alerting parameters, refer to [this page](../orchestrate-processings-with-workflow/workflow-configuration-file.md).

