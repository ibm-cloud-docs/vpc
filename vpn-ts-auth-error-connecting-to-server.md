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

# Why do I get an authentication error "User authentication failed" when connecting to VPN server?
{: troubleshooting-authentication-error}
{: troubleshoot}

The VPN connection attempts result in a `User authentication failed` error message.
{: tsSymptoms}

The authentication credentials do not have the correct access group or roles assigned.
{: tsCauses}

For user ID/password based authentication, follow these steps to resolve this issue:
{: tsResolve}

1. Verify that the client being connected is added to the correct access group with sufficient access policies to the specific VPN server. See [Creating an access group](/docs/vpc?topic=vpc-create-iam-access-group) for more details.
1. Verify that the **Users of the VPN Server need this role to connect to the VPN server** checkbox is selected in the Service access section.
