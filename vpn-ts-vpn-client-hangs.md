---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my VPN client hang when I initiate a VPN session?
{: #troubleshoot-vpn-client-hangs}
{: troubleshoot}
{: support}

The VPN server runs on the protocol and port that is specified when the server is provisioned. If a firewall is blocking traffic from your VPN client to the VPN server, your network administrator must open the protocol and port. For example, if your internet service provider (ISP) is blocking traffic, you can ask your VPN server administrator to change the protocol and port to an unblocked protocol and port. Typically, the TCP 443 port is not blocked by ISPs. Another root cause of a client not connecting to the VPN server is that the client certificate is expired.
{: shortdesc}

The VPN client logs the following error `TLS Error: TLS key negotiation failed to occur within 60 seconds (check your network connectivity)`.
{: tsSymptoms}

This error can occur due to the following reasons:
{: tsCauses}

* A firewall is blocking the VPN server protocol traffic.
* Your VPN client certificate is expired.
* The Certificate Revocation List (CRL) is outdated.

Follow these steps to resolve this issue:
{: tsResolve}

1. Check that the security group that is attached to the VPN server allows VPN server protocol traffic.
1. Check that the ACL on the subnet of the VPN server allows VPN server protocol traffic.
1. Check with your ISP to allow VPN server protocol traffic.
1. Check the client certificate expiration date. On Linux, you can run the following command to see the expiration date:

   `openssl x509 -enddate -noout -in client.crt`

1. An expired CRL can also lead to connection issues. Updating and replacing the CRL might resolve the problem. For more information, see [client certificate revocation lists](/docs/vpc?topic=vpc-client-to-site-vpn-planning#client-certificate-revocation-lists).
