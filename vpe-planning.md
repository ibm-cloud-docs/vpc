---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-02"

keywords: VPE, virtual private endpoint, endpoint gateway, planning

---

{{site.data.keyword.attribute-definition-list}}

# Planning for virtual private endpoint gateways
{: #planning-considerations}

Before you create a virtual private endpoint gateway, review the following considerations.

* A reserved IP address that is bound to a VPE gateway can receive traffic from other zones within the same VPC, as long as your Network ACL (NACL) rules allow it.
* You can create only one VPE per service within a single VPC.
* You can bind only one IP address per VPC zone to a VPE gateway.
* You can attach up to five security groups when you create a VPE. If you do not specify a security group during VPE creation, the default security group for your VPC is used.

   If you do not select at least one security group, it is recommended that you update your default security group rules to minimize disruption in traffic on a newly created VPE.
   {: important}

* You can create a security group before provisioning (or during provisioning in the console).

   * The security group must be created in the same VPC as your VPE.
   * Configure inbound rules that define what type of traffic is allowed to the VPE. For each rule, complete the following information:

      * Specify a CIDR block or IP address for the permitted inbound traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to all sources that are attached to the selected security group.

      * Select the protocols and ports to which the rule applies.

* You can access {{site.data.keyword.cloud}} services by using either VPEs or the service endpoint directly. However, if you want your VPC to enforce a certain behavior or discipline, it is recommended to block direct access to the service endpoint IP addresses that use NACLs. For more information, see [Configuring ACLs and security groups for use with endpoint gateways](/docs/vpc?topic=vpc-configure-acls-sgs-endpoint-gateways).

   {{site.data.keyword.cloud_notm}} services do not support accessing a service endpoint and VPE simultaneously from the same virtual instance.
   {: note} 

* The following items are not supported:

   * Services that are in zones and regions other than [IBM Cloud Multi-Zone Regions (MZRs)](/docs/overview?topic=overview-locations#table-mzr)
   * {{site.data.keyword.cloud_notm}} Flow Logs for VPC
   * {{site.data.keyword.cloud_notm}} Bare Metal Servers for VPC
   * VPE for User Datagram Protocol (UDP) services, for example, Network Time Protocol (NTP), is not accessible over {{site.data.keyword.cloud_notm}} Direct Link and {{site.data.keyword.cloud_notm}} Transit Gateway.

* The following items are architectural restrictions:

   * Virtual private endpoints support only IPv4 addressing.
   * Each endpoint gateway is bound to a single VPC network.
   * Each endpoint gateway to a service mapping varies based on the service that you are enabling. For best practices, consult the documentation for the specific {{site.data.keyword.cloud_notm}} service that you are enabling.
