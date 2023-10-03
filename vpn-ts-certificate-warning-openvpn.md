---

copyright:
  years: 2021, 2023
lastupdated: "2023-01-31"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see a certificate warning with OpenVPN Connect v2 or v3?
{: #troubleshoot-certificate-warning-openvpn}
{: troubleshoot}
{: support}

When the authentication mode is user ID and passcode only, a client certificate is not required. However, this approach is less secure than using both certificate and user ID and passcode authentication.
{: shortdesc}

`OpenVPN Connect V2` displays the error `Missing External PKI alias`.
{: tsSymptoms}

`OpenVPN Connect V3` displays the error `Miss external certificate`.

For v2, this message is a bug. For v3, this message is a warning that you can ignore.
{: tsCauses}

Try the following options to resolve this issue:
{: tsResolve}

1. Select one of the following steps:

   * For OpenVPN Connect v2, edit the VPN client profile by adding a random client private key and certificate, then reimport the client profile into the OpenVPN Connect V2 client.
   * * For OpenVPN Connect v3, select to continue and skip the warning message.
1. Use another VPN client, such as Tunnelblick. For OpenVPN Connect version 2 users, you can upgrade to version 3.
