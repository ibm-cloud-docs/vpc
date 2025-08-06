---

copyright:
  years: 2019, 2025
lastupdated: "2025-08-06"

keywords:
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations for VPN gateways
{: #planning-considerations-vpn}

Review the following considerations before you create a VPN gateway:

* The VPN gateway is created in the zone that is associated with the subnet that you select. The VPN gateway can connect to virtual server instances in this zone only. As a result, instances in other zones can't use this VPN gateway to communicate with the other network. For zone fault tolerance, deploy one VPN gateway per zone.
* Make sure that there's enough space in the subnet for the gateway. To ensure that VPN management and fail-over functions are able to function properly, create the VPN gateway in a subnet without any other VPC resources. This process guarantees that enough private IP addresses are available for the gateway. A VPN gateway needs four private IP addresses to accommodate high availability and rolling upgrades. Because a maximum of five private IP addresses in a subnet are reserved, the minimum subnet size that can be used to host a VPN gateway is 16 IP addresses (prefix `/28` or netmask `255.255.255.240`).
* If you plan to set a default route (`0.0.0.0/0`) in a VPC routing table to let egress traffic from your VPC resources pass through a VPN gateway, and you plan to use a route-based VPN, create your VPN gateway in a subnet different from the one associated with the routing table. Otherwise, this default route causes a routing conflict for the VPN gateway and brings the VPN connection down.
* By default, PFS (Perfect Forward Secrecy) is disabled for IBM Cloud VPN for VPC. Some vendors require PFS enablement for Phase 2. Check your vendor instruction and use custom policies if you require PFS.
* IBM Cloud VPN for VPC supports only one route-based VPN per zone per VPC.
* The IBM VPN gateway uses its public IP address as the IKE local identity and designates the peer's public IP address as the IKE peer identity by default. You can specify the local and peer IKE identities to override this default behavior when you create a VPN connection. In cases where the peer VPN gateway is located behind a NAT firewall, and the peer's public IP address is not associated with the peer VPN gateway interface, you can adjust the configuration of the peer VPN gateway. This configuration ensures that the peer's public IP address is used as the IKE identity. You can also specify the peer IKE identity when you create a VPN connection to use the real IKE identity of your peer VPN gateway.
* If your peer VPN gateway is behind a NAT device and has no public IP, you can associate an FQDN with the NATed IP address. You can then use this FQDN instead of an IP address when you create a VPN connection. In this way, the default peer IKE identity is the FQDN. You can specify the peer IKE identity to override this default when you create the VPN connection if the default does not match the real IKE identity of your peer VPN gateway.
* You can select an establish mode (bidirectional or peer only) when you create a VPN connection. Keep in mind that if your peer VPN gateway has no public IP address when creating a VPN connection, you must set the VPN gateway to **Peer only** mode. In this case, the peer VPN gateway is responsible for bringing the connection up if one goes down.
* After you provision a VPN connection, you cannot change the peer gateway address type from IP address to FQDN, or from FQDN to IP address.
* IBM Cloud VPC supports FQDN resolution through [DNS Services](/docs/dns-svcs). If the VPC where the VPN gateway resides is in the permitted network of a private DNS instance, then DNS records that are added in this private DNS are resolvable in the VPN gateway.


* A VPN gateway for VPC accepts VPN packets with [UDP Encapsulation of IPsec ESP Packets](https://datatracker.ietf.org/doc/html/rfc3948){: external} only. The [Encapsulating Security Payload (ESP)](https://datatracker.ietf.org/doc/html/rfc4303){: external} is not accepted. Make sure that the NAT-T feature is enabled on your on-premises VPN device. Also, make sure that UDP ports `500` and `4500` are allowed for both IBM VPC NACL and peer networks.
* When multiple networks, subnets, or both are associated with either an {{site.data.keyword.cloud_notm}} VPN gateway or an on-premises device, avoid mixing policy-based and route-based VPNs. Policy-based VPNs create a tunnel for each target network range. However, route-based VPNs route everything to a peer device through a single tunnel. Therefore, when multiple network ranges are configured, only a single tunnel that is associated with a single-network range can be established. Combining contiguous subnets into a single superset CIDR is a valid workaround to this limitation.
* Peer subnets of a VPN gateway connection cannot overlap.
* When you connect a policy-based VPN with a route-based peer (or static, route-based VPN with a policy-based peer), use only a single network range for both sides. A policy-based VPN uses one tunnel for each associated network. However, a route-based VPN requires only a single tunnel. Therefore, a connection between different types of VPNs associated with multiple network ranges on either side might result in a connection that only works for a single-network range.

   ![Mixed VPN types use case](images/vpn-mixed-types.svg){: caption="Mixed VPN Types use case" caption-side="bottom"}

   If possible, combine contiguous subnets into a single network range in a VPN configuration. For example, subnets `192.168.0.0/24` and `192.168.1.0/24` can be defined as `192.168.0.0/23` in a VPN or routing configuration.
   {: tip}

* An {{site.data.keyword.cloud_notm}} policy-based VPN gateway resides in the zone that is associated with the subnet that you select during provisioning. The VPN gateway serves only the virtual server instances in the same zone of the VPC. Therefore, instances in other zones can't use the VPN gateway to communicate with an on-premises private network. For zone fault tolerance, you must deploy one VPN gateway per zone.
* An {{site.data.keyword.cloud_notm}} route-based VPN gateway resides in the zone that is associated with the subnet that you select during provisioning. It is recommended that a VPN gateway serves only virtual server instances in the same zone of the VPC. Instances in other zones can use the route-based VPN gateway to communicate with an on-premises private network, but this setup is not recommended. For zone fault tolerance, you must deploy one VPN gateway per zone.
* When you configure and optimize site-to-site IPsec VPN connections, you might encounter network performance issues, one of which is related to Maximum Transmission Unit (MTU) and Maximum Segment Size (MSS) clamping. For more information, see [IBM site-to-site VPN Maximum Transmission Unit (MTU) clamping](/docs/vpc?topic=vpc-about-mtu).
* When a route uses a VPN gateway connection as its next hop, it must be present in an egress routing table that is associated with VPC subnets. Additionally, when the VPN gateway forwards traffic into the VPN tunnel, it checks whether the source IP of this traffic is within the subnet that is attached to that routing table. If the source IP is outside that subnet, the traffic isn't encrypted or sent through the VPN tunnel to the peer gateway. For instance, if the VPN gateway receives traffic that is routed through the ingress routing table, the traffic isn't forwarded into the VPN tunnel because the source IP is outside the subnet that is attached to the routing table.

   Creating a route in an ingress routing table with a VPN gateway connection as the next hop isn't supported.
   {: note}

* Setting the local and peer IKE identities such as address, FQDN, or hostname is optional when you create a VPN gateway. However, if you later update the peer address or FQDN, you must consider how this change affects VPN connectivity:

   * If you don't specify the local and peer IKE identities when you create a VPN gateway, the gateway automatically uses default values: the public IP of the VPN gateway is used as the local IKE identity, and the peer gateway’s public IP is used as the peer IKE identity. This default setup helps maintain a stable VPN connection even if the peer address changes.
   * If you explicitly set the local and peer IKE identities when you create a VPN gateway, you’re locking in specific values. In this case, updating the peer address or FQDN requires you to delete and re-create the VPN connection to avoid breaking connectivity.
* Restriction: When distributing traffic, the peer gateway must be able to support Equal-Cost Multi-Path routing (ECMP). Additionally, certain peer gateways might require specific configurations to enable ECMP. For more information, see the [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn#use-case-4-vpn) use case.

## IBM Power Virtual Servers: Automate the deployment of your workspace
{: #vpn-s2s-automate-deployment-powervs-workspace}

A site-to-site VPN automation project is available that provides a Terraform module to create a site-to-site VPN gateway. It also allows a secure connection over the internet from your local on-premises network to private resources in a Power Virtual Server workspace. This Terraform Infrastructure as Code (IaC) module creates a policy-based VPN gateway and connection with local and peer policies. A Transit Gateway and IBM Power Virtual Server workspace are created by default, but you can override the default by specifying existing ones.

The GitHub repository for this automation project is located in the [IBM /
power-vpn-gateway GitHub repository](https://github.com/IBM/power-vpn-gateway/){: external}. This project [Readme file](https://github.com/IBM/power-vpn-gateway/blob/master/README.md){: external} creates a VPN gateway and attaches it to a new or existing Power Virtual Server workspace, providing secure access to the IBM Cloud Power infrastructure.
