---

copyright:
  years: 2021
lastupdated: "2021-06-28"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see a certificate warning with OpenVPN Connect v2 or v3?
{: #troubleshoot-certificate-warning-openvpn}
{: troubleshoot}
{: support}

When the authentication mode is user ID/passcode only, a client certificate is not required. However, this approach is less secure than using both certificate and user ID/passcode authentication.  
{: shortdesc}

`OpenVPN Connect V2` displays the error `Missing External PKI alias`.

`OpenVPN Connect V3` displays the error `Miss external certificate`.
{: tsSymptoms}

For v2, this is a bug. For v3, this is a warning message that you can ignore.
{: tsCauses}

Try the following options to resolve this issue:
{: tsResolve}

1. Select one of the following:

   * For OpenVPN Connect v3, select to continue and skip the warning message.
   * For OpenVPN Connect v2, edit the VPN client profile by adding a random client private key and certificate, then re-import the client profile into the OpenVPN Connect V2 client.
1. Use another VPN client, such as Tunnelblick. For OpenVPN Connect version 2 users, you can upgrade to version 3.
