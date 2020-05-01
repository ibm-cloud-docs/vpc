---

copyright:
  years: 2019, 2020
lastupdated: "2020-04-17"

keywords: flow logs, faqs, cos bucket

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
 
# FAQs for flow log collectors (Beta)
{: #fl-faq} 

You might encounter the following frequently asked questions when using IBM Cloud Flow Logs.

## Why don't I see any Cloud Object Storage (COS) buckets as options when creating a flow log collector?
{: #faq-cos-buckets-as-options}
{: faq}
{: support}

The most likely reasons why you might not see your COS buckets when ordering a flow log collector:

   * You haven't provisioned a Cloud Object Storage (COS) service instance and a destination bucket for the flow logs to be collected in.
   * You haven't configured an IAM authorization granting the flow logs service permission to write flow logs to their bucket.

      In this case, you receive prompts to create a COS service instance and bucket, and add the required Identity and Access Management (IAM) authorization when you are creating a flow log collector.
      
      See [Creating flow log collectors](/docs/vpc?topic=vpc-ordering-flow-log-collector) for information on how to remedy this issue.
            
## Why am I getting a 403 error when provisioning a flow log collector?
{: #faq-403-flow-logs}
{: faq}
{: support}

Likely causes of this error include:

   * Your user is not authorized to create flow log collectors.
   * Your user is not authorized to access the specified target of the flow log collector.
   * Your Cloud Object Storage (COS) bucket is missing the Identity Authorization Management (IAM) authorization to allow te flow logs service to write flow logs to your bucket.
   
## Can I create multiple flow log collectors?
{: #faq-multiple-flow-log-collectors}
{: faq}
{: support}

You can create multiple flow log collectors as long as they are on different targets. Keep in mind that flow log collectors with different target scopes may have overlaps. You cannot create multiple flow log collectors on one single target.

## Can I modify the Cloud Object Storage location for a flow log collector?
{: #faq-modify-cos-location}
{: faq}
{: support}

You cannot change the COS bucket location for an existing flow log collector. You can delete the existing collector and create a new one with the desired COS bucket location.

## Can I modify the target scope for a flow log collector?
{: #faq-modify-target-scope-flow-log-collector}
{: faq}
{: support}

You cannot change the target scope for an existing flow log collector. You can delete the existing collector and create a new one with the desired target scope.

## Do I need to suspend the flow log collector before deleting it?
{: #faq-suspend-before-you-delete-flow-log-collector}
{: faq}
{: support}

No, you can delete a flow log collector whether it is active or not.
