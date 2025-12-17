---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see an auth-failure error with user ID and passcode authentication?
{: #troubleshoot-auth-failure-error}
{: troubleshoot}
{: support}

The VPN server uses your IAM username and passcode, not your IAM password to authenticate your VPN client. To generate a passcode, go to [https://iam.cloud.ibm.com/identity/passcode](https://iam.cloud.ibm.com/identity/passcode){: external}. The passcode is a Time-based One-time passcode (TOTP) and cannot be reused. You must have permission to access the VPN server. Ask your VPN server administrator to check that you were granted permissions to perform the action `is.vpn-server.vpn-server.connect`.
{: shortdesc}

The OpenVPN connect V2 shows the error `auth-failure`.
{: tsSymptoms}

Your username or passcode is wrong, or you do not have permission to access the VPN server.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Go to [https://iam.cloud.ibm.com/identity/passcode](https://iam.cloud.ibm.com/identity/passcode){: external} and generate a new passcode.
1. Try the new passcode with your IAM username.
1. If you still see this error, ask the VPN server administrator to confirm that you have permission to access the VPN server.
