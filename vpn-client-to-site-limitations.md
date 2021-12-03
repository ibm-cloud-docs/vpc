---

copyright:
  years: 2021
lastupdated: "2021-08-26"
keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPN server limitations (Beta)
{: #vpn-client-vpn-limitations}

Client VPN for VPC is available to all IBM Cloud users. After the Beta period ends, you will be given a time period to migrate your VPN servers to the standard pricing plan to avoid disruption of service.
{: beta}

The limitations for this beta release are as follows:

* Currently, only 500 maximum active client connections are supported per server.
* Only subnets associated with the default customer routing table are accessible from the VPN client.
* Security groups support is limited. You can create and configure one or more security groups using the client-to-site UI; however, you cannot attach or detach security groups after provisioning.
* MFA support is provided by IAM.
* Currently, SDK and Terraform support is not available.
* Features not supported:
   * Upgrading from a stand-alone VPN server to High Availability (HA) mode, and downgrading from HA to a stand-alone VPN server
   * Integration with other identity providers, such as Active Directory and RADIUS
   * IPsec client-to-site VPN
   * More than two VPN appliances per VPN server
   * VPN server auto scaling
   * In-house, customized OpenVPN client  
   * VPN client user self-service portal
   * Client-to-site API are not supported over private endpoint
