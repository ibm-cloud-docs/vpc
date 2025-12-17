---

copyright:
  years: 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot
---

{{site.data.keyword.attribute-definition-list}}

# Why can't I connect to websites through my client VPN?
{: #troubleshoot-not-access-with-client-vpn}
{: troubleshoot}
{: support}

Users can't access websites through their client VPN from the office network.
{: shortdesc}

The VPN connection fails to establish or disconnects shortly after the connection was established.
{: tsSymptoms}

The issue is caused by packet loss due to large packet sizes.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Add `tun-mtu 1320` to your OpenVPN client configuration files.
1. If that doesn't work, add `fragment 1400; mssfix` or `mssfix 1360` to the configuration file.
