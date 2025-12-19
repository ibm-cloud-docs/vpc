---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-19"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I update the system DNS resolver for the DNS hub VPC from "custom_resolver" to "private_resolver" or "default"?
{: #troubleshoot-hub-5}
{: troubleshoot}
{: support}

Three configurations (`default`,`private_resolver`,`custom_resolver`) exist with the "system" DNS resolver. These configurations are automatically updated by the system. However, if you run into the issue where the configuration fails to automatically update, follow these steps for the update to be successful.
{: shortdesc}

* If there is a custom resolver for the DNS hub VPC, the configuration of the DNS resolver is automatically updated to "custom_resolver".
* If a VPE gateway is created for the DNS hub VPC and there is no custom resolver, the configuration of the DNS resolver is automatically updated to "private_resolver".
* If there is neither a custom resolver nor VPE gateway, the configuration of the DNS resolver is automatically updated to "default".

Possible causes include:
{: tsCauses}

* The DNS resolution binding exists on the DNS hub VPC.
* The DNS hub VPC is configured with a custom resolver enabled.

To resolve this issue, follow these steps:
{: tsResolve}

1. List all the resolution bindings of the DNS hub VPC to view all the bound DNS-shared VPCs.
1. Delete all the resolution bindings from DNS-shared VPCs.
1. Disable the custom resolver.
