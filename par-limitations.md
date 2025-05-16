---

copyright:
  years: 2025
lastupdated: "2025-05-16"

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
*  You can't divide public address ranges into subranges or bind one to multiple VPCs or zones.
* Using public address ranges with a shared virtual IP for ingress routing (for example, NSX-T Tier 0 HA VIP or similar appliances with Bare Metal Server VNI VLAN attachments), can cause an asymmetric routing issue that disrupts stateful firewall handling in security groups. 

   To mitigate this, there are two workarounds:

   * Option A: Treat the security group rules as stateless for the affected public address range prefixes.
   * Option B: Allow all inbound and outbound traffic in the security group rules, and rely on the appliance’s built-in firewall capabilities.

   These appliances are typically NFVs with integrated firewall capabilities, and the purpose of the public address range and the VPC ingress route is to direct traffic to that appliance.
