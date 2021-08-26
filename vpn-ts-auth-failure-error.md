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

# Why do I see an auth-failure error with user ID and passcode authentication?
{: #troubleshoot-auth-failure-error}
{: troubleshoot}
{: support}

The VPN server uses your IAM username and passcode, not your IAM password to authentication your VPN client. To generate a passcode, you must go to `https://iam.cloud.ibm.com/identity/passcode`. The passcode is a Time-based One-time passcode (TOTP) and cannot be reused. You must have permission to access the VPN server. Ask your VPN server administrator to check that you have been granted the action `is.vpn-server.vpn-server.connect`.
{: shortdesc}

The OpenVPN connect V2 shows the error `auth-failure`.
{: tsSymptoms}

Your username/passcode is wrong, or you do not have permission to access the VPN server.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Go to `https://iam.cloud.ibm.com/identity/passcode` and generate a new passcode.
1. Try the new passcode with your IAM username.
1. If you still see this error, check with the VPN server administrator to ensure that you have permission to access the VPN server.
