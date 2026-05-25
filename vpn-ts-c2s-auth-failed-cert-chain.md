---

copyright:
  years: 2024, 2026
lastupdated: "2026-05-25"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my client-to-site VPN fail with 'AUTH_FAILED' even though the server is reachable?
{: #troubleshoot-c2s-auth-failed-cert-chain}
{: troubleshoot}
{: support}

The `AUTH_FAILED` error occurs when the VPN client cannot validate the VPN server’s certificate chain due to a missing or incorrect intermediate certificate authority (CA) in the client VPN profile. Updating the VPN client profile to include the correct intermediate CA that matches the server certificate issuer resolves the issue.
{: shortdesc}

When you connect to your client‑to‑site VPN server, the VPN client is able to reach the VPN hostname and begin the TLS handshake, but the connection fails shortly afterward. The VPN server appears healthy and no server‑side errors are reported. However, the client VPN logs show the following messages:
{: tsSymptoms}

   ```sh
   SESSION is ACTIVE
   Sending PUSH_REQUEST to server...
   AUTH_FAILED
   EVENT: AUTH_FAILED
   EVENT: DISCONNECTED
   ```
This issue occurs when the VPN server is using a certificate that is issued by a newer intermediate certificate authority, but the VPN client profile still contains an older certificate. In this scenario, the TLS handshake can begin successfully, but the client can't fully validate the server’s certificate chain. As a result, authentication fails and the VPN connection is dropped, even though the server is reachable and the credentials are correct.
{: tsCauses}

Follow these steps to resolve the issue:
{: tsResolve}

1. Identify the certificate that is bound to the VPN server and note the intermediate certificate authority that issued it (for example, a Let's Encrypt R‑series intermediate).
1. On the client system, open the VPN client profile (`.ovpn`) file that is used to connect to the VPN server.
1. Verify the CA certificates that are referenced in the profile and confirm that they include the correct intermediate CA that matches the issuer of the VPN server certificate, and the appropriate root CA.
1. If the profile references an outdated or retired intermediate CA, replace it with the correct intermediate CA obtained from the certificate issuer’s official repository. For more information, see [Managing VPN server and client certificates](/docs/vpc?topic=vpc-client-to-site-authentication&interface=ui#creating-cert-manager-instance-import).
1. Save the updated VPN client profile and reconnect to the VPN server.
1. If the connection is still failing, regenerate the VPN client profile and make sure that the correct intermediate and root CA certificates are included before you retry the connection.

## Related links
{: #related-links-c2s-auth-failed-cert-chain}

* [Why do I see a certificate warning with OpenVPN Connect v2 or v3?](/docs/vpc?topic=vpc-troubleshoot-certificate-warning-openvpn)
* [Why do I see a VPN client restart message when certificate authentication is used?](/docs/vpc?topic=vpc-troubleshoot-client-restart&interface=ui)
* [Why do I see Easy‑RSA file errors when generating a client‑to‑site VPN server certificate?](/docs/vpc?topic=vpc-troubleshoot-c2s-server-cert-easyrsa)
* [Why do I see a certificate not found error when I connect to a client-to-site VPN?](/docs/vpc?topic=vpc-troubleshoot-c2s-certificate-not-found&interface=cli)
