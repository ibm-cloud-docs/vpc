---

copyright:
  years: 2025, 2025
lastupdated: "2025-11-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configure the metadata service for bare metal servers
{: #configure-metadata-service-bare-metal}

Configure the metadata service by obtaining a bare metal server identity access token from the metadata service. Optionally, generate an IAM access token from this token to access IAM-enabled services in the account.
{: shortdesc}

## Accessing the metadata service by using the bare metal server identity access token service
{: #metadata-get-token-bare-metal}

To access the bare metal server metadata service, you must first obtain a bare metal server identity access token (a JSON Web Token). You can later generate an IAM token from the bare metal server identity access token and then use it to access IAM-enabled services.

Windows users have extra requirements to set up the metadata service. For more information, see [Setting up Windows servers for using the metadata service](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-service-enable-bare-metal)

### Bare metal server identity access token concepts
{: #metadata-token-concepts-bare-metal}

A bare metal server identity access token provides a security credential for accessing the metadata service. It's a signed token with a set of claims based on information about the bare metal server and information that is passed in the token request. The minimum version date to use the bare metal server identity access token feature is 1 March 2022.

To access the bare metal server identity, make a `PUT "https://api.metadata.cloud.ibm.com/i /identity/v1/token` call by using the [Metadata service API](/apidocs/vpc-identity-beta/initial#create-access-token) that invokes the bare metal server hostname. Communication between the bare metal server and metadata service never stops with the host. You acquire the token from within the bare metal server. If `https` secure protocol is not enabled on your bare metal server, you can use your bare metal server IP address instead of the hostname.

In the request, you specify an expiration time for the token. The default is 5 minutes, but you can specify that it expires sooner or later (5 seconds to 1 hour).

The response (a JSON payload) contains the bare metal server identity access token. Use this token to access the metadata service.

You can also generate an IAM token from this token and use the API to call IAM-enabled services. For more information, see [Generate an IAM token from a bare metal server identity access token](/docs/vpc?topic=vpc-get-metadata-bare-metal&interface=api#metadata-token-exchange-usemd-bare-metal).

### Acquiring a bare metal server identity access token
{: #metadata-json-token-bare-metal}

Using the Metadata service API, make `PUT /identity/v1/token` call to get a bare metal server identity access token from the token service. The following example uses `jq` to parse the JSON API response and then extract the bare metal server identity access token value. You can use your preferred JSON parser.

In the example, the return value of the CURL command is the bare metal server identity access token, which is extracted by `jq` and placed in the `identity_token` environment variable. You use specify this variable in a `GET` call to the metadata service to reach the metadata endpoint. For more information, see [Acquire a bare metal server identity access token](/docs/vpc?topic=vpc-get-metadata-bare-metal&interface=api#metadata-json-token-usemd-bare-metal).

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create a bare metal server. You might need to install `jq` before use or use any parser of your choice.
{: note}

```json
identity_token=`curl -X PUT "https://api.metadata.cloud.ibm.com/identity/v1/token?version=2025-06-30"\
  -H "Metadata-Flavor: ibm"\
  -d '{
        "expires_in": 3600
      }' | jq -r '(.access_token)'`
```
{: pre}

The following JSON response shows the bare metal server identity access token character string, date, and time that it was created, date, and time that it expires, and expiration time you set. Keep in mind that the token expires in 5 minutes.

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IlZTSS1DUl91cy1lYXN0X2I5...",
  "created_at": "2025-06-30T11:08:39.363Z",
  "expires_at": "2025-06-30T11:13:39.363Z",
  "expires_in": 3600
}
```
{: codeblock}

## Generate an IAM token from a bare metal server identity access token
{: #metadata-token-exchange-bare-metal}
{: api}

To access IBM Cloud IAM-enabled services in the account, you can generate an IAM token from the bare metal server identity access token by using trusted profile information. After you generate the IAM token, you can use it to access IAM-enabled services, such as {{site.data.keyword.cos_full_notm}}, Cloud Database Service, and the VPC APIs. You can reuse the token multiple times.

Make a `POST /identity/v1/iam_tokens` call and specify the ID of the trusted profile. This request uses the bare metal server identity access token and a trusted profile that is linked to a bare metal server to generate an IAM access token. The trusted profile can be linked either when you create the bare metal server or provided in the request body.

The IAM API used to pass the bare metal server identity access token and generate an IAM token is being deprecated. Beta users must migrate to the metadata service API to generate an IAM token by using `POST /identity/v1/iam_tokens`.
{: note}

Example request:

```json
iam_token=`curl -X POST "$vpc_metadata_api_endpoint/identity/v1/iam_tokens?version=2025-06-30"\
   -H "Authorization: Bearer $identity_token"\
   -d '{
       "trusted_profile": {
          "id": "Profile-8dd84246-7df4-4667-94e4-8cede51d5ac5"
       }
      }' | jq -r '(.access_token)'`
```
{: pre}

The JSON response shows the IAM token.

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0aGVfYmVzdCI6I8...",
  "created_at": "2025-06-30T14:10:15Z",
  "expires_at": "2025-06-30T15:10:15Z",
  "expires_in": 3600
}
```
{: codeblock}

For more information about trusted profiles, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Generating a bare metal server identity certificate by using a bare metal server identity access token
{: #metadata-acquire-certificate-bare-metal}

Bare metal server identity certificates are required to successfully enable and use encryption in transit between bare metal servers and {{site.data.keyword.filestorage_vpc_full}} shares. To generate a bare metal server identity certificate for the bare metal server, make a `POST /identity/v1/certificates` call with the bare metal server identity access token and a certificate signing request (CSR).

You can obtain the certificate signing requests (CSRs) from the open source command-line toolkit, [OpenSSL](https://docs.openssl.org/){: external}.

   1. The following command generates a *Certificate Signing Request* (CSR) and *RSA Key Pair* by using openssl. When you run the command, replace the country code `US` with your two-digit country code in `'/C=US'`.
      ```sh
      openssl req -sha256 -newkey rsa:4096 -subj '/C=US' -out ./sslcert.csr -keyout file.key -nodes
      ```
      {: pre}

       If you're using a different software to create the CSR, you might be prompted to enter information about your location such as country code (C), state (ST), locality (L), your organization name (O), and organization unit (OU). Any one of these naming attributes can be used. Any other naming attributes, such as common name, are rejected. CSRs with Common Name specified are rejected because when you make the request to the Metadata API, the system applies bare metal server ID values to the subject Common Name for the bare metal server identity certificates. CSRs with extensions are also rejected.
      {: important}

   2. Format the csr before you make an API call to the metadata service by using the following command.
      ```sh
      awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' sslcert.csr
      ```
      {: pre}

Then, you can make the API request to the Metadata service. See the following example. The `csr` value is required. The `expires_in` value is optional. The default value for expiration is 3600, which equals 1 hour.

```sh
curl -X POST "$vpc_metadata_api_endpoint /identity/v1/certificates?version=2025-06-30" \
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
  "created_at": "2025-06-30T13:50:14Z",
  "expires_at": "2025-06-30T14:50:14Z",
  "expires_in": 3600,
  "id": "9fd84246-7df4-4667-94e4-8ecde51d5ac5"
}

```
{: screen}

For more information, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).

## Creating a trusted profile for the bare metal server
{: #metadata-create-trusted-profile-config}

Trusted profiles for compute resource identities help you assign an {{site.data.keyword.cloud}} IAM identity to an {{site.data.keyword.cloud}} resource, such as a bare metal server. You can call any IAM-enabled service from a bare metal server without having to manage and distribute IAM secrets to the bare metal server. You can create a trusted profile when you generate an IAM token from a bare metal server identity access token and link it to the bare metal server. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-metadata-trusted-profile-bare-metal).

## Enabling or disabling the bare metal server metadata service
{: #metadata-service-enable-bare-metal}

The bare metal server metadata service is disabled by default. To retrieve metadata from a bare metal server, enable the service on new bare metal servers or existing bare metal servers by using the VPC UI, CLI, API, or Terraform.

The bare metal server metadata service is disabled by default. To retrieve metadata from a bare metal server, enable the service on new bare metal servers or existing bare metal servers by using the VPC UI, CLI, API, or Terraform.

### Enabling bare metal server metadata in the console
{: #metadata-enable-service-ui-bare-metal}
{: ui}

From the {{site.data.keyword.cloud}} console, you can enable or disable the bare metal server metadata service.

#### Enable the metadata service for an existing bare metal server in the console
{: #metadata-enable-on-bare-metal-server-ui-bare-metal}

Use the UI to enable the metadata service on an existing bare metal server.

1. Go to the list of bare metal servers. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.

1. Click the name of a bare metal server to go to the details page.

1. Go to **Advanced configuration details**.

1. Under **Metadata**, click the toggle (appears green).

#### Enable the metadata service when you create a new bare metal server in the console
{: #metadata-enable-new-bare-metal-server-ui-bare-metal}

The following procedure shows how to enable the metadata service when you create a new bare metal server.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.

1. Click **Create**.

1. [Provision the bare metal server](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=ui).

1. Go to **Advanced options**.

1. Under **Metadata**, click the toggle (appears green).

 For more information about creating bare metal servers, see [Creating Bare Metal Servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=ui).

### Disable the metadata service in the console
{: #metadata-disable-new-bare-metal-server}
{: ui}

This procedure shows how to disable the metadata service for a bare metal server on which it is enabled. By default, the metadata service is disabled when you create a new bare metal server.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.

2. Click a bare metal server from the list to go to its details page.

3. Go to **Advanced configuration details**. Under **Metadata**, the click the toggle to off (appears gray).

### Enable or disable bare metal server metadata from the CLI
{: #metadata-service-enable-cli-bare-metal}
{: cli}

Use the CLI to enable the metadata service when you create a new bare metal server or on an existing bare metal server.

Before you begin:

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

1. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli#create-a-vpc-cli).

#### Enable or disable the metadata service when you create a bare metal server by using the CLI
{: #metadata-enable-on-bare-metal-server-cli-bare-metal}

Run the `ibmcloud bare-metal-server-create` command and set the `metadata-service` property to `true`. The metadata service is disabled by default. In the response, you see `Metadata service enabled` set to `true`.

```json
ibmcloud is bare-metal-server-create test-bare-metal-server-1 7002c1cd-9b0b-43ee-8112-5124dedbe84b us-south-1  bx2-2x8  0711-08206578-d749-49ea-86c9-1014622d1c6f --image-id 9f0050d0-636b-4fe6-82ea-931664fd9d91 --metadata-service true

Creating bare metal server test under account VPC1 as user myuser@mycompany.com...

ID                         264060c2-e5e9-44d4-994f-eea4a6688172
Name                       test-bare-metal-server-1
CRN                        crn:v1:public:is:us-south-1:a/a1234567::bare-metal-server:264060c2-e5e9-44d4-994f-eea4a6688172
Status                     pending
Startable                  true
Profile                    bx2-2x8
Architecture               amd64
vCPUs                      2
Memory(GiB)                8
Bandwidth(Mbps)            4000
Metadata service enabled   true
Image                      ID                                          Name
                           9f0050d0-636b-4fe6-82ea-931664fd9d91        ibm-ubuntu-20-04-minimal-amd64-1

VPC                        ID                                          Name
                           7002c1cd-9b0b-43ee-8112-5124dedbe84b        test-vpc1

Zone                       us-south-1
Resource group             ID                                  Name
                           21cabbd983d9c4beb82690daab08717e8   Default

Created                    2022-08-08T22:12:11+05:30
Boot volume                ID   Name           Attachment ID                               Attachment name
                           -    PROVISIONING   954c1c47-906d-423f-a8c8-dd3adfafd278        my-vol-attachment1
```
{: codeblock}

#### Enable or disable the metadata service for an existing bare metal server from the CLI
{: #metadata-enable-on-existing-bare-metal-server-cli-bare-metal}

Run the `ibmcloud is bare-metal-server-update` command and specify the bare metal server ID. To enable the metadata service, set the `metadata-service` parameter to `true`; to disable, set it to `false`. An example command for enabling the service looks like this:

```sh
ibmcloud is bare-metal-server-update e219a883-41f2-4680-810e-ee63ade35f98 --metadata-service true
```
{: codeblock}

### Enable or disable bare metal server metadata from the API
{: #metadata-service-enable-api-bare-metal}
{: api}

#### Enable or disable the metadata service when you create a new bare metal server by using the API
{: #metadata-disable-on-bare-metal-server-api-bare}

The metadata service is disabled by default when you create a new bare metal server by making a `POST /bare_metal_servers` call.

You can enable the service by specifying the `metadata_service` parameter and setting `enabled` to `true`.

This example shows enabling the metadata service at bare metal server creation:

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers?version=2025-06-30&generation=2"\
-H "Authorization: Bearer $iam_token"\
-d '{
      "image": {
         "id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366"
      },
      "keys": [
         {
           "id": "363f6d70-0000-0001-0000-00000013b96c"
         }
      ],
      "name": "my-bare-metal-server",
      "metadata_service": {
         "enabled": true
      },
      .
      .
      .
   }'
```
{: codeblock}

The response shows that the metadata parameter set to `true` when you enable the service. You can also verify the metadata service setting by making a `GET /bare_metal_servers/{id}` call.

#### Enable or disable the metadata service for an existing bare metal server from the API
{: #metadata-enable-on-bare-metal-server-api}

To enable or disable the service from an existing bare-metal-server, make a `PATCH /bare_metal_servers/{id}` call and specify the `metadata_service` parameter. By default, the `enabled` property is set to `false`. To enable the service, set it to `true`.

This example call shows enabling the metadata service for a bare metal server:

```sh
curl -X PATCH "$vpc_api_endpoint/v1/bare_metal_servers/$id?version=2025-06-30&generation=2"\
    -H "Authorization: Bearer $iam_token"\
    -d '{
          "metadata_service": {
            "enabled": true
          }
      }'
```
{: codeblock}

The response shows that the metadata parameter set to `true` when you enable the service. You can also verify the metadata service setting by making a `GET /bare_metal_servers/{id}` call.

### Enabling bare metal server metadata from Terraform
{: #metadata-enable-service-terraform-bare-metal}
{: terraform}

From the {{site.data.keyword.cloud}} console, you can enable or disable the bare metal server metadata service.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

#### Enable the metadata service for an existing bare metal server from Terraform
{: #metadata-enable-on-bare-metal-server-terraform-bare-metal}

Use Terraform to enable the metadata service on an existing bare metal server.

The following example enables the metadata service on an existing bare metal server. By default, the `enabled` property is set to `false`. To enable the service, set it to `true`.

```terraform
resource "ibm_is_bare_metal_server" "example" {
  profile = "bx2-metal-192x768"
  name    = "example-bms1"
  image   = "r134-cdc7b64f-8d86-4412-ac96-7765b1e9253f"
  zone    = "us-south-2"
  keys    = [ibm_is_ssh_key.example.id]
  primary_network_interface {
    subnet = ibm_is_subnet.example.id
  }
  vpc = ibm_is_vpc.example.id
  default_trusted_profile {
      auto_link = false
      target {
          id = "Profile-a2075c9d-c69f-404f-8488-7962a383059c"
      }
    }
  metadata_service {
    enabled  = true
    protocol = "https"
  }
}
```
{: codeblock}

#### Enable the metadata service when you create a new bare metal server from Terraform
{: #metadata-enable-new-bare-metal-server-terraform-bare-metal}

Use Terraform to enable the metadata service on a new bare metal server.

The following example enables the metadata service on a new bare metal server. By default, the `enabled` property is set to `false`. To enable the service, set it to `true`.

```terraform
resource "ibm_is_vpc" "example" {
  name = "example-vpc"
}

resource "ibm_is_subnet" "example" {
  name            = "example-subnet"
  vpc             = ibm_is_vpc.vpc.id
  zone            = "us-south-3"
  ipv4_cidr_block = "10.240.129.0/24"
}

resource "ibm_is_ssh_key" "example" {
  name       = "example-ssh"
  public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
}

resource "ibm_is_bare_metal_server" "example" {
  profile = "bx2-metal-192x768"
  name    = "example-bms1"
  image   = "r134-cdc7b64f-8d86-4412-ac96-7765b1e9253f"
  zone    = "us-south-2"
  keys    = [ibm_is_ssh_key.example.id]
  primary_network_interface {
    subnet = ibm_is_subnet.example.id
  }
  vpc = ibm_is_vpc.example.id
  default_trusted_profile {
      auto_link = true
      target {
          id = "Profile-a2075c9d-c69f-404f-8488-7962a383059c"
      }
    }
  metadata_service {
    enabled  = true
    protocol = "https"
  }
}
```
{: codeblock}

### Disable the metadata service from Terraform
{: #metadata-disable-new--terraform-bare-metal-server}

Use Terraform to disable the metadata service on a new bare metal server.

The following example disables the metadata service on an existing bare metal server.

```terraform
resource "ibm_is_bare_metal_server" "example" {
  profile = "bx2-metal-192x768"
  name    = "example-bms1"
  image   = "r134-cdc7b64f-8d86-4412-ac96-7765b1e9253f"
  zone    = "us-south-2"
  keys    = [ibm_is_ssh_key.example.id]
  primary_network_interface {
    subnet = ibm_is_subnet.example.id
  }
  vpc = ibm_is_vpc.example.id
  default_trusted_profile {
      auto_link = true
      target {
          id = "Profile-a2075c9d-c69f-404f-8488-7962a383059c"
      }
    }
  metadata_service {
    enabled  = false
    protocol = "https"
  }
}
```
{: codeblock}

## Configure metadata settings by using the UI
{: #metadata-config-ui-bare-metal}
{: ui}

You can configure features of the metadata service by using the UI. When the metadata service is enabled, expand the metadata window to access the metadata service settings.

### Select a trusted profile by using the UI
{: #select-trusted-profile-ui-bare-metal}
{: ui}

You can select an existing trusted profile for the bare metal server metadata service.

**Select a trusted profile when you provision a bare metal server** To select a trusted profile, go to the default trusted profile option, then select a trusted profile from a list of preexisting trusted profiles.

For more information, see [Create a trusted profile for the bare metal server](/docs/vpc?topic=vpc-get-metadata-bare-metal&interface=api#metadata-token-exchange-usemd-bare-metal).

### Toggle auto link by using the UI
{: #auto-link-ui-bare-metal}
{: ui}

After you select a default trusted profile, you can toggle auto link for the bare metal server metadata service. When autolink is enabled, the specified trusted profile is automatically linked to the bare metal server when the bare metal server is provisioned.

On bare metal servers that are provisioned with auto link, the trusted profile is available to the bare metal server immediately at startup. When auto link is disabled, the specified trusted profile must be linked to the bare metal server for it to be used by the bare metal server.

After a server is provisioned, you can't add or remove a default trusted profile or its autolink
{: note}

**Toggle auto link when you provision a bare metal server** To toggle auto link when you provision a bare metal server, go to the Secure access setting in the Metadata window on the bare metal server provision page. Toggle the secure access switch so that it displays `Enabled`.

### Enable secure access by using the UI
{: #secure-access-ui-bare-metal}
{: ui}

You can enable secure access to the bare metal server metadata service. When secure access is enabled, the metadata service is accessible only to the bare metal server by encrypted HTTP secure protocol (HTTPS). When secure access is disabled, the metadata service is accessible only to the bare metal server by unencrypted HTTP protocol. Secure access is disabled by default.

More properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside a bare metal server with secure access to the bare metal server metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}

#### Enable secure access when you provision a bare metal server
{: #secure-access-ui-provisioning-bare-metal}
{: ui}

To enable secure access when you provision a bare metal server, go to the Secure access setting in the Metadata window when you **Create** a bare metal server from the **Bare metal servers for VPC** page. Toggle the secure access switch so that it displays `Enabled`.

#### Enable secure access on an existing bare metal server
{: #secure-access-ui-existing-bare-metal}
{: ui}

To enable secure access on an existing bare metal server, go to the Secure access setting on the **Bare metal server details** page of your bare metal server.

## Configure metadata settings by using the CLI
{: #metadata-config-cli-bare-metal}
{: cli}

You can enable and disable features of the metadata service by using the CLI.

The following example shows a bare metal server with the metadata service enabled.

```text
$ ibmcloud is bare-metal-server bare-metal-server-name -q

ID                                    0716_9cc6d74d-4b77-4cca-b1f4-31cc6edefe01
Name                                  bare-metal-server-name
CRN                                   crn:v1:bluemix:public:is:us-south-1:a/a1234567::bare-metal-server:0716_9cc6d74d-4b77-4cca-b1f4-31cc6edefe01
Status                                running
Availability policy on host failure   restart
Startable                             true
Profile                               bx2-2x8
Architecture                          amd64
vCPUs                                 2
Memory(GiB)                           8
Bandwidth(Mbps)                       4000
Volume bandwidth(Mbps)                1000
Network bandwidth(Mbps)               3000
Lifecycle Reasons                     Code   Message
                                      -      -

Lifecycle State                       stable
Metadata service                      Enabled   Protocol   Response hop limit
                                      false     http       1

Image                                 ID                                          Name
                                      r006-1025e040-7d6f-408c-b4db-6156dc986fc7   ibm-ubuntu-22-04-1-minimal-amd64-2

Numa Count                            1
VPC                                   ID                                          Name
                                      r006-ac1c1ae4-5573-42eb-9194-854c9a3d5555   fode

.
.
.
```
{: codeblock}


### Select a trusted profile by using the CLI
{: #select-trusted-profile-cli-bare-metal}
{: cli}

You can select an existing trusted profile for the bare metal server metadata service.

For more information, see [Create a trusted profile for the bare metal server](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=cli#metadata-create-trusted-profile-config).

You can link a bare metal server to more than one trusted profile, but the properties of a bare metal server support only one default trusted profile setting.

After a server is provisioned, you can't add or remove a default trusted profile or its autolink
{: note}

### Disable auto link
{: #auto-link-cli-bare-metal}
{: cli}

You can disable auto link for the bare metal server metadata service when you provision a bare metal server. Auto link is enabled by default when a trusted profile is selected. When autolink is enabled, the specified trusted profile is automatically linked to the bare metal server when the bare metal server is provisioned. A bare metal server that is provisioned with auto link the trusted profile is available to the bare metal server immediately at startup. When auto link is disabled, the specified trusted profile must be linked to the bare metal server for it to be used by the bare metal server.

To disable auto link, set the `--default-trusted-profile-auto-link` option to `true` when you provision a bare metal server. The following example shows a bare metal server provision command with auto link set to `false`.

```sh
ibmcloud is bare-metal-server-create .... --default-trusted-profile "Profile-9fd84246-7df4-4667-94e4-8ecde51d5ac5" --default-trusted-profile-auto-link false
```
{: pre}

### Enable secure access by using the CLI
{: #secure-access-cli-bare-metal}
{: cli}

You can enable secure access to the bare metal server metadata service. When secure access is enabled, the metadata service is accessible only to the bare metal server by encrypted HTTP Secure protocol (HTTPS). When secure access is disabled, the metadata service is accessible only to the bare metal server by unencrypted HTTP protocol. Secure access is disabled by default.

Certain properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside a bare metal server with secure access to the bare metal server metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}.

#### Enable secure access when you provision a bare metal server
{: #secure-access-cli-provision-bare-metal}
{: cli}

To enable secure access when you provision a bare metal server, specify a value for the `--metadata-service-protocol` option when you use the `bare-metal-server-create` command. For secure access, specify `https`. The default setting is unencrypted `http`.

```sh
ibmcloud is bare-metal-server-create BARE_METAL_SERVER_NAME VPC ZONE_NAME PROFILE_NAME SUBNET ... [--metadata-service-protocol http | https] ...

--metadata-service-protocol value : The communication protocol to use for the metadata service
  endpoint. Applies only when the metadata service is enabled. One of: http, https. (default: "http")
```
{: pre}

To enable secure access on an existing bare metal server, specify a value for the `protocol` subproperty of the `metadata service` when you use the `bare-metal-server-update` command.

## Configure metadata settings by using the API
{: #metadata-config-api-bare-metal}
{: api}

You can enable and disable features of the metadata service by using the API.

### Select a trusted profile by using the API
{: #select-trusted-profile-api-bare-metal}
{: api}

You can select an existing trusted profile for the bare metal server metadata service. Use the `GET /bare_metal_servers/{id}/initialization` call and include the `"default_trusted_profile"`.

For more information, see [Create a trusted profile for the bare metal server](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-create-trusted-profile-config).

You can link a bare metal server to more than one trusted profile, but the properties of a bare metal server support only one default trusted profile setting.

After a server is provisioned, you can't add or remove a default trusted profile or its autolink
{: note}

### Disable auto link by using the API
{: #auto-link-api-bare-metal}
{: api}

You can disable auto link for the bare metal server metadata service when you provision a bare metal server. Auto link is enabled automatically when a trusted profile is selected. When autolink is enabled, the specified trusted profile is automatically linked to the bare metal server when the bare metal server is provisioned. The trusted profile that is associated with a bare metal server that is provisioned with auto link is available to the bare metal server immediately at startup. When auto link is disabled, the specified trusted profile must be linked to the bare metal server for it to be used by the bare metal server.

To disable auto link by using the API, the `auto_link` value for the `default_trusted_profile` property must be set to `false`. The following example shows the `default_trusted_profile` property with the `auto_link` option disabled.

```json
"default_trusted_profile": {
      "auto_link": false,
   "target": {
       "id": "Profile-9fd84246-7df4-4667-94e4-8ecde51d5ac5"
   }
  },
```
{: codeblock}

### Enable secure access by using the API
{: #secure-access-api-bare-metal}
{: api}

You can enable secure access to the bare metal server metadata service. When secure access is enabled, the metadata service is accessible only to the bare metal server by encrypted HTTP Secure protocol (HTTPS). When secure access is disabled, the metadata service is accessible only to the bare metal server by unencrypted HTTP protocol. Secure access is disabled by default.
Certain properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside a bare metal server with secure access to the bare metal server metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}.

#### Enable secure access when you provision a bare metal server
{: #secure-access-api-provision-bare-metal}
{: api}

To enable secure access, when you provision a bare metal server with the [POST /bare_metal_servers](/apidocs/vpc/latest#create-bare-metal-server) method, specify a value for the `metadata_service.protocol` property for your bare metal server. For secure access, specify `https`. The default setting is unencrypted `http`.

#### Enable secure access on an existing bare metal server
{: #secure-access-api-existing-bare-metal}
{: api}

To enable secure access on an existing bare metal server, use the [PATCH /bare_metal_servers/{id}](/apidocs/vpc/latest#update-bare-metal-server) method to update the bare metal server. Specify a value for the `metadata_service.protocol` property for your bare metal server. For secure access, specify `https`. The default setting is unencrypted `http`. 

## Configure metadata settings by using Terraform
{: #metadata-config-terraform-bare-metal}
{: terraform}

You can enable and disable features of the metadata service by using the Terraform

### Select a trusted profile by using Terraform
{: #select-trusted-profile-terraform-bare-metal}
{: terraform}

You can select an existing trusted profile for the bare metal server metadata service. Use the `GET /bare_metal_servers/{id}/initialization` call and include the `"default_trusted_profile"`.

For more information, see [Create a trusted profile for the bare metal server](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-create-trusted-profile-config).

You can link a bare metal server to more than one trusted profile, but the properties of a bare metal server support only one default trusted profile setting.

After a server is provisioned, you can't add or remove a default trusted profile or its autolink
{: note}

For more information, see [Create a trusted profile for the bare metal server](/docs/vpc?topic=vpc-get-metadata-bare-metal&interface=api#metadata-token-exchange-usemd-bare-metal).

### Disable auto link by using Terraform
{: #auto-link-terraform-bare-metal}
{: terraform}

You can disable auto link for the bare metal server metadata service when you provision a bare metal server. Auto link is enabled automatically when a trusted profile is selected. When autolink is enabled, the specified trusted profile is automatically linked to the bare metal server when the bare metal server is provisioned. The trusted profile that is associated with a bare metal server that is provisioned with auto link is available to the bare metal server immediately at startup. When auto link is disabled, the specified trusted profile must be linked to the bare metal server for it to be used by the bare metal server.

To disable auto link by using the API, the `auto_link` value for the `default_trusted_profile` property must be set to `false`. The following example shows the `default_trusted_profile` property with the `auto_link` option disabled.

```json
"default_trusted_profile": {
      "auto_link": false,
      "target": {
           "id": "Profile-9fd84246-7df4-4667-94e4-8ecde51d5ac5"
   }
  },
```
{: codeblock}

### Enable secure access by using Terraform
{: #secure-access-terraform-bare-metal}
{: terraform}

You can enable secure access to the bare metal server metadata service. When secure access is enabled, the metadata service is accessible only to the bare metal server by encrypted HTTP Secure protocol (HTTPS). When secure access is disabled, the metadata service is accessible only to the bare metal server by unencrypted HTTP protocol. Secure access is disabled by default.
Certain properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside a bare metal server with secure access to the bare metal server metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}.

#### Enable secure access when you provision a bare metal server by using Terraform
{: #secure-access-terraform-provisioning-bare-metal}
{: terraform}

To enable secure access, when you provision a bare metal server with the [POST /bare_metal_servers](/apidocs/vpc/latest#create-bare-metal-server) method, specify a value for the `metadata_service.protocol` property for your bare metal server. For secure access, specify `https`. The default setting is unencrypted `http`.

#### Enable secure access on an existing bare metal server
{: #secure-access-terraform-existing-bare-metal}
{: terraform}

To enable secure access on an existing bare metal server, use the [PATCH /bare_metal_servers/{id}](/apidocs/vpc/latest#update-bare-metal-server) method to update the bare metal server. Specify a value for the `metadata_service.protocol` property for your bare metal server. For secure access, specify `https`. The default setting is unencrypted `http`.

## Activity tracking events for bare metal server metadata
{: #metadata-at-events-bare-metal}

Activity tracking events are triggered when you get a [bare metal server identity access token](#metadata-json-token-bare-metal) and then [use the service](/docs/vpc?topic=vpc-get-metadata-bare-metal). For more information about these events, see [Metadata service events](/docs/vpc?topic=vpc-at_events#events-metadata).

## Next steps
{: #metadata-token-next-bare-metal}

After you create a bare metal server identity access token and enable the metadata service, you can retrieve metadata for the bare metal server, SSH keys, and placement groups. For more information, see [Use the metadata service for bare metal servers](/docs/vpc?topic=vpc-get-metadata-bare-metal).
