---
copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: troubleshoot, mtls, mutual tls, client authentication, listener, certificate error, handshake failure

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't clients connect after enabling client authentication?
{: #alb-mtls-troubleshoot-listener}
{: troubleshoot}
{: support}

After enabling client authentication at the listener level, clients cannot establish connections to your application load balancer.
{: shortdesc}

Clients receive SSL handshake errors or connection failures when attempting to connect to the load balancer.
{: tsSymptoms}

Client authentication failures can occur for several reasons:
{: tsCauses}

- The client certificate is not signed by the configured Certificate Authority (CA)
- The client certificate has expired or is not yet valid
- The client certificate appears in the Certificate Revocation List (CRL)
- The certificate format is incorrect (not PEM format)
- The CA certificate is missing intermediate certificates

Follow these steps to resolve client authentication issues:
{: tsResolve}

1. Verify that the client certificate is signed by the correct CA:

   ```bash
   openssl verify -CAfile ca-cert.pem client-cert.pem
   ```
   {: pre}

   If verification fails, ensure that the client certificate is signed by the CA certificate configured in the listener. Check the certificate chain to ensure all intermediate certificates are included.

1. Check the validity period of the client certificate:

   ```bash
   openssl x509 -in client-cert.pem -noout -dates
   ```
   {: pre}

   Ensure that the current date falls within the certificate's validity period. Also verify that the system time on client machines is correct.

1. If you configured a CRL, verify that the client certificate has not been revoked:

   ```bash
   openssl crl -in crl.pem -noout -text | grep -A 1 "Serial Number"
   ```
   {: pre}

   Compare the serial number of the client certificate with the serial numbers in the CRL. If the certificate is revoked, you need to obtain a new certificate.

1. Verify that the certificate is in PEM format:

   ```bash
   openssl x509 -in client-cert.pem -text -noout
   ```
   {: pre}

   If this command fails, the certificate might not be in PEM format. Convert the certificate to PEM format if necessary. The load balancer does not support other formats such as DER or PKCS12.

1. Check that the CA certificate includes all intermediate certificates:

   ```bash
   openssl crl2pkcs7 -nocrl -certfile ca-cert.pem | openssl pkcs7 -print_certs -noout
   ```
   {: pre}

   If the CA certificate chain includes intermediate certificates, ensure that they are all included in the CA certificate uploaded to {{site.data.keyword.secrets-manager_short}}.

1. Test the connection with verbose output to see detailed error messages:

   ```bash
   curl -v --cert client-cert.pem --key client-key.pem https://your-load-balancer-hostname
   ```
   {: pre}

   Review the SSL handshake details in the output to identify the specific error.

1. Verify that the CA certificate in {{site.data.keyword.secrets-manager_short}} matches the CA that signed the client certificate:
   - Log in to {{site.data.keyword.secrets-manager_short}}
   - Locate the CA certificate used in the listener configuration
   - Download and compare it with the CA that signed your client certificate

If the issue persists after following these steps, contact IBM Support with the following information:
- Load balancer ID
- Listener ID
- Client certificate details (without private key)
- Error messages from connection attempts
- Output from the verification commands above
