---
copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: mtls, mutual tls, client authentication, listener, certificate verification, crl, certificate revocation list

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring client authentication at the listener level
{: #alb-mtls-listener}

Mutual Transport Layer Security (mTLS) authentication provides enhanced security for your {{site.data.keyword.cloud}} application load balancer (ALB) by enabling certificate-based authentication between clients and the load balancer, and between the load balancer and back-end servers.
{: shortdesc}

## Before you begin
{: #alb-mtls-listener-prereqs}

Before you configure client authentication at the listener level, ensure that you have:

- An Application Load Balancer (ALB) with a profile that supports mTLS (check the `mtls_supported` property)
- A listener configured with the HTTPS protocol
- A valid CA certificate in PEM format stored in {{site.data.keyword.secrets-manager_short}} to verify client certificates
- (Optional) A Certificate Revocation List (CRL) in PEM format if you want to check for revoked certificates
- The appropriate IAM permissions to manage load balancers and access certificates in {{site.data.keyword.secrets-manager_short}}

With mTLS support for ALBs, you can implement certificate-based authentication at two levels:

Front-end authentication
:   Verify client identities by requiring clients to present valid certificates when connecting to the load balancer listener.

Listener-level authentication
:   When you enable client authentication at the listener level, the load balancer requires clients to present a valid certificate during the TLS handshake. The load balancer verifies the client certificate against the configured Certificate Authority (CA) certificate and checks it against an optional Certificate Revocation List (CRL).

### Client authentication configuration
{: #alb-mtls-listener-config}

ALB mTLS support provides the following capabilities:

Client certificate verification at the listener
:   Enable client authentication at the load balancer front end by configuring a Certificate Authority (CA) certificate to verify client certificates. Verification ensures that only clients with valid certificates signed by the trusted CA can establish connections.

CRL
:   An optional list of revoked certificates. If provided, the load balancer checks whether the client certificate has been revoked before accepting the connection.

## Configuring client authentication in the console
{: #alb-mtls-listener-ui}
{: ui}

To configure client authentication for a listener in the {{site.data.keyword.cloud_notm}} console:

## Use cases
{: #mtls-use-cases}

mTLS authentication is valuable in scenarios where you need enhanced security and identity verification:

Zero-trust security architectures
:   Implement zero-trust principles by requiring certificate-based authentication for all connections, ensuring that every client and server is verified before communication is established.

API security
:   Protect API endpoints by requiring clients to present valid certificates, preventing unauthorized access to sensitive APIs and ensuring that only authenticated applications can consume your services.

Microservices communication
:   Secure communication between microservices by implementing mTLS at both the front-end and back-end, ensuring that all service-to-service communication is authenticated and encrypted.

Compliance requirements
:   Meet regulatory compliance requirements that mandate strong authentication mechanisms, such as those in financial services, healthcare, or government sectors.

Back-end server validation
:   Ensure that your load balancer connects only to legitimate back-end servers by verifying their certificates, protecting against rogue or compromised back-end instances.

## How mTLS works with application load balancers
{: #mtls-how-it-works}

ALBs support mTLS for both client-to-load-balancer and load-balancer-to-server connections. The following workflows show how certificate validation occurs during each TLS handshake.

### Front-end mTLS (listener level)
{: #mtls-frontend}

When you enable client authentication (mTLS) at the listener level:

1. A client initiates an HTTPS connection to the load balancer.
1. The load balancer presents its server certificate to the client.
1. The load balancer requests a client certificate from the client.
1. The client presents its certificate to the load balancer.
1. The load balancer verifies the client certificate against the configured CA certificate.
1. If a CRL is configured, the load balancer checks whether the certificate has been revoked.
1. If verification succeeds, the connection is established. Otherwise, the connection is rejected.

### Back-end mTLS (pool level)
{: #mtls-backend}

When you enable server authentication or client certificate presentation at the pool level:

1. The load balancer initiates a connection to a back-end server.
2. The back-end server presents its certificate to the load balancer.
3. If server verification is enabled, the load balancer validates the back-end server certificate against the configured CA certificate.
4. If the back-end server requests client authentication, the load balancer presents its client certificate.
5. The back-end server verifies the client certificate.
6. If verification succeeds, the connection is established and traffic flows to the back-end server.

## Certificate Revocation List (CRL) support
{: #mtls-crl}

A CRL is a list of certificates that have been revoked by the CA before their expiration date. Certificates might be revoked for various reasons, including:

- The private key has been compromised
- The certificate was issued incorrectly
- The certificate holder's privileges have changed
- The certificate is no longer needed

When you configure a CRL for your listener, the load balancer checks each client certificate against the revocation list during the TLS handshake. If a certificate appears in the CRL, the connection is rejected, even if the certificate is otherwise valid and properly signed.

CRL support provides an additional security layer by ensuring that compromised or invalidated certificates cannot be used to access your services, even if they haven't yet expired.

## Prerequisites
{: #mtls-prerequisites}

Before you configure mTLS for your ALB, ensure that you meet the following requirements:

- You have an ALB with a profile that supports mTLS. Check the `mtls_supported` property in the load balancer profile.
- Your listener uses the HTTPS protocol. mTLS is available only for HTTPS listeners.
- You have valid certificates in PEM format stored in {{site.data.keyword.secrets-manager_short}}.
- You have the appropriate IAM permissions to manage load balancers and access certificates in {{site.data.keyword.secrets-manager_short}}.

### Certificate requirements
{: #mtls-certificate-requirements}

All certificates used for mTLS must meet the following requirements:

- Certificates must be in PEM format.
- Certificates must be stored in {{site.data.keyword.secrets-manager_short}} and referenced by their CRN.
- CA certificates used for verification must include the complete certificate chain (root and intermediate certificates).
- Client certificates presented by the load balancer to back-end servers must include the private key.
- Certificates must be valid (not expired) and properly signed by a trusted CA.

## Important considerations
{: #mtls-considerations}

Keep the following considerations in mind when implementing mTLS:

Certificate management responsibility
:   You are responsible for managing all certificates, including obtaining, uploading, renewing, and revoking certificates. {{site.data.keyword.cloud_notm}} does not automatically manage or renew certificates.

Certificate validation
:   You must verify that certificates are properly signed by the intended Certificate Authorities and that trust relationships are correctly established. Invalid or mismatched certificates cause TLS handshake failures and service unavailability.

Back-end server verification
:   Back-end server verification is disabled by default to maintain backward compatibility. When enabled, you must ensure that a valid trust relationship exists between the load balancer and back-end servers by uploading the appropriate CA certificate.

Pool-level configuration
:   Both back-end server verification and client authentication are configured at the pool level. All back-end servers within a pool use the same configuration. If you need different certificate policies for different back-end servers, create separate pools.

Certificate updates
:   Certificate updates or revocations might require a reload of the load balancing service to take effect. Plan certificate updates during maintenance windows to minimize service disruption.

Multiple CAs
:   When back-end servers in the same pool are signed by different CAs, you can provide a bundled CA file containing all relevant root and intermediate certificates. All back-end servers are trusted if their certificate chains to any CA in the bundle.

1. Navigate to the [Load balancers for VPC](https://cloud.ibm.com/vpc-ext/network/loadBalancers){: external} page.
1. Click the name of your Application Load Balancer.
1. Click the **Front-end listeners** tab.
1. For an existing listener, click the **Actions** menu ![Actions menu](../icons/action-menu-icon.svg "Actions") and select **Edit**. To create a new listener, click **Create**.
1. In the listener configuration:
   - Ensure that **Protocol** is set to **HTTPS**.
   - In the **SSL certificate** section, select your server certificate from {{site.data.keyword.secrets-manager_short}}.
1. In the **Client authentication** section:
   - Select **Enable client authentication**.
   - For **Certificate authority**, select the CA certificate from {{site.data.keyword.secrets-manager_short}} that will be used to verify client certificates.
   - (Optional) For **Certificate revocation list**, enter the CRL content in PEM format or upload a CRL file.
1. Click **Save** or **Create**.

The listener now requires clients to present valid certificates signed by the configured CA.

## Configuring client authentication from the CLI
{: #alb-mtls-listener-cli}
{: cli}

### Creating a listener with client authentication
{: #alb-mtls-listener-cli-create}

To create a listener with client authentication enabled, use the `ibmcloud is load-balancer-listener-create` command:

```sh
ibmcloud is load-balancer-listener-create LOAD_BALANCER \
  --protocol https \
  --port 443 \
  --certificate-instance-crn SERVER_CERT_CRN \
  --client-auth-ca-crn CA_CERT_CRN \
  [--client-auth-crl CRL_CONTENT]
```
{: pre}

Where:

- `LOAD_BALANCER` is the ID or name of your load balancer.
- `SERVER_CERT_CRN` is the CRN of your server certificate in {{site.data.keyword.secrets-manager_short}}.
- `CA_CERT_CRN` is the CRN of the CA certificate used to verify client certificates.
- `CRL_CONTENT` is the optional Certificate Revocation List content in PEM format.

Example:

```sh
ibmcloud is load-balancer-listener-create my-load-balancer \
  --protocol https \
  --port 443 \
  --certificate-instance-crn crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f510 \
  --client-auth-ca-crn crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511
```
{: pre}

### Updating a listener to enable client authentication
{: #alb-mtls-listener-cli-update}

To update an existing listener to enable client authentication, use the `ibmcloud is load-balancer-listener-update` command:

```sh
ibmcloud is load-balancer-listener-update LOAD_BALANCER LISTENER_ID \
  --client-auth-ca-crn CA_CERT_CRN \
  [--client-auth-crl CRL_CONTENT]
```
{: pre}

Example:

```sh
ibmcloud is load-balancer-listener-update my-load-balancer my-listener \
  --client-auth-ca-crn crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511
```
{: pre}

### Disabling client authentication
{: #alb-mtls-listener-cli-disable}

To disable client authentication for a listener:

```sh
ibmcloud is load-balancer-listener-update LOAD_BALANCER LISTENER_ID --reset-client-auth
```
{: pre}

## Configuring client authentication with the API
{: #alb-mtls-listener-api}
{: api}

### Creating a listener with client authentication
{: #alb-mtls-listener-api-create}

To create a listener with client authentication enabled, call the `POST /load_balancers/{load_balancer_id}/listeners` method:

```sh
curl -X POST \
  "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/listeners?version=2026-05-06&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
    "protocol": "https",
    "port": 443,
    "certificate_instance": {
      "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f510"
    },
    "client_authentication": {
      "certificate_authority": {
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511"
      },
      "certificate_revocation_list": "-----BEGIN X509 CRL-----\n...\n-----END X509 CRL-----"
    }
  }'
```
{: pre}

### Updating a listener to enable client authentication
{: #alb-mtls-listener-api-update}

To update an existing listener to enable client authentication, call the `PATCH /load_balancers/{load_balancer_id}/listeners/{id}` method:

```sh
curl -X PATCH \
  "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/listeners/$listener_id?version=2026-05-06&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
    "client_authentication": {
      "certificate_authority": {
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511"
      }
    }
  }'
```
{: pre}

### Disabling client authentication
{: #alb-mtls-listener-api-disable}

To disable client authentication, set the `client_authentication` property to `null`:

```sh
curl -X PATCH \
  "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/listeners/$listener_id?version=2026-05-06&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
    "client_authentication": null
  }'
```
{: pre}

## Configuring client authentication with Terraform
{: #alb-mtls-listener-terraform}
{: terraform}

To configure client authentication for a listener using Terraform, use the `ibm_is_lb_listener` resource with the `client_authentication` block:

```terraform
resource "ibm_is_lb_listener" "example" {
  lb       = ibm_is_lb.example.id
  port     = 443
  protocol = "https"

  certificate_instance = "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f510"

  client_authentication {
    certificate_authority_crn = "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511"
    certificate_revocation_list = file("${path.module}/crl.pem")
  }
}
```
{: codeblock}

## Verifying client authentication configuration
{: #alb-mtls-listener-verify}

After configuring client authentication, verify that it's working correctly:

1. Test with a valid client certificate:

   ```sh
   curl --cert client-cert.pem --key client-key.pem https://your-load-balancer-hostname
   ```
   {: pre}

   This request should succeed if the client certificate is valid and signed by the configured CA.

1. Test without a client certificate:

   ```sh
   curl https://your-load-balancer-hostname
   ```
   {: pre}

   This request should fail with an SSL handshake error, as the load balancer requires a client certificate.

1. Test with a revoked certificate (if CRL is configured):

   ```sh
   curl --cert revoked-cert.pem --key revoked-key.pem https://your-load-balancer-hostname
   ```
   {: pre}

   This request should fail, as the certificate is in the revocation list.

## Updating the Certificate Revocation List
{: #alb-mtls-listener-update-crl}

To update the Certificate Revocation List for an existing listener:

1. Obtain the updated CRL from your Certificate Authority.
1. Update the listener configuration with the new CRL content using the CLI, API, or UI.
1. The load balancer applies the new CRL immediately for subsequent connections.

## Next steps
{: #alb-mtls-listener-next-steps}

- [Configuring mutual TLS authentication at the pool level](/docs/vpc?topic=vpc-alb-mtls-pool)
- [Managing certificates in {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager)
