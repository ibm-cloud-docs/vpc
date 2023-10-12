---

copyright:
  years: 2021, 2023
lastupdated: "2023-01-31"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my VPN client hang when I initiate a VPN session?
{: #troubleshoot-vpn-client-hangs}
{: troubleshoot}
{: support}

The VPN server runs on the protocol and port that is specified when the server is provisioned. If a firewall is blocking traffic from your VPN client to the VPN server, your network administrator must open the protocol and port. For example, if your internet service provider (ISP) is blocking traffic, you can ask your VPN server administrator to change the protocol and port to an unblocked protocol and port. Typically, the TCP 443 port is not blocked by ISPs. Another root cause of a client not connecting to the VPN server is that the client certificate expired.
{: shortdesc}

The VPN client logs the following error, `TLS Error: TLS key negotiation failed to occur within 60 seconds (check your network connectivity)`.
{: tsSymptoms}

The VPN server protocol traffic is blocked by a firewall, or your VPN client certificate expired.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Check that the security group that is attached to the VPN server allows VPN server protocol traffic.
2. Check that the ACL on the subnet of the VPN server allows VPN server protocol traffic.
3. Check with your ISP to allow VPN server protocol traffic.
4. Check the client certificate expiration date. On Linux, you can run the following command to see the expiration date:

   `openssl x509 -enddate -noout -in client.crt`
