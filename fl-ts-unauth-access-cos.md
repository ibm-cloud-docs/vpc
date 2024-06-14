---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-25"

keywords: is.flow-log-collector.00002E

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why isn't the flow log collector authorized to publish data to the {{site.data.keyword.cos_full_notm}} bucket?
{: #fl-ts-error-unauth-access-cos}
{: troubleshoot}
{: support}

A flow log collector requires an {{site.data.keyword.cos_full_notm}} bucket to be defined and accessible. If you see the error log with message ID `is.flow-log-collector.00002E`, the {{site.data.keyword.cos_full_notm}} bucket is not accessible. The flow log collector cannot publish data to the bucket.
{: shortdesc}

To avoid lost data, create an {{site.data.keyword.cos_full_notm}} bucket within the next 24 hours to correct this problem.
{: important}

The flow log collector is not authorized to publish data to the {{site.data.keyword.cos_full_notm}} bucket:
   `is.flow-log-collector.00002E: Unauthorized access to Cloud Object Storage bucket <BucketName>`
{: tsSymptoms}

The {{site.data.keyword.cos_full_notm}} bucket does not have the correct access to allow the flow log collector to publish data.
{: tsCauses}

Check for a defined authorization between the flow log collector and the {{site.data.keyword.cos_full_notm}} bucket. If not, add one so that the flow log collector can access the bucket.
{: tsResolve}

To define an authorization, follow these steps:

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** &gt; **Access (IAM)**.
1. Select **Authorizations** from the navigation pane.
1. Click **Create**.
1. For **Source service**:
   * Select **VPC Infrastructure Services**. Then, select **Services based on attributes**.
   * Select **Resource type**. Then, select **Flow Logs for VPC**.
   * Select **Source resource instance** and choose an option.
1. For **Target service**:
   * Select **Cloud Object Storage**. Then, select **Services based on attributes**.
   * Select **Service instance** and choose an option.
1. For **Service access**, select the **Writer** role.
1. Click **Authorize**.

For more information, see [Creating a flow log collector](/docs/vpc?topic=vpc-ordering-flow-log-collector).
{: note}
