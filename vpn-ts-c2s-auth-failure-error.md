---

copyright:
  years: 2021, 2026
lastupdated: "2026-04-21"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see an authentication failed error when I connect to my client‑to‑site VPN?
{: #troubleshoot-c2s-auth-failure-error}
{: troubleshoot}
{: support}

The VPN server uses your IAM username and passcode, not your IAM password to authenticate your VPN client. The passcode is a Time-based One-time passcode (TOTP) and cannot be reused. You must have permission to access the VPN server. Ask your VPN server administrator to check whether you have the necessary permissions for the action `is.vpn-server.vpn-server.connect`.
{: shortdesc}

When you connect to your client‑to‑site VPN server, the VPN tunnel and TLS session establish successfully, but the connection fails shortly afterward. The VPN client logs show an authentication failure similar to the following message:
{: tsSymptoms}

   ```sh
      AUTH_FAILED

      SIGUSR1[soft,auth-failure] received, process restarting
   ```

This issue occurs because client‑to‑site VPN authentication expects an IAM passcode in the VPN client password field. If a one‑time password (OTP), MFA code, or other authentication value is entered instead, the VPN server rejects the login credentials even though the tunnel and certificates are valid
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Go to [https://iam.cloud.ibm.com/identity/passcode](https://iam.cloud.ibm.com/identity/passcode){: external} and generate a new passcode.
1. Try the new passcode with your IAM username.
1. If you still see this error, ask the VPN server administrator to confirm that you have permission to access the VPN server.
