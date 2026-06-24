---
copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: mtls, mutual tls, client authentication, certificate verification, load balancer security, application load balancer

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About mutual TLS authentication for application load balancers
{: #alb-mtls-about}

Mutual Transport Layer Security (mTLS) authentication provides enhanced security for your {{site.data.keyword.cloud}} application load balancer (ALB) by enabling certificate-based authentication between clients and the load balancer, and between the load balancer and back-end servers.
{: shortdesc}

## What is mutual TLS?
{: #mtls-overview}

mTLS is an extension of the standard TLS protocol that requires both parties in a connection to authenticate each other using digital certificates. While standard TLS only requires the server to present a certificate to the client, mTLS requires both the client and server to present certificates, creating a two-way authentication mechanism.

With mTLS support for ALBs, you can implement certificate-based authentication at two levels:

Front-end authentication
:   Verify client identities by requiring clients to present valid certificates when connecting to the load balancer listener.

Back-end authentication
:   Authenticate the load balancer to back-end servers by presenting a client certificate, and optionally verify back-end server certificates.

## Key capabilities
{: #mtls-capabilities}

ALB mTLS support provides the following capabilities:

Client certificate verification at the listener
:   Enable client authentication at the load balancer front end by configuring a Certificate Authority (CA) certificate to verify client certificates. Verification ensures that only clients with valid certificates signed by the trusted CA can establish connections.

Certificate Revocation List (CRL) support
:   Upload a CRL to check whether client certificates have been revoked, providing an additional layer of security by rejecting compromised or expired certificates.

Back-end server certificate verification
:   Validate back-end server certificates during TLS handshakes to ensure that the load balancer connects only to trusted back-end servers. Validation helps prevent man-in-the-middle attacks.

Back-end client authentication
:   Present a client certificate from the load balancer to back-end servers when mTLS is required by the back-end infrastructure, enabling secure two-way authentication.

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

## Next steps
{: #mtls-next-steps}

- [Configuring client authentication at the listener level](/docs/vpc?topic=vpc-alb-mtls-listener)
- [Configuring server authentication and client certificates at the pool level](/docs/vpc?topic=vpc-alb-mtls-pool)
