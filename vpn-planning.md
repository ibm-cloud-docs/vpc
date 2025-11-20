---

copyright:
  years: 2019, 2025
lastupdated: "2025-11-20"

keywords:
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations for VPN gateways
{: #planning-considerations-vpn}

Before creating a VPN gateway, review applicable planning considerations, configuration requirements, and other guidelines.

## General considerations
{: #general-considerations-vpn}

Review the following general considerations before you create a VPN gateway:

* Make sure that there's enough space in the subnet for the gateway. To ensure that VPN management and fail-over functions are able to function properly, create the VPN gateway in a subnet without any other VPC resources. This process guarantees that enough private IP addresses are available for the gateway. A VPN gateway needs four private IP addresses to accommodate high availability and rolling upgrades. Because a maximum of five private IP addresses in a subnet are reserved, the minimum subnet size that can be used to host a VPN gateway is `/28` (16 IP addresses or netmask `255.255.255.240`).
* By default, PFS (Perfect Forward Secrecy) is disabled for IBM Cloud VPN for VPC. Some vendors require PFS enablement for Phase 2. Check your vendor instruction and use custom policies if you require PFS.
* The IBM VPN gateway uses its public IP address as the IKE local identity and designates the peer's public IP address as the IKE peer identity by default. You can specify the local and peer IKE identities to override this default behavior when you create a VPN connection. In cases where the peer VPN gateway is located behind a NAT firewall, and the peer's public IP address is not associated with the peer VPN gateway interface, you can adjust the configuration of the peer VPN gateway. This configuration ensures that the peer's public IP address is used as the IKE identity. You can also specify the peer IKE identity when you create a VPN connection to use the real IKE identity of your peer VPN gateway.
* If your peer VPN gateway is behind a NAT device and has no public IP, you can associate an FQDN with the NATed IP address. You can then use this FQDN instead of an IP address when you create a VPN connection. In this way, the default peer IKE identity is the FQDN. You can specify the peer IKE identity to override this default when you create the VPN connection if the default doesn't match the actual IKE identity of your peer VPN gateway.
* You can select an establish mode (bidirectional or peer only) when you create a VPN connection. Keep in mind that if your peer VPN gateway has no public IP address when creating a VPN connection, you must set the VPN gateway to **Peer only** mode. In this case, the peer VPN gateway is responsible for bringing up the connection if one goes down.
* After you provision a VPN connection, you can't change the peer gateway address type from IP address to FQDN, or from FQDN to IP address.
* IBM Cloud VPC supports FQDN resolution through [DNS Services](/docs/dns-svcs). If the VPC where the VPN gateway resides is in the permitted network of a private DNS instance, then DNS records that are added in this private DNS are resolvable in the VPN gateway.

## Policy-based VPN gateway considerations
{: #policy-based-vpn-considerations}

Review the following consideration before you create a policy-based VPN gateway:

* The policy-based VPN gateway is created in the zone that is associated with the subnet that you select. The VPN gateway can connect to virtual server instances in this zone only. As a result, instances in other zones can't use this VPN gateway to communicate with the other network. For zone fault tolerance, deploy one VPN gateway per zone.

## Route-based VPN gateway considerations
{: #route-based-vpn-considerations}

Review the following considerations before you create a route-based VPN gateway:

### Static route-based VPN connection considerations
{: #static-route-based-connection-considerations}

Review the following considerations before you create a static route-based VPN connection:

* If you plan to set a default route (`0.0.0.0/0`) in a VPC routing table to let egress traffic from your VPC resources pass through a VPN gateway, create your VPN gateway in a subnet different from the one associated with the routing table. Otherwise, this default route causes a routing conflict for the VPN gateway and can bring the VPN connection down.
* IBM Cloud VPN for VPC supports only one route-based VPN per zone per VPC.

### Dynamic route-based VPN connection considerations
{: #dynamic-route-based-connection-considerations}

Review the following considerations before you create a dynamic route-based VPN connection:

* To enable dynamic routing between your VPN gateway and on-premises devices, you must create a transit gateway and attach it to the VPN gateway. In this setup, the transit gateway automatically manages and distributes traffic between your devices. See [VPN gateway connection considerations](/docs/transit-gateway?topic=transit-gateway-helpful-tips#vpn-connection-considerations) with Transit Gateway. Keep in mind that static routing doesn't support this type of attachment.
* You can create dynamic VPN connections at any time, even before the VPN gateway is attached to the transit gateway. However, traffic flows only when the VPN gateway is attached to the transit gateway.
* A local and peer ASN value is required for dynamic route-based VPN. The local ASN identifies your local network for BGP peering whereas the peer ASN identifies the remote peer network with which the VPN exchanges routes. If you don't specify the local ASN value, the VPN gateway is created with the default ASN of `64520`.
* Certain ASN values are restricted when you create a VPN gateway and can't be used as local or peer ASNs, which include `0`, `13884`, `36351`, `64512`, `64513`, `65100`, `65200–‍65234`, `65402‍–‍65433`, `65500`, or `4201065000‍–‍4201065999`. These values are either reserved or part of private ASN ranges, and can cause routing conflicts.
* When you connect a transit gateway to a VPN, you can't change the ASN value until you remove the service connection.
* Multiple VPN gateways can connect to the same transit gateway, but each VPN gateway can attach to only one transit gateway.
* If you plan to use a non-RFC 1918 CIDR for your transit gateway connection that is not within the standard private IP ranges (`10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`), you must add a delegate-VPC route in your VPC's egress routing table. This route must point to the CIDR that you've chosen and must be associated with the VPN subnet in the same zone. See [VPN gateway connection considerations](/docs/transit-gateway?topic=transit-gateway-helpful-tips#vpn-connection-considerations) for transit gateway.
* Each IBM VPN supports a maximum of 120 routes for each VPN peer in a dynamic routing setup. If this limit is exceeded, the BGP session for that peer automatically shuts down. To restore the session, reduce the number of routes advertised from your on-premises peer network to 120 or less, then toggle the connection in the IBM Cloud to reestablish the BGP session.
* Each VPN appliance supports a total of 120 routes, regardless of the number of connected peers. If more than 120 routes are received across a combination of peers, the appliance forwards only the first 120 routes to the transit gateway. For example, if two peers each send 70 routes to the VPN appliance, only the first 120 routes are propagated to the transit gateway.

## IBM Power Virtual Servers: Automate the deployment of your workspace
{: #vpn-s2s-automate-deployment-powervs-workspace}

A site-to-site VPN automation project is available that provides a Terraform module to create a site-to-site VPN gateway. It also allows a secure connection over the internet from your local on-premises network to private resources in a Power Virtual Server workspace. This Terraform Infrastructure as Code (IaC) module creates a policy-based VPN gateway and connection with local and peer policies. A Transit Gateway and IBM Power Virtual Server workspace are created by default, but you can override the default by specifying existing ones.

The GitHub repository for this automation project is located in the [IBM /
power-vpn-gateway GitHub repository](https://github.com/IBM/power-vpn-gateway/){: external}. The project's [README file](https://github.com/IBM/power-vpn-gateway/blob/master/README.md){: external} creates a VPN gateway and attaches it to a new or existing Power Virtual Server workspace, providing secure access to the IBM Cloud Power infrastructure.

## Related link
{: #related-link-vpn-gateways}

[Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations)
