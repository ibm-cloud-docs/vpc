---

copyright:
  years: 2023, 2026
lastupdated: "2026-10-23"
keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for VPN servers
{: #vpn-client-vpn-limitations}

Known issues are identified bugs or unexpected behaviors that were not fixed before release, but weren’t critical enough to delay it. These issues are communicated to you, often with workarounds, and are prioritized for resolution in the near term by the development team.
{: shortdesc}

Known issues for client-to-site VPN servers are as follows:

* Currently, only 2000 maximum active client connections are supported per server.
* MFA support is provided by IAM.
* Features not supported:
   * Integration with other identity providers, such as Active Directory and RADIUS
   * IPsec client-to-site VPN
   * More than two VPN appliances per VPN server
   * VPN server auto scaling
   * In-house, customized OpenVPN client
   * VPN client user self-service portal
* In client certificate authentication mode or client certificate and User ID/passcode authentication mode, authentication failure due to an invalid client certificate does not generate an activity track event.
* Some countries or regions (such as Egypt) enforce internet filtering or regulatory restrictions that block OpenVPN traffic and protocols. Users who attempt to connect to a client-to-site VPN server from these locations might experience connection failures, such as TLS handshake timeouts or key negotiation errors. For more information, see [Regional restrictions on the use of OpenVPN products](https://support.openvpn.com/hc/en-us/articles/19196576161819-Regional-restrictions-on-the-use-of-OpenVPN-products){: external}.
