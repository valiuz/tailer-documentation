---
description: "Some features of Tailer\_Platform rely on Google Cloud\_Platform's resources, or allow you to manipulate them. Some steps need to be performed as preparation."
---

# Set up Google Cloud Platform

## 🗄 Create a project

Create a dedicated Google Cloud Platform project to use with Tailer Platform. Billing needs to be enabled.

To create a new project:

1. Go to the [**Manage resources**](https://console.cloud.google.com/cloud-resource-manager) page in the Cloud Console.
2. In the organization drop-down list at the top of the page, select the organization in which you want to create a project.
3. Click **Create Project**.
4. In the **New Project** window that appears, enter a project name and select a billing account as applicable.
5. If you want to add the project to a folder, enter the folder name in the **Location** box.
6. When you're finished entering new project details, click **Create**.

## 🔑 Enable APIs

The following Google Cloud APIs need to be enabled for the project you have created:

* Cloud Functions
* Identity and Access Management \(IAM\)
* Cloud Resource Manager

To enable an API:

1. Open the [**APIs & services**](https://console.cloud.google.com/apis/) page for your project in the Cloud Console.
2. Click the **Enable APIs and Services** button.
3. Click the API you want to enable. If you need help finding the API, use the search field.
4. In the page that displays information about the API, click **Enable**.

## 🔓 Grant roles to the App Engine default account

When you enable the Cloud Functions API, a service account is automatically created for your project. It should be named as follows:

**YOUR-PROJECT-ID@appspot.gserviceaccount.com**

You need to grant roles to this service account so that it has permission to complete specific actions on some resources in your Cloud Platform project.

To grant roles to the service account:

1. Open the [**IAM & Admin**](https://console.cloud.google.com/iam-admin/) page for your project in the Cloud Console.
2. Click the **Edit** ![Screenshot\_2020-05-12 IAM &#x2013; IAM et admin &#x2013; fd-io-jarvis-demo-e&#x2026; &#x2013; Google Cloud Platform](https://support.fashiondata.io/hs-fs/hubfs/Screenshot_2020-05-12%20IAM%20%E2%80%93%20IAM%20et%20admin%20%E2%80%93%20fd-io-jarvis-demo-e%E2%80%A6%20%E2%80%93%20Google%20Cloud%20Platform.png?width=17&name=Screenshot_2020-05-12%20IAM%20%E2%80%93%20IAM%20et%20admin%20%E2%80%93%20fd-io-jarvis-demo-e%E2%80%A6%20%E2%80%93%20Google%20Cloud%20Platform.png) button corresponding to YOUR-PROJECT-ID@appspot.gserviceaccount.com.
3. Add the **Project** &gt; **Editor** and **Service Account** &gt; **Service Account Token Creator** roles.
4. Click **Save** to apply the roles to the service account.

## 👥 Add the App Engine default account to the appropriate groups

The App Engine default account will need to access the following elements of Tailer Platform:

* Composer \(Airflow\): to trigger DAGs
* Firestore: to retrieve data operations
* Source Repositories: to retrieve Cloud Functions source code

The GCP project hosting the Composer and Firestore instances should already have groups with the appropriate permissions. You have to add the App Engine default account to these groups.

## 🆕 Create a generic service account

This generic service account will be used among other things for:

* Deploying configurations on behalf of an authorized user
* Moving files from one bucket to another

{% hint style="info" %}
You will need a dedicated service account for each GCP project you will use Tailer with \(for example if they contain a source or destination bucket used in a transfer operation\). We recommend you only create one service account per project to avoid right administration becoming too complex.
{% endhint %}

To create the service account:

1. Open the [**Service Accounts**](https://console.cloud.google.com/projectselector2/iam-admin/serviceaccounts) page in the Cloud Console.
2. Click **Select a project**, choose your project, and click **Open**.
3. Click **Create Service Account**.
4. You can use **YOUR-PROJECT-ID** as a name for the service account.
5. Click **Save**.

{% hint style="success" %}
You should now have a new service account named as follows: 

**YOUR-PROJECT-ID@YOUR-PROJECT-ID.iam.gserviceaccount.com**
{% endhint %}

## 🔐 Generate JSON credentials

To generate JSON credentials:

1. In the [**Service Accounts**](https://console.cloud.google.com/projectselector2/iam-admin/serviceaccounts) page of the Cloud Console, find the row of the YOUR-PROJECT-ID@YOUR-PROJECT-ID.iam.gserviceaccount.com service account that you've just created.
2. In that row, click the **More** ![Screenshot\_2020-05-14 Comptes de service &#x2013; IAM et admin &#x2013; fd-jarvis-datalake &#x2013; Google Cloud Platform](https://support.fashiondata.io/hs-fs/hubfs/Jarvis%20Documentation/Screenshot_2020-05-14%20Comptes%20de%20service%20%E2%80%93%20IAM%20et%20admin%20%E2%80%93%20fd-jarvis-datalake%20%E2%80%93%20Google%20Cloud%20Platform.png?width=14&name=Screenshot_2020-05-14%20Comptes%20de%20service%20%E2%80%93%20IAM%20et%20admin%20%E2%80%93%20fd-jarvis-datalake%20%E2%80%93%20Google%20Cloud%20Platform.png) button, and then click **Create key**.
3. Select **JSON** as **Key type** and click **Create**.

{% hint style="warning" %}
When you create a key, your new public/private key pair is generated and downloaded to your machine. It serves as the only copy of the private key. You are responsible for storing the private key securely.
{% endhint %}

{% hint style="success" %}
These credentials will need to be encrypted, so you can use them later in a data operation JSON configuration file.
{% endhint %}

