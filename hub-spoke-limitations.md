---

copyright:
  years: 2023, 2024
lastupdated: "2024-03-29"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues and limitations when configuring DNS sharing for VPE gateways
{: #vpe-dns-sharing-limitations}

Review the following issues and limitations before you configure DNS sharing for VPE gateways:
{: shortdesc}

* Hub and DNS-shared VPCs must be in the same region. There is no support for DNS-shared VPCs in a remote region.
* VPEs on the DNS hub VPC will always be shared with their associated DNS-shared VPCs. You must configure all VPEs on the DNS hub with `allow_dns_resolution` enabled before the VPC can be enabled as a DNS hub.
* Control of on-prem DNS servers pointing to a DNS custom resolver of the hub VPC on IBM Cloud is not supported.
* A DNS-shared VPC can only be associated to a single hub.
* When the hub VPC has DNS resolution bindings to DNS-shared VPCs, you cannot disable the DNS hub VPC. In addition, authorized users cannot delete the hub VPC.
* Zone affinity is not supported for a custom resolver when it has multiple addresses. One custom resolver address always becomes the primary DNS address for all availability zones for this VPC.
* You cannot disable or delete the custom resolver on the DNS hub VPC as long as it is in use.
* When deleting or recreating a DNS resolution binding, if the process takes less than 5 minutes, it will fail. To fix this issue, ensure the binding process takes longer than 6 minutes.
* When disabling or enabling DNS resolution binding for endpoint gateways, if the process takes less than 5 minutes, it will fail. To fix this issue, ensure the binding process takes longer than 6 minutes.
* If you remove and recreate the same VPE on any combination of hub or DNS-shared VPCs within a span of 5 minutes, the creation of the VPE may fail.
