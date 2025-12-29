---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-29"

keywords: flow logs, FAQs

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for flow log collectors
{: #fl-faq}

You might encounter the following questions when you use {{site.data.keyword.cloud_notm}} Flow Logs for VPC.

## Why don't I see any {{site.data.keyword.cos_full_notm}} buckets as options when I create a flow log collector?
{: #faq-cos-buckets-as-options}
{: faq}
{: support}

The most likely reasons why you might not see your {{site.data.keyword.cos_short}} buckets when you order a flow log collector:

* An {{site.data.keyword.cos_full}} service instance isn't provisioned or a destination bucket to collect flow logs.
* An IAM authorization that grants the flow logs service permission to write flow logs to their bucket isn't configured.

   In this case, you receive prompts to create an {{site.data.keyword.cos_short}} service instance and bucket, and add the required Identity and Access Management (IAM) authorization when you are creating a flow log collector.
   See [Creating flow log collectors](/docs/vpc?topic=vpc-ordering-flow-log-collector) for information on how to remedy this issue.

## Why am I getting a 403 error when I provision a flow log collector?
{: #faq-403-flow-logs}
{: faq}
{: support}

Likely causes of this error include:

* Your user is not authorized to access the specified target of the flow log collector.
* Your {{site.data.keyword.cos_full}} bucket is missing the Identity Authorization Management (IAM) authorization to allow the flow logs service to write flow logs to your bucket.

## Can I create multiple flow log collectors?
{: #faq-multiple-flow-log-collectors}
{: faq}
{: support}

You can create multiple flow log collectors on the condition that they are on different targets. Keep in mind that flow log collectors with different target scopes might overlap. You cannot create multiple flow log collectors on one single target.

## Can I modify the {{site.data.keyword.cos_full_notm}} location for a flow log collector?
{: #faq-modify-cos-location}
{: faq}
{: support}

You cannot change the {{site.data.keyword.cos_short}} bucket location for an existing flow log collector. You can delete the existing collector and create a new one with the {{site.data.keyword.cos_short}} bucket location that you want to use.

## Are virtual appliances (IKS workers, ROKS, LBaaS, VPN gateway) included in the flow log collector data output?
{: #virtual-appliances-in-collector-output}
{: faq}
{: support}

Flow Logs for VPC supports:

* IBM Cloud Kubernetes Service (IKS) workers
* RedHat OpenShift Kubernetes Service (ROKS)
* Load Balancer as a Service (LBaaS)

VPN gateway is also available at a VPC and subnet level.

At this time, flow log collection does not include support for bare metal server network interfaces and endpoint gateways.
{: note}

## Is there a viewer or filter for flow logs?
{: #viewer-filter}
{: faq}
{: support}

Flow Logs for VPC does not have a native viewer or filter.

## Can I modify the target scope for a flow log collector?
{: #faq-modify-target-scope-flow-log-collector}
{: faq}
{: support}

You cannot change the target scope for an existing flow log collector. You can delete the existing collector and create a new one with the target scope that you want to use.

## Do I need to suspend a flow log collector before I delete it?
{: #faq-suspend-before-you-delete-flow-log-collector}
{: faq}
{: support}

No, you can delete a flow log collector at any time, whether it is active or not.
