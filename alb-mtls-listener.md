---
copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: mtls, mutual tls, client authentication, listener, certificate verification, crl, certificate revocation list

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring mutual TLS client authentication at the listener level
{: #alb-mtls-listener}

Configure mutual TLS (mTLS) authentication at the listener level to verify client identities by requiring clients to present valid certificates when connecting to your application load balancer.
{: shortdesc}

## Before you begin
{: #alb-mtls-listener-prereqs}

Before you configure client authentication at the listener level, ensure that you have:

- An Application Load Balancer (ALB) with a profile that supports mTLS (check the `mtls_supported` property)
- A listener configured with the HTTPS protocol
- A valid CA certificate in PEM format stored in {{site.data.keyword.secrets-manager_short}} to verify client certificates
- (Optional) A Certificate Revocation List (CRL) in PEM format if you want to check for revoked certificates
- The appropriate IAM permissions to manage load balancers and access certificates in {{site.data.keyword.secrets-manager_short}}

## Understanding listener-level client authentication
{: #alb-mtls-listener-overview}

When you enable client authentication at the listener level, the load balancer requires clients to present a valid certificate during the TLS handshake. The load balancer verifies the client certificate against the configured Certificate Authority (CA) certificate and checks it against a optional Certificate Revocation List (CRL).

### Client mTLS authentication configuration
{: #alb-mtls-listener-config}

Client authentication at the listener level consists of two components:

CA certificate
:   A CA certificate that the load balancer uses to verify client certificates. The CA certificate must include the complete certificate chain (root and intermediate certificates) needed to validate client certificates.

CRL
:   An optional list of revoked certificates. If provided, the load balancer checks whether the client certificate has been revoked before accepting the connection.

## Configuring mTLS client authentication in the console
{: #alb-mtls-listener-ui}
{: ui}

To configure client authentication for a listener in the {{site.data.keyword.cloud_notm}} console:

1. Navigate to the [Load balancers for VPC](https://cloud.ibm.com/vpc-ext/network/loadBalancers){: external} page.
2. Click the name of your Application Load Balancer.
3. Click the **Front-end listeners** tab.
4. For an existing listener, click the **Actions** menu ![Actions menu](../icons/action-menu-icon.svg "Actions") and select **Edit**. To create a new listener, click **Create**.
5. In the listener configuration:
   - Ensure that **Protocol** is set to **HTTPS**.
   - In the **SSL certificate** section, select your server certificate from {{site.data.keyword.secrets-manager_short}}.
6. In the **Client authentication** section:
   - Select **Enable client authentication**.
   - For **Certificate authority**, select the CA certificate from {{site.data.keyword.secrets-manager_short}} that will be used to verify client certificates.
   - (Optional) For **Certificate revocation list**, enter the CRL content in PEM format or upload a CRL file.
7. Click **Save** or **Create**.

The listener now requires clients to present valid certificates signed by the configured CA.

## Configuring mTLS client authentication from the CLI
{: #alb-mtls-listener-cli}
{: cli}

### Creating a listener with mTLS client authentication
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

### Updating a listener to enable mTLS client authentication
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

### Disabling mTLS client authentication
{: #alb-mtls-listener-cli-disable}

To disable client authentication for a listener:

```sh
ibmcloud is load-balancer-listener-update LOAD_BALANCER LISTENER_ID --reset-client-auth
```
{: pre}

## Configuring mTLS client authentication with the API
{: #alb-mtls-listener-api}
{: api}

### Creating a listener with mTLS client authentication
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

### Updating a listener to enable mTLS client authentication
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

### Disabling mTLS client authentication
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

## Configuring mTLS client authentication with Terraform
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

## Verifying mTLS client authentication configuration
{: #alb-mtls-listener-verify}

After configuring client authentication, verify that it's working correctly:

1. Test with a valid client certificate:
   ```
   curl --cert client-cert.pem --key client-key.pem https://your-load-balancer-hostname
   ```
   {: pre}

   This request should succeed if the client certificate is valid and signed by the configured CA.

1. Test without a client certificate:
   ```
   curl https://your-load-balancer-hostname
   ```
   {: pre}

   This request should fail with an SSL handshake error, as the load balancer requires a client certificate.

1. Test with a revoked certificate (if CRL is configured):
   ```
   curl --cert revoked-cert.pem --key revoked-key.pem https://your-load-balancer-hostname
   ```
   {: pre}

   This request should fail, as the certificate is in the revocation list.

## Updating the mTLS Certificate Revocation List
{: #alb-mtls-listener-update-crl}

To update the Certificate Revocation List for an existing listener:

1. Obtain the updated CRL from your Certificate Authority.
1. Update the listener configuration with the new CRL content using the CLI, API, or UI.
1. The load balancer applies the new CRL immediately for subsequent connections.

## Next steps
{: #alb-mtls-listener-next-steps}

- [Configuring mutual TLS server authentication and client certificates at the pool level](/docs/vpc?topic=vpc-alb-mtls-pool)
- [Managing certificates in {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager)
