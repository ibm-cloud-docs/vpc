---

copyright:
  years: 2019, 2023
lastupdated: "2023-09-21"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPN gateway limitations
{: #vpn-limitations}

Lists known limitations for IBM Cloud VPN for VPC.
{: shortdesc}

* A VPN gateway for VPC accepts VPN packets with [UDP Encapsulation of IPsec ESP Packets]( https://datatracker.ietf.org/doc/html/rfc3948){: external} only. The [Encapsulating Security Payload (ESP)](https://datatracker.ietf.org/doc/html/rfc4303){: external} is not accepted. Make sure that the NAT-T feature is enabled on your on-premises VPN device. Also, make sure that UDP ports 500 and 4500 are allowed for both IBM VPC NACL and peer networks.
* When multiple networks, subnets, or both are associated with either an {{site.data.keyword.cloud_notm}} VPN gateway or an on-premises device, avoid mixing policy-based and route-based VPNs. Policy-based VPNs create a tunnel for each target network range. However, route-based VPNs route everything to a peer device through a single tunnel. Therefore, when multiple network ranges are configured, only a single tunnel that is associated with a single-network range can be established. Combining contiguous subnets into a single superset CIDR is a valid workaround to this limitation.
* Peer subnets of a VPN gateway connection cannot overlap.
* When connecting a policy-based VPN with a route-based peer (or static, route-based VPN with a policy-based peer), use only a single network range for both sides. A policy-based VPN uses one tunnel for each associated network. However, a route-based VPN requires only a single tunnel. Therefore, a connection between different types of VPNs associated with multiple network ranges on either side might result in a connection that only works for a single-network range.

   ![Mixed VPN Types use case](images/vpn-mixed-types.png){: caption="Figure 2. Mixed VPN Types use case" caption-side="bottom"}

   If possible, combine contiguous subnets into a single network range in a VPN configuration. For example, subnets `192.168.0.0/24` and `192.168.1.0/24` can be defined as `192.168.0.0/23` in a VPN or routing configuration.
   {: tip}

* An {{site.data.keyword.cloud_notm}} policy-based VPN gateway resides in the zone that is associated with the subnet that you select during provisioning. The VPN gateway serves only the virtual server instances in the same zone of the VPC. Therefore, instances in other zones can't use the VPN gateway to communicate with an on-premises private network. For zone fault tolerance, you must deploy one VPN gateway per zone.
* An {{site.data.keyword.cloud_notm}} route-based VPN gateway resides in the zone that is associated with the subnet that you select during provisioning. It is recommended that a VPN gateway serves only virtual server instances in the same zone of the VPC. Instances in other zones can use the route-based VPN gateway to communicate with an on-premises private network, but this is not recommended. For zone fault tolerance, you must deploy one VPN gateway per zone.
* When you configure and optimize site-to-site IPsec VPN connections, you might encounter network performance issues, one of which is related to Maximum Transmission Unit (MTU) and Maximum Segment Size (MSS) clamping. For more information, see [IBM site-to-site VPN Maximum Transmission Unit (MTU) clamping](/docs/vpc?topic=vpc-about-mtu).
