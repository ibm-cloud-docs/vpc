---

copyright:
  years: 2023
lastupdated: "2023-09-22"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

# Known issues and limitations
{: #hub-spoke-limitations}

* Hub and DNS-shared VPCs must be in the same region. Support for DNS-shared VPCs in a remote region is not supported.
* VPEs on the DNS hub VPC will always be shared with associated DNS-shared VPCs. All VPEs on the DNS hub must be configured with `allow_dns_resolution` enabled before the VPC can be enabled as a DNS hub.
* You are responsible for designing and managing your own architecture among multiple VPCs and accounts.
* Control of on-prem DNS servers pointing to a DNS custom resolver of the hub VPC on IBM Cloud is not supported.
* Only the DNS-shared VPC authorized users can initiate the creation of a DNS resolution binding to a hub VPC. Similarly, only DNS-shared VPC authorized users can delete DNS resolution bindings.
* When the hub VPC has DNS resolution bindings to DNS-shared VPCs, you cannot disable the DNS hub VPC. In addition, authorized users cannot delete the hub VPC.
* When deleting or recreating a DNS resolution binding, if the process takes less than 5 minutes, it will fail. To fix this issue, ensure the binding process takes longer than 6 minutes.
* When disabling or enabling DNS resolution binding for endpoint gateways, if the process takes less than 5 minutes, it will fail. To fix this issue, ensure the binding process takes longer than 6 minutes.
* If you remove and recreate the same VPE on any combination of hub or DNS-shared VPCs within 5 minutes, the creation of the VPE may fail.
* Zone affinity is not supported for a custom resolver when it has multiple addresses. One custom resolver address always become the primary DNS address for all availability zones for this VPC.
* Don’t disable or delete the custom resolver on the DNS hub VPC. This can cause DNS resolution failure for the DNS hub VPC and all DNS-shared VPCs delegating the DNS resolver to it.
