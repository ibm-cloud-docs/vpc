---

copyright:
  years: 2021, 2025
lastupdated: "2025-11-07"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPNs for VPC overview
{: #vpn-overview}

IBM Cloud has two VPN services. _VPN for VPC_ offers site-to-site gateways, which connect your on-premises network to the IBM Cloud VPC network. _Client VPN for VPC_ offers client-to-site servers, which allow clients on the internet to connect to VPN servers, while still maintaining secure connectivity.
{: shortdesc}

## Site-to-site gateways
{: #site-to-site-vpn-gateway}

IBM Cloud VPN for VPC provides a simple, yet powerful solution for highly scalable and robust site-to-site VPN gateways. With this service, you can create site-to-site VPN tunnels for secure, encrypted connectivity. Connect from on-premises sites to IBM Cloud through a VPN gateway on an IBM Cloud VPC, and a peer gateway on-premises. 

This service provides a mixture of industry-standard security and encryption options as well as support for Pre-Shared Key (PSK) authentication. This service also provides the ability to quickly add and remove VPN connections with the option to use pre-defined configurations. For more information, see [About site-to-site VPN gateways](/docs/vpc?topic=vpc-using-vpn).

The following features are included:

* Secure tunnels - Create a VPN in route-based or policy-based mode to set up IPsec site-to-site tunnels between your VPC and your on-premises private network, or another VPC.
* High availability - Built on two VPN devices, provides appliance-level redundancy. 
* Pre-defined and custom encryption proposals - Choose from multiple pre-defined proposals for a quick, secure VPN configuration with completely customizable Internet Key Exchange (IKE) Phase 1 and Phase 2 encryption settings.
* Monitoring - View the monitoring dashboard to see the current status of all tunnels and connections. You can also suspend and restart your individual VPN connections at any time.

## Client-to-site servers
{: #client-to-site-vpn-server}

IBM Cloud Client VPN for VPC provides an open-source compatible client-to-site VPN solution that allows users to connect to IBM Cloud resources through secure, encrypted connections. 

When your users are working remotely, traveling, or at a location without a site-to-site VPN connection, they can use an OpenVPN client to connect to VPN servers on your IBM Cloud VPC. For more information, see [About client-to-site VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-overview).

The following features are included:

* Secure, encrypted connectivity - A TLS 1.2/1.3-based secure, encrypted way for your employees (or individuals) to access IBM Cloud VPC remotely. With VPN servers set up for client connections and support of Transit Gateway, you can also privately interconnect to Classic IaaS and other VPCs on IBM Cloud.
* High availability - Spans multiple availability zones for high throughput and resiliency.
* Multi-factor authentication - Apply to your VPN server and client connections to provide an added layer of security to meet compliance and security requirements.
* Operations and management - View the VPN server dashboard to monitor status and manage the lifecycle of VPN servers and their client connections. In different business scenarios, you can also delete clients that are connected to the servers. 
