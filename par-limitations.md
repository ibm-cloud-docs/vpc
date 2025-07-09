---

copyright:
  years: 2025
lastupdated: "2025-07-09"

keywords: public address ranges, limitations

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues (limitations) for public address ranges
{: #par-limitations} 

You can now create and use public address ranges in the Frankfurt and Madrid regions, if your account has been approved for access. To request access to Public Address Ranges for VPC, contact your IBM representative.
{: preview} 

Before you create a public address range, review the following limitations:
{: shortdesc}

* You can't assign IP addresses from a public address range to resources in a VPC. You can only use these destination IP addresses in ingress custom route tables with "Public internet" enabled as the Traffic source to direct traffic to a next-hop target resource, such as a virtual server instance, network appliance, or other compute resource.
* This service only supports IBM-provided public IP ranges. Bringing your own public IP or subnet is not supported. 
*  You can't divide public address ranges into subranges or bind one to multiple VPCs or zones.
* Using public address ranges with a shared virtual IP for ingress routing (for example, NSX-T Tier 0 HA VIP or similar appliances with Bare Metal Server VNI VLAN attachments), can cause an asymmetric routing issue that disrupts stateful firewall handling in security groups. 

   To mitigate this, there are two workarounds:

   * Option A: Treat the security group rules as stateless for the affected public address range prefixes.
   * Option B: Allow all inbound and outbound traffic in the security group rules, and rely on the appliance’s built-in firewall capabilities.

   These appliances are typically NFVs with integrated firewall capabilities, and the purpose of the public address range and the VPC ingress route is to direct traffic to that appliance.
