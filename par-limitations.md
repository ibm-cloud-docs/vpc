---

copyright:
  years: 2025
lastupdated: "2025-04-17"

keywords: public address ranges, limitations

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Public address range limitations
{: #par-limitations}

Public Address Ranges for VPC is only available for evaluation and testing purposes for users with special access. 
{: beta}

Before you create a public address range, review the following limitations:
{: shortdesc}

* You can't assign IP addresses from a public range to resources in a VPC. These IPs can only be used in custom routes to configure the routing to the intended Network Functions Virtualization (NFV) appliance.
* This service only supports IBM-provided public IP ranges. Bringing your own public IP or subnet is not supported. 
*  You can't divide public address ranges into subranges and bind it to multiple VPCs or zones.
