---

copyright:
  years: 2022, 2026
lastupdated: "2026-06-23"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Identity operations
{: #imd-identity-operations}

You can use the metadata service to obtain an identity access token from the metadata service, generate an IAM access token, and to create identity certificate. These tokens and certificates can be used to access the metadata services to call IAM-enabled services, and to establish encrypted connections between file shares and virtual server instances.
{: shortdesc}

## Identity access token
{: #imd-json-token}
{: api}

An identity access token provides a security credential for accessing the {{site.data.keyword.cloud}} Metadata and VPC Identity services. It's a signed token with a set of claims based on information about the instance and information that is passed in the token request. The minimum version date to use the identity access token feature is `2022-03-01`.

Communication between the instance and the metadata service stays within the host. You acquire the token from within the instance. If secure access to the metadata service is enabled on your instance, use the "https" protocol instead of the "http" protocol.
{: important}

To obtain the identity token, make a `PUT /identity/v1/token` request to the [Metadata service API](/apidocs/vpc-identity/latest#create-identity-token).

If you currently use the `/instance_identity/v1/token` method and want to adopt the API release version `2025-08-26` or later, review the changes that are described in the migration guidance: [Updating to the `2025-08-26` version of the VPC Identity API](/docs/vpc?topic=vpc-2025-08-26-migration-metadata-identity).
{: note}

```sh
curl -X PUT "https://api.metadata.cloud.ibm.com/identity/v1/token?version=2025-08-26" -H "Metadata-Flavor: ibm" -d '{}'
```
{: pre}

In the request, you can specify an expiration time for the token. The default expiration value is 5 minutes, but you can specify any value between 5 seconds to 1 hour. See the following example for a host that has secure access enabled. In the next example, the expiration time of the token is specified as one hour.

```sh
curl -X PUT "https://api.metadata.cloud.ibm.com/identity/v1/token?version=2025-08-26" -H "Metadata-Flavor: ibm" -d '{"expires_in": 3600}'
```
{: pre}

The API response contains the identity access token. Use this token to access the metadata service.

The following JSON response shows an identity access token character string, date, and time that it was created, date, and time that it expires, and expiration time you set. This token expires in 5 minutes.

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IlZTSS1DUl91cy1lYXN0X2I5...",
  "created_at": "2025-06-10T11:08:39.363Z",
  "expires_at": "2025-06-10T11:13:39.363Z",
  "expires_in": 300
}
```
{: codeblock}

Or you can also use the following command:

```json
identity_token=`curl -X PUT "https://api.metadata.cloud.ibm.com/identity/v1/token?version=2025-08-26"\
  -H "Metadata-Flavor: ibm"\
  -d '{
        "expires_in": 3600
      }' | jq -r '(.access_token)'`
```
{: codeblock}

In the following example, the return value of the cURL command is the identity access token. The token is extracted by `jq` and placed in the `identity_token` environment variable.

```json
identity_token=`curl -X PUT "https://api.metadata.cloud.ibm.com/identity/v1/token?version=2025-08-26"\
  -H "Metadata-Flavor: ibm"\
  -d '{
        "expires_in": 3600
      }' | jq -r '(.access_token)'`
```
{: codeblock}

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create an instance. You might need to install `jq` before use or use another parser of your choice.
{: note}

You can specify `identity_token` variable in a `GET` call to the metadata service to invoke one of the metadata methods. For more information, see [Retrieve metadata from your running instances](/docs/vpc?topic=vpc-imd-access-instance-metadata&interface=api#imd-access-md-use).

You can also generate an IAM token from this identity token and use the VPC API to call IAM-enabled services. For more information, see [Generate an IAM token from an identity access token](/docs/vpc?topic=vpc-imd-identity-operations#imd-token-exchange).

## Generate an IAM token from an identity access token
{: #imd-token-exchange}
{: api}

To access IBM Cloud IAM-enabled services in the account, you can generate an IAM token from the identity access token and a trusted profile. After you generate the IAM token, you can use it to access IAM-enabled services, such as {{site.data.keyword.cos_full_notm}}, Cloud Database Service, and the VPC APIs. You can reuse the token multiple times.

Make a `POST /identity/v1/iam_tokens` request and specify the ID of the trusted profile. This request uses the identity access token and a trusted profile that is linked to a virtual server instance to generate an IAM access token. The trusted profile can be linked either when you create the instance or provided in the request body.


The IAM API used to pass the identity access token and generate an IAM token is being deprecated. Beta users must migrate to the metadata service API to generate an IAM token by using `POST /identity/v1/iam_tokens`.
{: note}

Example request:

```json
iam_token=`curl -X POST "$vpc_metadata_api_endpoint/identity/v1/iam_tokens?version=2025-10-14" \
-H "Authorization: Bearer $identity_token" \
-d '{
      "trusted_profile": {
        "id": "Profile-8dd84246-7df4-4667-94e4-8cede51d5ac5"
      }
    }'| jq -r '(.access_token)'`
```
{: codeblock}

The JSON response shows the IAM token.

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0aGVfYmVzdCI6I8...",
  "created_at": "2025-06-10T14:10:15Z",
  "expires_at": "2025-06-10T15:10:15Z",
  "expires_in": 3600
}
```
{: codeblock}

For more information about trusted profiles, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Generating an identity certificate by using an identity access token
{: #imd-acquire-certificate}
{: api}

Identity certificates are required to successfully enable and use encryption in transit between virtual server instances and {{site.data.keyword.filestorage_vpc_full}} shares. To generate an identity certificate for the instance, make a `POST /identity/v1/certificates` call with the identity access token and a certificate signing request (CSR).

You can obtain the certificate signing requests (CSRs) from the open source command-line toolkit, [OpenSSL](https://docs.openssl.org/){: external}.

   1. The following command generates a *Certificate Signing Request* (CSR) and *RSA Key Pair* by using openssl. When you run the command, replace the country code `US` with your two-digit country code in `'/C=US'`.
      ```sh
      openssl req -sha256 -newkey rsa:4096 -subj '/C=US' -out ./sslcert.csr -keyout file.key -nodes
      ```
      {: pre}

       If you're using a different software to create the CSR, you might be prompted to enter information about your location such as country code (C), state (ST), locality (L), your organization name (O), and organization unit (OU). Any one of these naming attributes can be used. Any other naming attributes, such as common names are rejected. CSRs with Common Name specified are rejected because when you make the request to the Metadata API, the system applies instance ID values to the subject Common Name for the identity certificates. CSRs with extensions are also rejected.
      {: important}

   2. Format the csr before you make an API call to the metadata service by using the following command.
      ```sh
      awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' sslcert.csr
      ```
      {: pre}

Then, you can make the API request to the Metadata service. See the following example. The `csr` value is required. The `expires_in` value is optional. The default value for expiration is 3600, which equals 1 hour.

```sh
curl -X POST "$vpc_metadata_api_endpoint/identity/v1/certificates?version=2024-11-12" \
 -H "Authorization: Bearer $identity_token" \
 -d '{ "csr": "-----BEGIN CERTIFICATE REQUEST-----\nMIICnTCCAYUCAQAwWDELMAkGA1UEBhMCVVMxEjAQBgNVBAgMCU1pbm5lc290YTES\nMBAGA1UEBwwJUm9jaGVzdGVyMSEwHwYDVQQKDBhJbnRlcm5ldCBXaWRnaXRzIFB0\neSBMdGQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCYBvW12cKEkRUu\nyPScs7Xjwu/m+W8pZSQf9wrBa7DBVLFCdh440xOuSnIbsm+BNgYz4wL6/8la+N/K\nff06CdEwy9HLhPYc2z62tECxOBhI1G9gnsRUwb6WHNY71VulZs+37/9Mgd/eQy2n\nKHULNEU7sjNpLYoguKX8GRV3etKDp3tlFQmB6cNGOAgB3aQDmhdAh7K6oftesm0R\n8C7nmFA4SSjaI+855JxoxadlB2cCA5boaQ2gNO6YhYbtuTrMicQb0MTlZmacqzqP\nAxXWD3yFmAuUCpa2tBFBsavSW/kc52m4ldcO60U6hARvOxcXDqrbwu8r1ieY+tcZ\ncqjjBi99AgMBAAGgADANBgkqhkiG9w0BAQsFAAOCAQEAgAqWjtH3yAsX8QfTa9Pv\n3kktYFQKFsBzntmFDdIrOkeGayWRCuSG06f3sHWH0RuGkpq1x/4bedjcyyNVSna7\nxYX6kPOQX5iqf9pISD7A0XIkfS6XAos7gOh/jadjtxSwPCkuztSqIPKObH9OClAE\nU1fYDEtZCaZxsUdLwWJwOzbsivT97g1UVnbJAEzAJrqyaV4cUbv/w/slytHF+GAg\nNoUvPD8NGOQ+VzuI2oQuK515cyHO1SXrJyvkEVwRVVr3SoasqqWIQRrIv6zgzgik\nLN+uQxpzL1EeTB8qKy7xjymo2y1PbmaZzVNQNaBnxJfLE522pfW69evBRJ1qhrby\nTQ==\n-----END CERTIFICATE REQUEST-----\n"}'
```
{: codeblock}

Or you can also use the following command:

```sh
curl -X POST "$vpc_metadata_api_endpoint/identity/v1/certificates?version=2025-08-26" \
 -H "Authorization: Bearer $identity_token" \
 -d '{ "csr": "-----BEGIN CERTIFICATE REQUEST-----\nMIICnTCCAYUCAQAwWDELMAkGA1UEBhMCVVMxEjAQBgNVBAgMCU1pbm5lc290YTES\nMBAGA1UEBwwJUm9jaGVzdGVyMSEwHwYDVQQKDBhJbnRlcm5ldCBXaWRnaXRzIFB0\neSBMdGQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCYBvW12cKEkRUu\nyPScs7Xjwu/m+W8pZSQf9wrBa7DBVLFCdh440xOuSnIbsm+BNgYz4wL6/8la+N/K\nff06CdEwy9HLhPYc2z62tECxOBhI1G9gnsRUwb6WHNY71VulZs+37/9Mgd/eQy2n\nKHULNEU7sjNpLYoguKX8GRV3etKDp3tlFQmB6cNGOAgB3aQDmhdAh7K6oftesm0R\n8C7nmFA4SSjaI+855JxoxadlB2cCA5boaQ2gNO6YhYbtuTrMicQb0MTlZmacqzqP\nAxXWD3yFmAuUCpa2tBFBsavSW/kc52m4ldcO60U6hARvOxcXDqrbwu8r1ieY+tcZ\ncqjjBi99AgMBAAGgADANBgkqhkiG9w0BAQsFAAOCAQEAgAqWjtH3yAsX8QfTa9Pv\n3kktYFQKFsBzntmFDdIrOkeGayWRCuSG06f3sHWH0RuGkpq1x/4bedjcyyNVSna7\nxYX6kPOQX5iqf9pISD7A0XIkfS6XAos7gOh/jadjtxSwPCkuztSqIPKObH9OClAE\nU1fYDEtZCaZxsUdLwWJwOzbsivT97g1UVnbJAEzAJrqyaV4cUbv/w/slytHF+GAg\nNoUvPD8NGOQ+VzuI2oQuK515cyHO1SXrJyvkEVwRVVr3SoasqqWIQRrIv6zgzgik\nLN+uQxpzL1EeTB8qKy7xjymo2y1PbmaZzVNQNaBnxJfLE522pfW69evBRJ1qhrby\nTQ==\n-----END CERTIFICATE REQUEST-----\n"}'
```
{: codeblock}

A successful response returns the new certificate with information such as its ID, and creation and expiration times.

```json
{
  "certificates": [
    "-----BEGIN CERTIFICATE-----\nMIIDmTCCAoECFDGlhn2VlwNEQymsNpyt9rOiiiWDMA0GCSqGSIb3DQEBCwUAMIGJ\nMQswCQYDVQQGEwJVUzESMBAGA1UECAwJTWlubmVzb3RhMRIwEAYDVQQHDAlSb2No\nZXN0ZXIxDDAKBgNVBAoMA0lCTTEeMBwGA1UECwwVVmlydHVhbCBQcml2YXRlIENs\nb3VkMSQwIgYDVQQDDBtWUEMgRXhhbXBsZSBJbnRlcm1lZGlhdGUgQ0EwHhcNMjIx\nMTAxMTM1MDE0WhcNMjIxMTAxMTQyMDE0WjCBhzELMAkGA1UEBhMCVVMxEjAQBgNV\nBAgMCU1pbm5lc290YTESMBAGA1UEBwwJUm9jaGVzdGVyMQwwCgYDVQQKDANJQk0x\nHjAcBgNVBAsMFVZpcnR1YWwgUHJpdmF0ZSBDbG91ZDEiMCAGA1UEAwwZRXhhbXBs\nZSBTaGFyZSBDZXJ0aWZpY2F0ZTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoC\nggEBAM6JytY3R4zWo3zzw/dM9ldUw8TIDQ9dNt+0sm3bFHHlAXaSKvmI+Ls/uQoh\n9VPpRLTx+WyljnKNnkXC6BQOzlugjAfi8hE2f5CC0A0m58XcBiZqH5BwTeLI4vVZ\nO9pLySckkEtHcmFE4h70KS5+1jDApeOTTS6EJsQcal/AAVYg7PDyXr1jE2HTKxnt\nlXopB/+bvWmBQ2k50Km0h0D1n0Ipoqqwb1wwWCrzQ2ds2XNKCUGkCgN6buFiF2nN\nLYS1tsIaw6OsTx+VheNGlYdlOhMUVypCok9JQ85P4NU47O6YgITX1V63ewZBnn5p\napywqdg8K2X2YgU/tLdpl5Jz2ysCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEABuOX\npxGbBQPdG3VGkNCYScZUcxocqmx4mCegBFfv4PjWU2+eG+3JikB3YWwqD11hixQm\n5Qwge/zMXzuKPs5D4yyblpDJlq5Iz/0VMjEl2paCHg9nm5Z3QaSydFH3SCGwfvld\nRn9ib6DSw4a58hmqON+CiWUSSibQy46gUsqVvYhq2lJimejTAN2DlePY2su1xvNV\nAdmDjmvO7j7YV/eWk6r7OgcqtVaAovN3okaybwxf8sLAFxLzp/aUaqXL10qJ/ISz\nVL+UHN7t5WzjHdh2OjDXwz0BOyhdbjyNX8ptKd+E0O21PsFFe8ErfShDh00g/ERP\nzXuEUsCxzTyWRTm8GA==\n-----END CERTIFICATE-----\n",
    "-----BEGIN CERTIFICATE-----\nMIIEADCCAuigAwIBAgIUDzQruKqvBY7+CS6DL0u93Na6cLMwDQYJKoZIhvcNAQEL\nBQAwgYExCzAJBgNVBAYTAlVTMRIwEAYDVQQIDAlNaW5uZXNvdGExEjAQBgNVBAcM\nCVJvY2hlc3RlcjEMMAoGA1UECgwDSUJNMR4wHAYDVQQLDBVWaXJ0dWFsIFByaXZh\ndGUgQ2xvdWQxHDAaBgNVBAMME1ZQQyBFeGFtcGxlIFJvb3QgQ0EwHhcNMjIxMTAx\nMDM0OTI5WhcNMjcxMDMxMDM0OTI5WjCBiTELMAkGA1UEBhMCVVMxEjAQBgNVBAgM\nCU1pbm5lc290YTESMBAGA1UEBwwJUm9jaGVzdGVyMQwwCgYDVQQKDANJQk0xHjAc\nBgNVBAsMFVZpcnR1YWwgUHJpdmF0ZSBDbG91ZDEkMCIGA1UEAwwbVlBDIEV4YW1w\nbGUgSW50ZXJtZWRpYXRlIENBMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC\nAQEAxjvxOtSFKsJKl4teBLgkX4+myxhClz2Qmg5MnNQ+oyhyNrpYvjG3+O+DrSUK\nKTXzmWSkKU/6BKmHQPNdpd4ymbb0cG7wmpcU3YjjrSNFgd/o3CEK9M7+ofIuQtTX\nXNUQWX5rb3wBqEA1TWazVTZpphhhcGQ8u03VTKvoF4S2DI6L3brDJJ0w1DM9Isaa\nB2mS64VYMIj3jLry39ryGEoYq1a0tC4C9fET3V5NmUnIRNqVDnGGkYBy/57VRACU\nXxXcQuW6eoPYGk6Ho3eKly34eilF2n9xD/bB41R4NzaxO/0lHq+caI5r1WlnTXtF\nE8wLpFoYMkuC0qiKBesyuyef2QIDAQABo2YwZDAdBgNVHQ4EFgQU2MIYc9g4Z7Kj\n79u2HPGYyTk5QHwwHwYDVR0jBBgwFoAUVnTLKJHyjHUcRp22jx+d3uGqnrwwEgYD\nVR0TAQH/BAgwBgEB/wIBADAOBgNVHQ8BAf8EBAMCAYYwDQYJKoZIhvcNAQELBQAD\nggEBADhOBfnBEaWVWCsZo3UR7UlP5/8i3mRgyFt4YkICPMacy2IcnDw8aoyjTO5b\n4BLO4J1m4AmcJnDJcFIEKLBSNbzsiDdP2rWIAAJKO4gKxdTArIuLgq7zrR74j46L\nn6IFwumKQRw0diGYD6wWIo/f9kGy1NQ46igmRYrEfzA5HWitEpF0mu6lz8mZ8m9s\na6CTEqwLFhP+qOcWtpGjNTa+OHENAmmAR4mR4Os4MsBBnb4RA//S/4suW419Cz8N\n1/Ul7KduYRKpRMSiS9YWbCvC5WiEvOvfp8Z4ecXlC+ohU5MLuCRPfP+blBvxNx2O\nsLotlbzDpim/gYiJCHgW3POlsLE=\n-----END CERTIFICATE-----\n"
  ],
  "created_at": "2024-11-12T13:50:14Z",
  "expires_at": "2024-11-12T14:50:14Z",
  "expires_in": 3600,
  "id": "9fd84246-7df4-4667-94e4-8ecde51d5ac5"
}
```
{: codeblock}

For more information, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).

## Decoding the identity access token
{: #imd-json-token-decode}
{: api}

{{site.data.keyword.vpc_full}} Identity metadata access tokens are standard JSON Web Tokens (JWTs). You can decode them using command-line tools, online decoders, or programmatic libraries to view claims like expiration time (exp), server info (iaas), and nonce.

The fastest way to decode an {{site.data.keyword.vpc_short}} identity token in your terminal is using standard Unix tools or specialized CLI utilities.

Identity tokens consist of three parts (header, payload, signature) separated by period `.` separators.
1. The encoded header object consists of key-value pairs.
1. The encoded payload object consisting of claims, which are essentially key-value pairs. The following are examples of {{site.data.keyword.vpc_short}} claims in identity tokens.
   - `iaas`
   - `vpc`
   - custom ip addresses
   - subnets
1. The signature of the issuer which is used to determine whether the JWT code was compromised or altered after it was issued. The signature is created by the issuer by signing the encoded header and payload objects with a server or private key. The issuer is indicated by the `iss` claim.

- To decode an identity token using base64 and jq, you can decode the payload (the second part) with the following command.

   ```bash
   # Replace <TOKEN> with your actual token string
   echo "<TOKEN>" | cut -d '.' -f 2 | base64 --decode | jq .
   ```
   {: pre}

   For tokens that use URL-safe Base64, you might need to replace `-` with `+` and `_` with `/` before decoding.

- To decode an identity token using jwt-cli, use the following steps. This process should be used with caution.

   1. Install a dedicated tool for a cleaner output.

      ```bash
      npm install -g jwt-cli
      ```
      {: pre}

   1. Issue the following command.

      ```bash
      jwt decode <TOKEN>
      ```
      {: pre}

   1. Review the information in the payload section. See the following example.

      ```screen
      {
        "aud": [
          "VSI-CR_us-south",
          "247cd7d3-ff6b-11ef-8b70-ca73f727becf_63791398320466773051136262737897537855791952320"
        ],
        "exp": 1741802544,
        "iaas": {
          "crn": "crn:v1:staging:public:is:us-south-3:a/af6443f619a949c9919c1eb1625d6cc5::instance:7389_bca3d8e4-b8b4-42e0-881e-761a01a89f20",
          "accountID": "af6443f619a949c9919c1eb1625d6cc5",
          "instanceID": "7389_bca3d8e4-b8b4-42e0-881e-761a01a89f20",
          "zone": "us-south-3",
          "region": "us-south",
          "profile_name": "bx2-2x8",
          "resource_group_id": "6d42cce33e604a86b95485c5735be52d"
        },
        "iat": 1741802244,
        "ip_addresses": [
          {
            "address": "52.118.123.193"
          },
          {
            "address": "10.240.128.4"
          }
        ],
        "iss": "VSI-CR_us-south",
        "nonce": "2026-03-12T12:34:56.789Z",
        "subnets": [
          {
            "crn": "crn:v1:staging:public:is:us-south-3:a/af6443f619a949c9919c1eb1625d6cc5::subnet:7389-9b1a8341-5863-40da-a29f-9a9d6eef2a51",
            "name": "fode-sn3",
            "id": "7389-9b1a8341-5863-40da-a29f-9a9d6eef2a51"
          }
        ],
        "vpc": {
          "crn": "crn:v1:staging:public:is:us-south:a/af6443f619a949c9919c1eb1625d6cc5::vpc:r134-199db3c7-928c-41af-95fd-d4e166648773",
          "name": "fode",
          "id": "r134-199db3c7-928c-41af-95fd-d4e166648773"
        }
      }
      ```
      {: codeblock}

   1. To validate the JWT, check the `iss` signature and verify the encoded header object and the encoded payload object were not altered after the JWT was issued.

You can also use a third party tool, such as [python-jwt](https://pypi.org/project/jwt/){: external} or [golang-jwt](https://github.com/golang-jwt/jwt){: external}, to decode the JWT.

## Creating a trusted profile for the instance
{: #imd-trusted-profile-config}

Trusted profiles for compute resource identities help you assign an {{site.data.keyword.cloud}} IAM identity to an {{site.data.keyword.cloud}} resource, such as a virtual server instance. You can call any IAM-enabled service from an instance without having to manage and distribute IAM secrets to the instance. You can create a trusted profile when you generate an IAM token from an identity access token and link it to the instance. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Next steps
{: #imd-identity-operations-next}

After you create an identity access token and enable access to the metadata service, you can retrieve metadata for the instance, SSH keys, and placement groups. For more information, see [Retrieving metadata from an instance](/docs/vpc?topic=vpc-imd-access-instance-metadata).
