---

copyright:
  years: 2023, 2024
lastupdated: "2024-06-20"
keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPN server limitations
{: #vpn-client-vpn-limitations}

The limitations for this release are as follows:

* Currently, only 2000 maximum active client connections are supported per server.
* MFA support is provided by IAM.
* Features not supported:
   * Integration with other identity providers, such as Active Directory and RADIUS
   * IPsec client-to-site VPN
   * More than two VPN appliances per VPN server
   * VPN server auto scaling
   * In-house, customized OpenVPN client
   * VPN client user self-service portal
* In client certificate authentication mode or client certificate and User ID/passcode authentication mode, authentication failure due to an invalid client certificate will not generate an activity track event
