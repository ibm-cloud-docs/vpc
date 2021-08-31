---

copyright:
  years: 2020
lastupdated: "2020-10-30"

keywords: VPE, virtual private endpoint, endpoint gateway, planning

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Planning for virtual private endpoint gateways
{: #planning-considerations}

Review the following considerations before creating a virtual private endpoint gateway:

* A reserved IP address bound to an endpoint gateway can receive traffic from another zone of the same VPC if your Network ACL (NACL) allows it.
* You can bind only one IP address per VPC zone to an endpoint gateway.
* You can attach up to 5 security groups when creating a VPE. If you do not specify a security group during VPE creation, the default security group for your VPC is used.

   If you do not select at least one security group, it is recommended that you update your default security group rules to minimize disruption in traffic on a newly created VPE.
   {: important}

* You can create a security group before or during provisioning.  

   * The security group must be created in the same VPC as your VPE.
   * Configure inbound rules that define what type of traffic is allowed to and from the security group. For each rule, complete the following information:

      * Specify a CIDR block or IP address for the permitted inbound traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to all sources that are attached to the selected security group.

      * Select the protocols and ports to which the rule applies.

* You can access {{site.data.keyword.cloud}} services by using either VPEs or the service endpoint directly. However, if you want your VPC to enforce a certain behavior or discipline, it is recommended to block direct access to the service endpoint IP addresses that use NACLs. For more information, see [Configuring ACLs for use with virtual private endpoints](/docs/vpc?topic=vpc-vpe-configuring-acls).

   {{site.data.keyword.cloud_notm}} services do not support accessing a service endpoint and VPE simultaneously from the same virtual instance.
   {: note}
