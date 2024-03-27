---

copyright:
  years: 2019, 2024
lastupdated: "2024-0-28"

keywords:
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations for VPN gateways
{: #planning-considerations-vpn}

Review the following considerations before creating a VPN gateway:

* The VPN gateway is created in the zone that is associated with the subnet that you select. Because the VPN gateway can connect to virtual server instances in this zone only, instances in other zones can't use this VPN gateway to communicate with the other network. For zone fault tolerance, deploy one VPN gateway per zone.
* Make sure that there's enough space in the subnet for the gateway. To ensure that VPN management and fail-over functions are able to function properly, create the VPN gateway in a subnet without any other VPC resources to guarantee that there are enough available private IP addresses for the gateway. A VPN gateway needs four private IP addresses to accommodate high availability and rolling upgrades. Because a maximum of five private IP addresses in a subnet are reserved, the minimum subnet size that can be used to host a VPN gateway is 16 IP addresses (prefix `/28` or netmask `255.255.255.240`).
* If you plan to set a default route (`0.0.0.0/0`) in a VPC routing table to let egress traffic from your VPC resources pass through a VPN gateway, and you plan to use a route-based VPN, create your VPN gateway in a subnet different from the one associated with the routing table. Otherwise, this default route causes a routing conflict for the VPN gateway and brings the VPN connection down.
* By default, PFS (Perfect Forward Secrecy) is disabled for IBM Cloud VPN for VPC. Some vendors require PFS enablement for Phase 2. Check your vendor instruction and use custom policies if PFS is required.
* IBM Cloud VPN for VPC supports only one route-based VPN per zone per VPC.

   The IBM VPN gateway uses its public IP address as the IKE local identify and designates the peer's public IP address as the IKE peer identify. In cases where the peer VPN gateway is located behind a NAT firewall, and the peer's public IP address is not associated with the peer VPN gateway interface, it is imperative to adjust the configuration of the peer VPN gateway to ensure that the peer's public IP address is used as the IKE identify.
   {: note}

## IBM Power Virtual Servers: Automate deployment of your workspace
{: #vpn-s2s-automate-deployment-powervs-workspace}

A site-to-site VPN automation project is available that provides a Terraform module to create a site-to-site VPN gateway, allows a secure connection over the internet from your local on-premises network to private resources in a Power Virtual Server workspace. This Terraform IaC creates a policy-based VPN gateway and connection with local and peer policies. A Transit Gateway and IBM Power Virtual Server workspace are created by default, but you can override the default by specifying existing ones.

The Github repository for this automation project is located in the [IBM /
power-vpn-gateway Github repository](https://github.com/IBM/power-vpn-gateway/){: external}. This project [Readme file](https://github.com/IBM/power-vpn-gateway/blob/master/README.md){: external} create a VPN gateway and attaches it to a new or existing Power Virtual Server workspace, providing secure access to the IBM Cloud Power infrastructure.
