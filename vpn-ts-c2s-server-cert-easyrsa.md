---

copyright:
  years: 2024, 2026
lastupdated: "2026-04-21"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see Easy‑RSA file errors when generating a client‑to‑site VPN server certificate?
{: #troubleshoot-c2s-server-cert-easyrsa}
{: troubleshoot}
{: support}

The VPN server certificate generation fails when Easy‑RSA is not properly initialized or its required internal files are missing. Reinitializing Easy‑RSA or creating the certificate by using IBM Cloud Secrets Manager resolves the issue.
{: shortdesc}

When you generate a new server certificate for an existing client‑to‑site VPN server, the certificate creation fails and displays the following Easy‑RSA error:
{: tsSymptoms}

   ```sh
      Easy-RSA error:
      sign_req - Randomize Serial number failed

      system library:fopen: No such file or directory
      BIO routines:BIO_new_file:no such file
   ```
These errors typically indicate that Easy‑RSA cannot access required internal files (such as the certificate index file) in its public key infrastructure (PKI) directory. As a result, Easy‑RSA is unable to sign the certificate request, and the server certificate is not generated.
{: tsCauses}

Follow these steps to resolve the issue:
{: tsResolve}

1. Download or clone the Easy‑RSA repository again and extract it into a new, clean directory.
1. Change to the Easy‑RSA directory and initialize the PKI by running the following command:

   ```sh
      ./easyrsa init-pki
   ```
1. Create or rebuild the certificate authority before you generate the VPN server certificate:

   ```sh
      ./easyrsa build-ca nopass
   ```

1. After successful initialization, generate the VPN server certificate again.
1. If the issue persists, create a new private certificate by using IBM Cloud Secrets Manager and associate it with the VPN server instead of generating the certificate locally with Easy‑RSA. For more information, see [Setting up client-to-site authentication](/docs/vpc?topic=vpc-client-to-site-authentication).
