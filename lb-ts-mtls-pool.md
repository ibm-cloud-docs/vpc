---
copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: troubleshoot, mtls, mutual tls, backend authentication, pool, server verification, certificate error

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't the load balancer connect to back-end servers after enabling authentication?
{: #alb-mtls-troubleshoot-pool}
{: troubleshoot}
{: support}

After enabling server authentication or client authentication at the pool level, the load balancer cannot establish connections to back-end servers.
{: shortdesc}

Back-end servers appear unhealthy in the pool, health checks fail, or traffic cannot reach back-end servers.
{: tsSymptoms}

Pool-level authentication failures can occur for several reasons:
{: tsCauses}

- Back-end server certificates are not valid or not signed by the configured CA
- Back-end servers do not trust the client certificate presented by the load balancer
- Certificates have expired or are not yet valid
- The CA certificate is missing intermediate certificates
- Certificate format is incorrect (not PEM format)
- Health check connections cannot validate back-end server certificates

Follow these steps to resolve pool authentication issues:
{: tsResolve}

1. Verify that back-end server certificates are valid and properly signed:

   ```bash
   openssl s_client -connect backend-server:443 -showcerts
   ```
   {: pre}

   Review the certificate chain returned by the back-end server. If using self-signed certificates, ensure that the CA certificate uploaded to {{site.data.keyword.secrets-manager_short}} includes the complete certificate chain.

1. Check the validity period of back-end server certificates:

   ```bash
   echo | openssl s_client -connect backend-server:443 2>/dev/null | openssl x509 -noout -dates
   ```
   {: pre}

   Ensure that the current date falls within the certificate's validity period. Verify that system time is correct on both the load balancer and back-end servers.

1. If client authentication is enabled, verify that the back-end server trusts the client certificate:

   ```bash
   openssl s_client -connect backend-server:443 -cert client-cert.pem -key client-key.pem
   ```
   {: pre}

   Check back-end server logs for certificate validation errors. Ensure that the client certificate presented by the load balancer is signed by a CA that the back-end server trusts.

1. Verify that all certificates are in PEM format:

   ```bash
   openssl x509 -in certificate.pem -text -noout
   ```
   {: pre}

   If this command fails, the certificate might not be in PEM format. Convert certificates to PEM format if necessary. The load balancer does not support other formats such as DER or PKCS12.

1. Check that the CA certificate includes all intermediate certificates:

   ```bash
   openssl crl2pkcs7 -nocrl -certfile ca-cert.pem | openssl pkcs7 -print_certs -noout
   ```
   {: pre}

   If the certificate chain includes intermediate certificates, ensure that they are all included in the CA certificate bundle uploaded to {{site.data.keyword.secrets-manager_short}}.

1. Verify the pool configuration:

   ```bash
   ibmcloud is load-balancer-pool LOAD_BALANCER POOL --output JSON
   ```
   {: pre}

   Review the `server_authentication` and `client_authentication` settings to ensure they are configured correctly.

1. Check back-end server health:

   ```bash
   ibmcloud is load-balancer-pool-members LOAD_BALANCER POOL
   ```
   {: pre}

   If members show as unhealthy, review the health check configuration. Health check connections must also be able to validate back-end server certificates if server verification is enabled.

1. Review load balancer logs for TLS handshake errors:
   - Navigate to {{site.data.keyword.la_full_notm}}
   - Filter logs for your load balancer
   - Look for SSL/TLS handshake errors or certificate validation failures

1. If using multiple back-end servers with different CAs, verify that the CA bundle includes all necessary certificates:

   ```bash
   openssl crl2pkcs7 -nocrl -certfile ca-bundle.pem | openssl pkcs7 -print_certs -text
   ```
   {: pre}

   When back-end servers in the same pool are signed by different Certificate Authorities, provide a bundled CA file containing all relevant root and intermediate certificates.

1. Test back-end connectivity directly:

   ```bash
   curl -v --cacert ca-cert.pem https://backend-server:443
   ```
   {: pre}

   If this fails, the issue is with the back-end server certificate or CA configuration, not the load balancer.

If health checks fail after enabling server verification, consider these options:
- Use a separate health check port that doesn't require TLS.
- Ensure health check connections can validate back-end server certificates.
- Verify that the health check protocol and port are configured correctly.

If the issue persists after following these steps, contact [IBM Support](/docs/support?topic=support-using-avatar) with the following information:
- Load balancer ID
- Pool ID
- Back-end server details
- Certificate details (without private keys)
- Error messages from connection attempts
- Health check status and configuration
- Output from the verification commands above
