---
copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: mtls, mutual tls, backend authentication, pool, server verification, client certificate

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring mutual TLS authentication at the pool level
{: #alb-mtls-pool}

Configure mutual TLS (mTLS) authentication at the pool level to verify back-end server certificates and present client certificates when connecting to back-end servers.
{: shortdesc}

## Before you begin
{: #alb-mtls-pool-prereqs}

Before you configure authentication at the pool level, ensure that you have:

- An application load balancer with a profile that supports mTLS (check the `mtls_supported` property)
- A pool configured with the HTTPS protocol
- (Optional) A valid CA certificate in PEM format stored in {{site.data.keyword.secrets-manager_short}} to verify back-end server certificates
- (Optional) A valid client certificate with private key in PEM format stored in {{site.data.keyword.secrets-manager_short}} to present to back-end servers
- The appropriate IAM permissions to manage load balancers and access certificates in {{site.data.keyword.secrets-manager_short}}

## Understanding mTLS pool-level authentication
{: #alb-mtls-pool-overview}

Pool-level authentication provides two distinct capabilities for securing connections between the load balancer and back-end servers:

Server authentication
:   Verify the identity of back-end servers by validating their certificates against a configured CA certificate. This ensures that the load balancer connects only to trusted back-end servers.

Client authentication
:   Present a client certificate from the load balancer to back-end servers when the back-end infrastructure requires mTLS. This allows back-end servers to verify the identity of the load balancer.

These capabilities can be configured independently or together, depending on your security requirements.

### Server mTLS authentication configuration
{: #alb-mtls-pool-server-auth}

Server authentication consists of two components:

Verify certificate
:   A boolean flag that enables or disables back-end server certificate verification. When enabled, the load balancer validates back-end server certificates during the TLS handshake.

Certificate Authority (CA) certificate
:   An optional CA certificate used to verify back-end server certificates. If server verification is enabled but no CA certificate is provided, the load balancer uses the system trust store to validate back-end server certificates.

### Client mTLS authentication configuration
{: #alb-mtls-pool-client-auth}

Client authentication requires:

Client certificate
:   A certificate with private key that the load balancer presents to back-end servers when they request client authentication during the TLS handshake. The certificate must be stored in {{site.data.keyword.secrets-manager_short}}.

## Configuring mTLS pool authentication in the console
{: #alb-mtls-pool-ui}
{: ui}

To configure authentication for a pool in the {{site.data.keyword.cloud_notm}} console:

1. Navigate to the [Load balancers for VPC](https://cloud.ibm.com/vpc-ext/network/loadBalancers){: external} page.
2. Click the name of your Application Load Balancer.
3. Click the **Back-end pools** tab.
4. For an existing pool, click the **Actions** menu ![Actions menu](../icons/action-menu-icon.svg "Actions") and select **Edit**. To create a new pool, click **Create**.
5. In the pool configuration:
   - Ensure that **Protocol** is set to **HTTPS**.
6. In the **Server authentication** section:
   - Select **Verify server certificate** to enable back-end server certificate verification.
   - (Optional) For **Certificate authority**, select the CA certificate from {{site.data.keyword.secrets-manager_short}} that will be used to verify back-end server certificates. If not specified, the system trust store is used.
7. In the **Client authentication** section:
   - For **Client certificate**, select the certificate from {{site.data.keyword.secrets-manager_short}} that the load balancer will present to back-end servers.
8. Click **Save** or **Create**.

## Configuring mTLS pool authentication from the CLI
{: #alb-mtls-pool-cli}
{: cli}

### Creating a pool with mTLS server authentication
{: #alb-mtls-pool-cli-create-server}

To create a pool with back-end server certificate verification enabled, use the `ibmcloud is load-balancer-pool-create` command:

```bash
ibmcloud is load-balancer-pool-create POOL_NAME LOAD_BALANCER ALGORITHM PROTOCOL \
  HEALTH_DELAY HEALTH_RETRIES HEALTH_TIMEOUT HEALTH_TYPE \
  --server-auth-verify-cert true \
  [--server-auth-ca-crn CA_CERT_CRN]
```
{: pre}

Where:

- `POOL_NAME` is the name for your pool.
- `LOAD_BALANCER` is the ID or name of your load balancer.
- `ALGORITHM` is the load balancing algorithm (round_robin, weighted_round_robin, or least_connections).
- `PROTOCOL` must be `https` for mTLS support.
- `CA_CERT_CRN` is the optional CRN of the CA certificate used to verify back-end server certificates.

Example:

```bash
ibmcloud is load-balancer-pool-create my-pool my-load-balancer round_robin https \
  20 2 5 http \
  --server-auth-verify-cert true \
  --server-auth-ca-crn crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511
```
{: pre}

### Creating a pool with mTLS client authentication
{: #alb-mtls-pool-cli-create-client}

To create a pool with client certificate presentation enabled:

```bash
ibmcloud is load-balancer-pool-create POOL_NAME LOAD_BALANCER ALGORITHM PROTOCOL \
  HEALTH_DELAY HEALTH_RETRIES HEALTH_TIMEOUT HEALTH_TYPE \
  --client-auth-cert-crn CLIENT_CERT_CRN
```
{: pre}

Example:

```bash
ibmcloud is load-balancer-pool-create my-pool my-load-balancer round_robin https \
  20 2 5 http \
  --client-auth-cert-crn crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f512
```
{: pre}

### Creating a pool with both server and client mTLS authentication
{: #alb-mtls-pool-cli-create-both}

To create a pool with both server verification and client certificate presentation:

```bash
ibmcloud is load-balancer-pool-create my-pool my-load-balancer round_robin https \
  20 2 5 http \
  --server-auth-verify-cert true \
  --server-auth-ca-crn crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511 \
  --client-auth-cert-crn crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f512
```
{: pre}

### Updating a pool to enable mTLS server authentication
{: #alb-mtls-pool-cli-update-server}

To update an existing pool to enable server authentication:

```bash
ibmcloud is load-balancer-pool-update LOAD_BALANCER POOL \
  --server-auth-verify-cert true \
  [--server-auth-ca-crn CA_CERT_CRN]
```
{: pre}

Example:

```bash
ibmcloud is load-balancer-pool-update my-load-balancer my-pool \
  --server-auth-verify-cert true \
  --server-auth-ca-crn crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511
```
{: pre}

### Updating a pool to enable mTLS client authentication
{: #alb-mtls-pool-cli-update-client}

To update an existing pool to enable client authentication:

```bash
ibmcloud is load-balancer-pool-update LOAD_BALANCER POOL \
  --client-auth-cert-crn CLIENT_CERT_CRN
```
{: pre}

### Disabling mTLS server authentication
{: #alb-mtls-pool-cli-disable-server}

To disable server authentication for a pool:

```bash
ibmcloud is load-balancer-pool-update LOAD_BALANCER POOL --reset-server-auth
```
{: pre}

### Disabling mTLS client authentication
{: #alb-mtls-pool-cli-disable-client}

To disable client authentication for a pool:

```bash
ibmcloud is load-balancer-pool-update LOAD_BALANCER POOL --reset-client-auth
```
{: pre}

## Configuring pool authentication with the API
{: #alb-mtls-pool-api}
{: api}

### Creating a pool with mTLS server and client authentication
{: #alb-mtls-pool-api-create}

To create a pool with both server and client authentication enabled, call the `POST /load_balancers/{load_balancer_id}/pools` method:

```bash
curl -X POST \
  "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/pools?version=2026-05-06&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
    "name": "my-pool",
    "algorithm": "round_robin",
    "protocol": "https",
    "health_monitor": {
      "delay": 20,
      "max_retries": 2,
      "timeout": 5,
      "type": "http"
    },
    "server_authentication": {
      "verify_certificate": true,
      "certificate_authority": {
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511"
      }
    },
    "client_authentication": {
      "certificate_instance": {
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f512"
      }
    }
  }'
```
{: pre}

### Updating a pool to enable mTLS server authentication
{: #alb-mtls-pool-api-update-server}

To update an existing pool to enable server authentication, call the `PATCH /load_balancers/{load_balancer_id}/pools/{id}` method:

```bash
curl -X PATCH \
  "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/pools/$pool_id?version=2026-05-06&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
    "server_authentication": {
      "verify_certificate": true,
      "certificate_authority": {
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511"
      }
    }
  }'
```
{: pre}

### Updating a pool to enable mTLS client authentication
{: #alb-mtls-pool-api-update-client}

To update an existing pool to enable client authentication:

```bash
curl -X PATCH \
  "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/pools/$pool_id?version=2026-05-06&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
    "client_authentication": {
      "certificate_instance": {
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f512"
      }
    }
  }'
```
{: pre}

### Disabling mTLS server authentication
{: #alb-mtls-pool-api-disable-server}

To disable server authentication, set the `server_authentication` property to `null`:

```bash
curl -X PATCH \
  "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/pools/$pool_id?version=2026-05-06&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
    "server_authentication": null
  }'
```
{: pre}

### Disabling mTLS client authentication
{: #alb-mtls-pool-api-disable-client}

To disable client authentication, set the `client_authentication` property to `null`:

```bash
curl -X PATCH \
  "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/pools/$pool_id?version=2026-05-06&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
    "client_authentication": null
  }'
```
{: pre}

## Configuring mTLS pool authentication with Terraform
{: #alb-mtls-pool-terraform}
{: terraform}

To configure authentication for a pool using Terraform, use the `ibm_is_lb_pool` resource with the `server_authentication` and `client_authentication` blocks:

```terraform
resource "ibm_is_lb_pool" "example" {
  lb                 = ibm_is_lb.example.id
  name               = "my-pool"
  algorithm          = "round_robin"
  protocol           = "https"
  health_delay       = 20
  health_retries     = 2
  health_timeout     = 5
  health_type        = "http"

  server_authentication {
    verify_certificate = true
    certificate_authority_crn = "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f511"
  }

  client_authentication {
    certificate_instance_crn = "crn:v1:bluemix:public:secrets-manager:us-south:a/aa5a471f75bc456fac416bf02c4ba6de:aace9348-39da-4498-b132-e5ab918237f4:secret:e3bd96ce-1e4c-f642-d1f2-0d0ab025f512"
  }
}
```
{: codeblock}

## Verifying mTLS pool authentication configuration
{: #alb-mtls-pool-verify}

After configuring pool authentication, verify that it's working correctly:

1. Check the pool status to ensure back-end servers are healthy:
   ```bash
   ibmcloud is load-balancer-pool LOAD_BALANCER POOL
   ```
   {: pre}

1. If server verification is enabled, ensure that back-end servers present valid certificates signed by the configured CA.

1. If client authentication is enabled, verify that back-end servers can validate the client certificate presented by the load balancer.

1. Monitor load balancer logs for any TLS handshake errors that might indicate certificate validation issues.

## Important considerations for mTLS pool-level authentication
{: #alb-mtls-pool-considerations}

Pool-level configuration applies to all back-end servers
:   Both server authentication and client authentication are configured at the pool level. All back-end servers within the pool use the same authentication settings. If you need different certificate policies for different back-end servers, create separate pools.

Multiple Certificate Authorities for back-end servers
:   When back-end servers in the same pool are signed by different Certificate Authorities, you can provide a bundled CA file containing all relevant root and intermediate certificates. All back-end servers are trusted if their certificate chains to any CA included in the bundle.

System trust store
:   If server verification is enabled but no CA certificate is provided, the load balancer uses the system trust store (Ubuntu Linux) to validate back-end server certificates. This works for certificates signed by well-known public CAs but not for self-signed certificates or private CAs.

Certificate updates
:   When you update certificates in {{site.data.keyword.secrets-manager_short}}, the load balancer automatically retrieves the updated certificates. However, active connections might need to be re-established to use the new certificates.

## Next steps
{: #alb-mtls-pool-next-steps}

- [About mutual TLS authentication for application load balancers](/docs/vpc?topic=vpc-alb-mtls-about)
- [Configuring mutual TLS client authentication at the listener level](/docs/vpc?topic=vpc-alb-mtls-listener)
- [Managing certificates in {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager)
