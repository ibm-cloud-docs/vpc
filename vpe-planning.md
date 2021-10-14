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
* You can access {{site.data.keyword.cloud}} services by using either VPEs or the service endpoint directly. However, if you want your VPC to enforce a certain behavior or discipline, it is recommended to block direct access to the service endpoint IP addresses that use NACLs. For more information, see [Configuring ACLs for use with virtual private endpoints](/docs/vpc?topic=vpc-vpe-configuring-acls).

   {{site.data.keyword.cloud_notm}} services do not support accessing a service endpoint and VPE simultaneously from the same virtual instance.
   {: note}
