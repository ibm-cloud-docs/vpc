---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-07"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why isn't the DNS resolution request sent to the DNS server specified on the VPN server?
{: #troubleshoot-dns-request}
{: troubleshoot}
{: support}

I configured the DNS server IP addresses when I provisioned the VPN server; however, the DNS resolution request was not sent to the DNS server.
{: shortdesc}

The DNS resolution request is sent to the original DNS server.
{: tsSymptoms}

For Linux, the OpenVPN command-line client can receive the DNS option from the VPN server, but the OpenVPN command-line client expects an external command to act on this information. No such commands are configured by default. You must specify these commands with the `up` and `down` config options. For more information, see [OpenVPN DNS](https://wiki.archlinux.org/title/OpenVPN#DNS).
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Option 1: Use [OpenVPN Connect V2 or V3](https://openvpn.net/client/) instead of the OpenVPN command-line client.
1. Option 2: Use a custom script recommended in [OpenVPN DNS](https://wiki.archlinux.org/title/OpenVPN#DNS).
