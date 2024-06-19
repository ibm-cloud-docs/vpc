---

copyright:
  years: 2019, 2024
lastupdated: "2024-06-19"

keywords:
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations for VPN gateways
{: #planning-considerations-vpn}

Review the following considerations before creating a VPN gateway:

* The VPN gateway is created in the zone that is associated with the subnet that you select. The VPN gateway can connect to virtual server instances in this zone only. As a result, instances in other zones can't use this VPN gateway to communicate with the other network. For zone fault tolerance, deploy one VPN gateway per zone.
* Make sure that there's enough space in the subnet for the gateway. To ensure that VPN management and fail-over functions are able to function properly, create the VPN gateway in a subnet without any other VPC resources. This guarantees that there are enough available private IP addresses for the gateway. A VPN gateway needs four private IP addresses to accommodate high availability and rolling upgrades. Because a maximum of five private IP addresses in a subnet are reserved, the minimum subnet size that can be used to host a VPN gateway is 16 IP addresses (prefix `/28` or netmask `255.255.255.240`).
* If you plan to set a default route (`0.0.0.0/0`) in a VPC routing table to let egress traffic from your VPC resources pass through a VPN gateway, and you plan to use a route-based VPN, create your VPN gateway in a subnet different from the one associated with the routing table. Otherwise, this default route causes a routing conflict for the VPN gateway and brings the VPN connection down.
* By default, PFS (Perfect Forward Secrecy) is disabled for IBM Cloud VPN for VPC. Some vendors require PFS enablement for Phase 2. Check your vendor instruction and use custom policies if you require PFS.
* IBM Cloud VPN for VPC supports only one route-based VPN per zone per VPC.
* The IBM VPN gateway uses its public IP address as the IKE local identity and designates the peer's public IP address as the IKE peer identity by default. You can specify the local and peer IKE identities to override this default behavior when you create a VPN connection. In cases where the peer VPN gateway is located behind a NAT firewall, and the peer's public IP address is not associated with the peer VPN gateway interface, you can adjust the configuration the peer VPN gateway. This ensures that the peer's public IP address is used as the IKE identity. You can also specify the peer IKE identity when you create a VPN connection to use the real IKE identity of your peer VPN gateway.
* If your peer VPN gateway is behind a NAT device and has no public IP, you can associate a FQDN with the NATed IP address. You can then use this FQDN instead of an IP address when creating a VPN connection. In this way, the default peer IKE identity is the FQDN. You can specify the peer IKE identity to override this default when you create the VPN connection if the default does not match the real IKE identity of your peer VPN gateway.
* You can select an establish mode (bidirectional or peer only) when creating a VPN connection. Keep in mind that if your peer VPN gateway has no public IP address when creating a VPN connection, you must set the VPN gateway to **Peer only** mode. In this case, the peer VPN gateway is responsible for bringing a connection up if one goes down.
* After you provision a VPN connection, you cannot change the peer gateway address type from IP address to FQDN, or from FQDN to IP address.
* IBM Cloud VPC supports FQDN resolution through [DNS Services](/docs/dns-svcs). If the VPC where the VPN gateway resides is in the permitted network of a private DNS instance, then DNS records added in this private DNS are resolvable in the VPN gateway.

## IBM Power Virtual Servers: Automate the deployment of your workspace
{: #vpn-s2s-automate-deployment-powervs-workspace}

A site-to-site VPN automation project is available that provides a Terraform module to create a site-to-site VPN gateway. It also allows a secure connection over the internet from your local on-premises network to private resources in a Power Virtual Server workspace. This Terraform Infrastructure as Code (IaC) module creates a policy-based VPN gateway and connection with local and peer policies. A Transit Gateway and IBM Power Virtual Server workspace are created by default, but you can override the default by specifying existing ones.

The Github repository for this automation project is located in the [IBM /
power-vpn-gateway Github repository](https://github.com/IBM/power-vpn-gateway/){: external}. This project [Readme file](https://github.com/IBM/power-vpn-gateway/blob/master/README.md){: external} create a VPN gateway and attaches it to a new or existing Power Virtual Server workspace, providing secure access to the IBM Cloud Power infrastructure.
