---
description: Learn how to set constants with the Context data operation.
---

# Set constants with Context

## :bulb:What is a Context?

Before getting started with the creation of your data pipeline operations, you can first set constants using a Context configuration.

Constants are objects which act as placeholders. Their value is stored through a Context configuration, and can be called in all your subsequent data operation configurations. When you deploy a data operation, you are asked the Context you want to use. The constants are then replaced by their value according to the Context.

Constants can be used for example to replace GCP credential parameters so they can remain confidential, to manage different GCS bucket names for development and production environnement, etc.

## ✅ Supported data operations

All data operations can contain the constants set with Context.

{% hint style="warning" %}
For STS, STT, TTT and TTS data operations, you will need to add`"version" : "2"`in the JSON configuration file to be able to use constants.
{% endhint %}

## ⚙️ How it works

You set a constant as follows through a Context data operation:

```
"test_integer": {
			"value": 123456,
			"type": "integer",
			"resource": "value",
			"description": "Some random value"
		}
```

Once you have deployed your Context configuration, you can start using the constants it contains in your other data operations using the name of the constant wrapped in double curly brackets. For example, to set the max\_active\_run to your "test\_integer":

```
"max_active_runs": {{test_integer}}
```

{% hint style="danger" %}
**make sure there is no space between your constant and the brackets**

```
{{test_integer}} -> OK
{{ test_integer }} -> KO
```
{% endhint %}

When you deploy a configuration file, you will be asked the Context you want to use. The constants are then replaced by the values defined in the Context configuration.

You can use the constants in the SQL files of your table-to-table, for example:

`SELECT * FROM {{bq_dataset}}.my_table`

There are two generic constants you can use in a your data operation configurations without setting them explicitly in your Context configuration:

* {{FD\_ACCOUNT}} will take the value of the "account" parameter of your Context configuration
* {{FD\_ENV}} will take the value of the "environment" parameter of your Context configuration

When you deploy a configuration using a Context, the ID of the configuration deployed in Tailer is the concatenation of the Context's `account_id`, the Context's `configuration_id`, your data operation's `configuration_id` and your data operation's `environment`. With this concatenation, you don't risk to eraze configurations for different contexts.

## **📋 How to set and use a constant**

1. Access your **tailer** folder (created during [installation](../../getting-started/install-tailer-sdk.md)).
2. Create a working folder as you want, and create a JSON file for your Context data operation inside.
3. Set all the required constants in your JSON configuration file. Refer to this page to learn about all the [parameters](context-configuration-file.md).
4.  Access your working folder by running the following command:

    ```
    cd "[path to your working folder]"
    ```
5.  To deploy the Context data operation, run the following command:

    ```
    tailer deploy configuration your-file.json
    ```
6. Log in to [Tailer Studio](http://studio.tailer.ai) to check the status and details of your data operation.
7. You can now use your constants in other data operation configurations files. Dont forget to add `"version" : "2"` for STS, STT, TTT and TTS data operations. If your constant is a string, do not forget to include them wrapped in double quotes, for example: `"gcp_project_id": "{{gcp_dlk_project_id}}"`
8. You can now deploy your configuration file. You will be asked the Context you want to use. If you want to skip this question, you can use the --context flag when you deploy, for example: `tailer deploy configuration your-data-operation.json --context your-context-name`
