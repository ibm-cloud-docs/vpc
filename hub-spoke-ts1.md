---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-18"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't the DNS hub VPC resolve records for an endpoint gateway in a DNS-shared VPC?
{: #troubleshoot-hub-1}
{: troubleshoot}
{: support}

When you configure DNS sharing for Virtual Private Endpoint (VPE) gateways, you might encounter issues. Often, you can recover by following a few steps.
{: shortdesc}

All DNS-shared VPCs must have their DNS resolver pointed to the custom resolver on the hub VPC. Any misconfiguration or change to the transit gateway that's connecting the hub and shared VPCs might cause DNS failures. Also, any updates to the custom resolver on the hub VPC can potentially impact DNS resolution for the whole network.

When the VPE DNS resolution binding switch (`allow_dns_resolution_binding` flag) is disabled and enabled in less than 5 minutes, the VPEs in a DNS-shared VPC can't be resolved from the hub VPC. This problem also occurs when a DNS resolution binding between a DNS-shared VPC and the hub VPC is deleted and re-created. The VPEs in DNS shared VPC can't be resolved from the hub VPC if these operations are performed less than 5 minutes apart.
{: tsSymptoms}

Possible causes include:
{: tsCauses}

This issue is a known limitation. If the process takes less than 5 minutes, DNS resolution can fail.

Follow these steps to troubleshoot DNS failures:
{: tsResolve}

Make sure that the binding process takes longer than 6 minutes when `allow_dns_resolution_binding` for endpoint gateways is disabled and enabled, and when a DNS resolution binding is deleted and re-created.

You must perform the following configuration steps in the following order, or a DNS resolution outage might occur.
{: important}

When a new DNS-shared VPC wants to connect to a DNS hub, follow these steps:

1. Make sure that a connection exists between the DNS hub and DNS-shared VPCs.
1. The shared VPC owner creates a DNS resolution binding to connect to the DNS hub VPC. For more information, see [Creating a DNS resolution binding](/docs/vpc?topic=vpc-vpe-dns-sharing-resolution-bindings).
1. The DNS-shared VPC owner configures the shared VPC's DNS resolver to Delegated type to connect to the custom resolver on the hub VPC. Only then can the lifecycle state of the DNS resolution binding change to `Stable`. For more information, see [Setting the DNS resolver type](/docs/vpc?topic=vpc-configure-dns-resolver).

When a DNS-shared VPC wants to disconnect from a hub VPC, follow these steps:

1. The DNS-shared VPC owner configures the DNS-shared VPC's resolver type to System. For more information, see [Setting the DNS resolver type](/docs/vpc?topic=vpc-configure-dns-resolver).
1. The DNS-shared VPC owner deletes the DNS resolution binding. For more information, see [Disconnecting DNS sharing to a hub VPC](/docs/vpc?topic=vpc-remove-sharing-hub&interface=ui).
