---

copyright:
  years: 2021, 2023
lastupdated: "2023-01-31"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# What do I do if my connection fails?
{: #troubleshoot-connection-failure}
{: troubleshoot}

Client VPN for VPC requires accurate configuration to complete any connection. Changes to the configuration can disrupt your connection, causing a failure.

The VPN connection is failing.
{: tsSymptoms}

You changed your configuration, and the connection cannot complete.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. If you are using client-based authentication, verify that the client certificate information is added to the download client configuration file.

   For example, if you update the authentication method from certificate only to both certificate and user ID + passcode authentication, you must download the client profile again and reconfigure the client certificates.

1. Check the security group configuration and verify that the VPN ports and protocols are added to the inbound and outbound rules.
