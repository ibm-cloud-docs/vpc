---

copyright:
  years: 2020, 2024
lastupdated: "2024-11-06"

keywords: is.flow-log-collector.00003E

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't the {{site.data.keyword.cos_full_notm}} bucket be found?
{: #fl-ts-error-cos-bucket}
{: troubleshoot}
{: support}

A flow log collector requires an {{site.data.keyword.cos_full_notm}} bucket to be created and accessible. If you see the error log with message ID `is.flow-log-collector.00003E`, the bucket does not exist, or is no longer accessible. The flow log collector cannot publish data to the {{site.data.keyword.cos_full_notm}} bucket.
{: shortdesc}

To avoid lost data, create an {{site.data.keyword.cos_full_notm}} bucket within the next 24 hours to correct this problem.
{: important}

The flow log collector lost connection to an {{site.data.keyword.cos_full_notm}} bucket:
   `is.flow-log-collector.00003E: Cloud Object Storage bucket <BucketName> was not found`
{: tsSymptoms}

A bucket that is associated with a flow log collector was deleted, or is not accessible by a flow log collector.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Create an {{site.data.keyword.cos_full_notm}} bucket with the same `<BucketName>` specified in the error message. To create an {{site.data.keyword.cos_full_notm}} bucket, see the [{{site.data.keyword.cos_full_notm}}](/objectstorage/create){: external} ordering page.

   The {{site.data.keyword.cos_full_notm}} bucket must be a single-region bucket in the same region as the target resource. Additionally, it is recommended that you secure the bucket through IAM access groups and audit logging.
   {: note}

1. Check to make sure that you defined an authorization between the flow log collector and the {{site.data.keyword.cos_full_notm}} bucket so that the flow log collector can publish data. To define an authorization, use the following steps:

   * In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**.
   * Select **Authorizations** from the navigation pane.
   * Click **Create**.
   * For **Source service**:
      * Select **VPC Infrastructure Services**. Then, select **Services based on attributes**.
      * Select **Resource type**. Then, select **Flow Logs for VPC**.
      * Select **Source resource instance** and choose an option.
   * For **Target service**:
      * Select **Cloud Object Storage**. Then, select **Services based on attributes**.
      * Select **Service instance** and choose an option.
   * For **Service access**, select the **Writer** role.
   * Click **Authorize**.

For more information, see [Creating a flow log collector](/docs/vpc?topic=vpc-ordering-flow-log-collector).
{: note}
