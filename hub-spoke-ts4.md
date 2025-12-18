---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-18"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I create or delete a cross-account DNS resolution binding or delegated resolver type in a DNS-shared VPC?
{: #troubleshoot-hub-4}
{: troubleshoot}
{: support}

When you configure DNS sharing for Virtual Private Endpoint (VPE) gateways, you might encounter issues. Often, you can recover by following a few steps.
{: shortdesc}

When a DNS resolution binding between a DNS-shared VPC and a cross-account DNS hub VPC is created or deleted, you receive a `401 unauthorized` error. You receive the same error when the DNS resolver type of a DNS-shared VPC is updated to 'delegated'.
{: tsSymptoms}

If the hub and DNS-shared VPCs are in different accounts, IAM uses a delegation token to check whether the service-to-service (s2s) authorization grants Read access on the hub VPC to the DNS-shared VPC. If IAM denies this action, a `401 unauthorized` error message is returned.
{: tsCauses}

If the DNS hub and DNS-shared VPCs are on different accounts, you must have an s2s authorization policy that grants Read access on the hub VPC to the DNS-shared VPC. For more information, see [Establishing service-to-service authorization](/docs/vpc?topic=vpc-vpe-dns-sharing-s2s-auth).
{: tsResolve}
