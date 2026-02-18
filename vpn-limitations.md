---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for VPN gateways
{: #vpn-limitations}

Known issues are identified bugs or unexpected behaviors that were not fixed before release, but weren’t critical enough to delay it. These issues are communicated to you, often with workarounds, and are prioritized for resolution in the near term by the development team.
{: shortdesc}

Known issues for site-to-site VPN gateways are as follows:

* A VPN gateway for VPC accepts only VPN packets with [UDP encapsulation of IPsec ESP packets](https://datatracker.ietf.org/doc/html/rfc3948){: external}. [Encapsulating Security Payload (ESP)](https://datatracker.ietf.org/doc/html/rfc4303){: external} packets are not accepted. Make sure that the NAT-T feature is enabled on your on-premises VPN device. Also, make sure that UDP ports `500` and `4500` are allowed for both IBM VPC NACL and peer networks.

   NAT-T allows VPN traffic to pass through NAT devices by encapsulating IPsec packets in UDP. Without NAT-T, IPsec packets might be dropped by NAT devices because they can't properly handle ESP traffic. To achieve reliable VPN connectivity across NAT devices, NAT-T must be enabled on your on-premises device.
   {: note}

* When multiple networks, subnets, or both are associated with either an {{site.data.keyword.cloud_notm}} VPN gateway or an on-premises device, avoid mixing policy-based and route-based VPNs. Policy-based VPNs create a tunnel for each target network range. However, route-based VPNs route everything to a peer device through a single tunnel. Therefore, when multiple network ranges are configured, only a single tunnel that is associated with a single network range can be established. Combining contiguous subnets into a single superset CIDR is a valid workaround.
* Peer subnets of a VPN gateway connection cannot overlap.
* Transit Gateway prefix filtering is currently not supported for VPN gateways.
* When you connect a policy-based VPN with a route-based peer (or static, route-based VPN with a policy-based peer), use only a single network range for both sides. A policy-based VPN uses one tunnel for each associated network, while a route-based VPN only requires a single tunnel. Connections between different types of VPNs associated with multiple network ranges on either side might only work for one network range.

   ![Mixed VPN types use case](images/vpn-mixed-types.svg){: caption="Mixed VPN Types use case" caption-side="bottom"}

   If possible, combine contiguous subnets into a single network range in a VPN configuration. For example, subnets `192.168.0.0/24` and `192.168.1.0/24` can be defined as `192.168.0.0/23` in a VPN or routing configuration.
   {: tip}

* An {{site.data.keyword.cloud_notm}} policy-based VPN gateway resides in the zone that is associated with the subnet that you select during provisioning. The VPN gateway serves only virtual server instances in the same zone of the VPC. Therefore, instances in other zones can't use the VPN gateway to communicate with an on-premises private network. For zone fault tolerance, deploy one VPN gateway per zone.
* An {{site.data.keyword.cloud_notm}} route-based VPN gateway resides in the zone that is associated with the subnet that you select during provisioning. It is recommended that a VPN gateway serves only virtual server instances in the same zone of the VPC. By adding custom egress routes to the routing table to direct traffic, instances in other zones can use the route-based VPN gateway to communicate with an on-premises private network; however, this setup is not recommended. For zone fault tolerance, deploy one VPN gateway per zone.
* When you configure and optimize site-to-site IPsec VPN connections, you might encounter network performance issues, one of which is related to Maximum Transmission Unit (MTU) and Maximum Segment Size (MSS) clamping. For more information, see [IBM site-to-site VPN Maximum Transmission Unit (MTU) clamping](/docs/vpc?topic=vpc-about-mtu).
* When a route uses a VPN gateway connection as its next hop, it must be present in an egress routing table that is associated with VPC subnets. Additionally, when the VPN gateway forwards traffic into the VPN tunnel, it checks whether the source IP of this traffic is within the subnet that is attached to that routing table. If the source IP is outside that subnet, the traffic is neither encrypted nor sent through the VPN tunnel to the peer gateway. For instance, if the VPN gateway receives traffic that is routed through the ingress routing table, the traffic isn't forwarded into the VPN tunnel because the source IP is outside the subnet that is attached to the routing table.

   Creating a route in an ingress routing table with a VPN gateway connection as the next hop isn't supported.
   {: note}

* Setting the local and peer IKE identities (address, FQDN, or hostname) is optional when you create a VPN gateway. However, updating the peer address or FQDN later might affect VPN connectivity:

   * If you don't specify the local and peer IKE identities, the gateway automatically uses default values: the public IP of the VPN gateway is used as the local IKE identity, and the peer gateway’s public IP is used as the peer IKE identity. This default setup helps maintain a stable VPN connection even if the peer address changes.
   * If you explicitly set the local and peer IKE identities, you’re locking in specific values. In this case, updating the peer address or FQDN requires you to delete and re-create the VPN connection to avoid breaking connectivity.

## Distributing traffic restrictions
{: #distributing-traffic-restrictions}

When distributing traffic, the peer gateway must be able to support Equal-Cost Multi-Path routing (ECMP). Additionally, certain peer gateways might require specific configurations to enable ECMP. For more information, see the [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn#use-case-4-vpn) use case.
