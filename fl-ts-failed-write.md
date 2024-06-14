---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-25"

keywords: is.flow-log-collector.00001E

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why didn't data write to the flow log file in the past 24 hours?
{: #fl-ts-error-failed-write}
{: troubleshoot}
{: support}

A flow log collector publishes data to an {{site.data.keyword.cos_full_notm}} bucket. If you see an entry in the error log with message ID `is.flow-log-collector.00001E`, the flow log collector failed to publish data due to either a Cloud Object Storage bucket that does not exist, or a bucket that was not accessible. An error message was sent each hour for the past 24 hours to take necessary action. Unfortunately, the data is lost.
{: shortdesc}

Take immediate action to prevent further loss of data.
{: important}

The {{site.data.keyword.cos_short}} bucket is not created, or is not accessible by the flow log collector:
   `is.flow-log-collector.00001E: Failed to write Flow Log file for the past 24 hours. Dropping flow log for Virtual Server <VSICRN>`
{: tsSymptoms}

The {{site.data.keyword.cos_short}} bucket does not exist, or does not have correct permissions.
{: tsCauses}

Before this error message was logged, you received one of the following error messages each hour for 24 hours. Follow the resolution procedure for the error message that you received.
* [is.flow-log-collector.00002E: Unauthorized access to Cloud Object Storage bucket `<BucketName>`](/docs/vpc?topic=vpc-fl-ts-error-unauth-access-cos)
* [is.flow-log-collector.00003E: Cloud Object Storage bucket `<BucketName>` was not found](/docs/vpc?topic=vpc-fl-ts-error-cos-bucket)
{: tsResolve}
