---

copyright:
  years: 2023, 2024
lastupdated: "2024-03-29"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I create a Delegated DNS resolver for my DNS-shared VPC?
{: #troubleshoot-hub-2}
{: troubleshoot}
{: support}

When you configure DNS sharing for Virtual Private Endpoint (VPE) gateways, you might encounter issues. Often, you can recover by following a few steps.
{: shortdesc}

The request fails when updating the DNS resolver type to Delegated on the DNS-shared VPC.
{: tsSymptoms}

Possible causes include:
{: tsCauses}

* The DNS hub VPC must be disabled before you change the DNS resolver type.
* The DNS hub VPC is not configured with a custom resolver.

To resolve this issue:
{: tsResolve}

1. Make sure that the DNS hub is enabled. For more information, see [Enabling a VPC as a DNS hub](/docs/vpc?topic=vpc-vpe-dns-sharing-configure-hub).
1. Enable a custom resolver in the DNS hub VPC. As indicated in the [Getting started](/docs/vpc?topic=vpc-vpe-dns-sharing&interface=ui#vpe-dns-sharing-process) process, you must [configure a DNS custom resolver](/docs/dns-svcs?topic=dns-svcs-ui-create-cr) on the hub VPC to be responsible for resolving DNS queries from hub and DNS-shared VPCs, as well as those from on-prem networks.
