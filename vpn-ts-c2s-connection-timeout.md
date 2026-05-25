---

copyright:
  years: 2024, 2026
lastupdated: "2026-05-25"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my client-to-site VPN connection time-out without reaching the server?
{: #troubleshoot-c2s-connection-timeout}
{: troubleshoot}
{: support}

The client‑to‑site VPN connection timeouts can occur when the security group that is attached to the VPN server restricts inbound traffic to specific source IP addresses. When client traffic is blocked at the security group level, connection attempts never reach the VPN server, which results in timeouts on the client side.
{: shortdesc}

When you attempt to connect to your client‑to‑site VPN, the VPN client continuously times out and fails to establish a connection. The VPN server appears healthy and stable, but the client logs show repeated timeout messages.
{: tsSymptoms}

   ```sh
   TCP connection timeout

   TLS handshake failed

   Connection timed out
   ```
This issue occurs when the ACL or security group that attached to the VPN server blocks the client device's IP. If VPN clients attempt to connect from an IP address that is not explicitly allowed, the traffic is dropped before it reaches the VPN server.
{: tsCauses}

Follow these steps to resolve the issue:
{: tsResolve}

1. Identify the security group that is attached to your client‑to‑site VPN server.
1. Review the inbound rules in the security group and confirm that the VPN protocol and port match the VPN server configuration. For example, if the VPN server uses TCP on port `443`, make sure that TCP port `443` is allowed. For more information, see [Configuring security groups and NACLs for use with a VPN server](https://cloud.ibm.com/docs/vpc?topic=vpc-vpn-client-to-site-security-groups).
1. Verify that the source IP addresses or CIDR ranges in the inbound rules include the networks from which VPN clients connect. Avoid restricting access to a single IP address unless all clients originate from that address.
1. Update the security group to allow inbound VPN traffic from the appropriate source ranges. For example, allow traffic from `0.0.0.0/0` or from trusted client CIDR blocks as required by your security policy.
1. Ensure that the all outbound rules are allowed so that the VPN server can respond to client traffic.
1. Save the security group changes and retry the VPN connection from the client.
1. Confirm that connection attempts now appear in the VPN server logs and that the client successfully establishes the VPN connection.

## Related links
{: #related-links-vpn-server-timeout}

[Why does my VPN client hang when I initiate a VPN session?](/docs/vpc?topic=vpc-troubleshoot-vpn-client-hangs&interface=ui)
