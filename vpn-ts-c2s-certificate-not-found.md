---

copyright:
  years: 2024, 2026
lastupdated: "2026-05-25"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see a "certificate not found" error when I provision my client-to-site VPN?
{: #troubleshoot-c2s-certificate-not-found}
{: troubleshoot}
{: support}

The `vpn_server_certificate_not_found` error occurs due to missing service‑to‑service authorization, even though the certificate exists or an incorrect certificate CRN provided during provisioning.
{: shortdesc}

When you create a client‑to‑site VPN server by using API, CLI, or Terraform, the VPN creation fails and returns the following error:
{: tsSymptoms}

   ```sh
   "code": "vpn_server_certificate_not_found",

   "message": "The certificate could not be found. Please try again with a correct certificate CRN."
   ```

This issue occurs when the VPN service requires explicit authorization to read certificates from Secrets Manager. If this authorization is missing, Secrets Manager blocks access, and the VPN service reports the certificate as not found. Similarly, if an invalid or incorrect certificate CRN is specified, the VPN service cannot locate the certificate and returns the same error.
{: tsCauses}

Follow these steps to resolve the issue:
{: tsResolve}

1. Verify that the certificate CRN is correct and that the certificate exists in IBM Cloud Secrets Manager.
1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Create an IAM service-to-service authorization. For more information, see [Creating an IAM service-to-service authorization](/docs/vpc?topic=vpc-client-to-site-authentication&interface=ui#creating-iam-service-to-service).
1. After you create the authorization, the VPN service can access the certificate in Secrets Manager, and the VPN provisioning completes successfully.

## Related links
{: #related-links-certificate-not-found}

* [Why do I see a certificate warning with OpenVPN Connect v2 or v3?](/docs/vpc?topic=vpc-troubleshoot-certificate-warning-openvpn)
* [Why do I see a VPN client restart message when certificate authentication is used?](/docs/vpc?topic=vpc-troubleshoot-client-restart&interface=ui)
* [Why do I see Easy‑RSA file errors when generating a client‑to‑site VPN server certificate?](/docs/vpc?topic=vpc-troubleshoot-c2s-server-cert-easyrsa)
