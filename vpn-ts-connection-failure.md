---

copyright:
  years: 2021
lastupdated: "2021-06-28"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:note: .note}
{:tip: .tip}
{:important: .important}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# What do I do if my connection fails?
{: #troubleshoot-connection-failure}
{: troubleshoot}

Client VPN for VPC requires accurate configuration to complete any connection. Changes to the configuration can disrupt your connection, causing a failure.

The VPN connection is failing.
{: tsSymptoms}

You made changes to your configuration, and the connection cannot complete.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. If you are using client-based authentication, verify that the client certificate information is added to the download client configuration file.

   For example, if you update the authentication method from certificate only to both certificate and user ID + password authentication, you must download the client profile again and reconfigure the client certificates.
   
1. Check the security group configuration and verify that the VPN ports and protocols are added to the inbound and outbound rules.
