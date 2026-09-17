---

copyright:
  years: 2026
lastupdated: "2026-09-17"

keywords: troubleshooting floating ip, unexpected charges, floating ip billing, cancelled virtual server instance

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I still being charged for a floating IP after I canceled my virtual server instance?
{: #fip-ts-unexpected-charges}
{: troubleshoot}
{: support}

You canceled or reclaimed a virtual server instance, but charges for a floating IP address continue to appear on your bill.
{: shortdesc}

After canceling a virtual server instance, your account still shows charges for a floating IP address.
{: tsSymptoms}

Canceling or reclaiming a virtual server instance does not automatically release any floating IP that was attached to it. The floating IP remains allocated to your account as an independent billable resource until you manually release it.
{: tsCauses}

Release any floating IPs that are no longer needed. 

1. In the {{site.data.keyword.cloud_notm}} console, go to **Navigation menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Network > Floating IPs**.
1. Locate the floating IP, and click **Release**. 

You can also release a floating IP by using the CLI (`ibmcloud is floating-ip-delete <floating-ip-id>`) or the API.
{: tsResolve}
