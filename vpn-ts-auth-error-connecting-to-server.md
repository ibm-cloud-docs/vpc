---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I get an authentication error "User authentication failed" when I try connecting to the VPN server?
{: #troubleshooting-authentication-error}
{: troubleshoot}

The VPN connection attempts result in a `User authentication failed` error message.
{: tsSymptoms}

The authentication credentials do not have the correct access group or roles assigned.
{: tsCauses}

For user ID and password-based authentication, follow these steps to resolve this issue:
{: tsResolve}

1. Verify that the client that's being connected is added to the correct access group with sufficient access policies to the specific VPN server. See [Creating an access group](/docs/vpc?topic=vpc-create-iam-access-group) for more details.
1. Verify that the **Users of the VPN Server need this role to connect to the VPN server** checkbox is selected in the Service access section.
