---

copyright:
  years: 2019, 2025
lastupdated: "2025-07-28"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for VPN gateways
{: #vpn-limitations}

Lists known limitations, issues, and restrictions for IBM Cloud VPN for VPC.
{: shortdesc}

* A VPN gateway for VPC accepts VPN packets with [UDP Encapsulation of IPsec ESP Packets](https://datatracker.ietf.org/doc/html/rfc3948){: external} only. The [Encapsulating Security Payload (ESP)](https://datatracker.ietf.org/doc/html/rfc4303){: external} is not accepted. Make sure that the NAT-T feature is enabled on your on-premises VPN device. Also, make sure that UDP ports `500` and `4500` are allowed for both IBM VPC NACL and peer networks.
* When multiple networks, subnets, or both are associated with either an {{site.data.keyword.cloud_notm}} VPN gateway or an on-premises device, avoid mixing policy-based and route-based VPNs. Policy-based VPNs create a tunnel for each target network range. However, route-based VPNs route everything to a peer device through a single tunnel. Therefore, when multiple network ranges are configured, only a single tunnel that is associated with a single-network range can be established. Combining contiguous subnets into a single superset CIDR is a valid workaround to this limitation.
* Peer subnets of a VPN gateway connection cannot overlap.
* When you connect a policy-based VPN with a route-based peer (or static, route-based VPN with a policy-based peer), use only a single network range for both sides. A policy-based VPN uses one tunnel for each associated network. However, a route-based VPN requires only a single tunnel. Therefore, a connection between different types of VPNs associated with multiple network ranges on either side might result in a connection that only works for a single-network range.

   ![Mixed VPN types use case](images/vpn-mixed-types.png){: caption="Mixed VPN Types use case" caption-side="bottom"}

   If possible, combine contiguous subnets into a single network range in a VPN configuration. For example, subnets `192.168.0.0/24` and `192.168.1.0/24` can be defined as `192.168.0.0/23` in a VPN or routing configuration.
   {: tip}

* An {{site.data.keyword.cloud_notm}} policy-based VPN gateway resides in the zone that is associated with the subnet that you select during provisioning. The VPN gateway serves only the virtual server instances in the same zone of the VPC. Therefore, instances in other zones can't use the VPN gateway to communicate with an on-premises private network. For zone fault tolerance, you must deploy one VPN gateway per zone.
* An {{site.data.keyword.cloud_notm}} route-based VPN gateway resides in the zone that is associated with the subnet that you select during provisioning. It is recommended that a VPN gateway serves only virtual server instances in the same zone of the VPC. Instances in other zones can use the route-based VPN gateway to communicate with an on-premises private network, but this setup is not recommended. For zone fault tolerance, you must deploy one VPN gateway per zone.
* When you configure and optimize site-to-site IPsec VPN connections, you might encounter network performance issues, one of which is related to Maximum Transmission Unit (MTU) and Maximum Segment Size (MSS) clamping. For more information, see [IBM site-to-site VPN Maximum Transmission Unit (MTU) clamping](/docs/vpc?topic=vpc-about-mtu).
* When a route uses a VPN gateway connection as its next hop, it must be present in an egress routing table that is associated with VPC subnets. Additionally, when the VPN gateway forwards traffic into the VPN tunnel, it checks whether the source IP of this traffic is within the subnet that is attached to that routing table. If the source IP is outside that subnet, the traffic isn't encrypted or sent through the VPN tunnel to the peer gateway. For instance, if the VPN gateway receives traffic that is routed through the ingress routing table, the traffic isn't forwarded into the VPN tunnel because the source IP is outside the subnet that is attached to the routing table.

   Creating a route in an ingress routing table with a VPN gateway connection as the next hop isn't supported.
   {: note}

* When you update the `peer.address` or `peer.fqdn` of a VPN gateway connection, it affects how the VPN identifies and connects to your remote peer.

   * If you didn’t explicitly set the `local.ike_identities` and `peer.ike_identity` properties when you create the VPN gateway connection, [updating a VPN gateway connection](/apidocs/vpc/latest#update-vpn-gateway-connection) and specifying either `peer.address` or `peer.fqdn` changes the `peer.ike_identity` automatically to match the updated address or domain name. This setup maintains a working VPN connection without manual changes.
   * However, if you explicitly set the `local.ike_identities` and `peer.ike_identity` properties when you create the VPN gateway connection, it doesn't update the `peer.ike_identity` automatically when you change the address or domain. To avoid breaking the VPN connection in this case, you must delete and re-create the VPN connection to apply the changes.

## Distributing traffic restrictions
{: #distributing-traffic-restrictions}

When distributing traffic, the peer gateway must be able to support Equal-Cost Multi-Path routing (ECMP). Additionally, certain peer gateways might require specific configurations to enable ECMP. For more information, see the [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn#use-case-4-vpn) use case.
