---

copyright:
  years: 2022, 2025
lastupdated: "2025-04-24"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configure the instance metadata service
{: #imd-configure-service}

Configure the metadata service by obtaining an instance identity access token from the metadata service. Optionally, generate an IAM access token from this token to access IAM-enabled services in the account.
{: shortdesc}

## Accessing the metadata service by using the instance identity access token service
{: #imd-get-token}

To access the instance metadata service, you must first obtain an instance identity access token (a JSON Web Token). You can later generate an IAM token from the instance identity access token and then use it to access IAM-enabled services.

Windows users have extra requirements to set up the metadata service. For more information, see [Setting up Windows servers for using the metadata service](/docs/vpc?topic=vpc-imd-windows-configuration).
{: note}

### Instance identity access token concepts
{: #imd-token-concepts}

An instance identity access token provides a security credential for accessing the metadata service. It's a signed token with a set of claims based on information about the instance and information that is passed in the token request. The minimum version date to use the instance identity access token feature is 2022-03-01.

To access the instance identity, make a `PUT "/instance_identity/v1/token` request to the [Metadata service API](/apidocs/vpc-metadata#create-access-token).

```sh
curl -X PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2025-04-22" -H "Metadata-Flavor: ibm" -d '{}'
```
{: pre}

Communication between the instance and metadata service never leaves the host, you acquire the token from within the instance. If secure access to the instance metadata service is enabled on your instance, use the "https" protocol instead of the "http" protocol.
{: important}

In the request, you can specify an expiration time for the token. The default expiration value is 5 minutes, but you can specify any value between 5 seconds to 1 hour. See the following example for a host that has secure access enabled. In the example, the expiration time of the token is specified as one hour.

```sh
curl -X PUT "https://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2025-04-22" -H "Metadata-Flavor: ibm" -d '{"expires_in": 3600}'
```
{: pre}

The API response contains the instance identity access token. Use this token to access the metadata service.

You can also generate an IAM token from this token and use the RIAS API to call IAM-enabled services. For more information, see [Generate an IAM token from an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange).

### Acquiring an instance identity access token
{: #imd-json-token}

Using the Metadata service API, make `PUT /instance_identity/v1/token` request to get an instance identity access token from the token service.

In the example, the return value of the cURL command is the instance identity access token, which is extracted by `jq` and placed in the `instance_identity_token` environment variable. You can specify this variable in a `GET` call to the metadata service to reach the metadata endpoint. For more information, see [Retrieve metadata from your running instances](/docs/vpc?topic=vpc-imd-get-metadata&interface=api#imd-retrieve-instance-data).

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create an instance. You might need to install `jq` before use or use another parser of your choice.
{: note}

```json
instance_identity_token=`curl -X PUT "https://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2024-11-12"\
  -H "Metadata-Flavor: ibm"\
  -d '{
        "expires_in": 3600
      }' | jq -r '(.access_token)'`
```
{: pre}

The following JSON response shows the instance identity access token character string, date, and time that it was created, date, and time that it expires, and expiration time you set. Keep in mind that the token expires in 5 minutes.

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IlZTSS1DUl91cy1lYXN0X2I5...",
  "created_at": "2024-11-12T11:08:39.363Z",
  "expires_at": "2024-11-12T11:13:39.363Z",
  "expires_in": 3600
}
```
{: codeblock}

## Generate an IAM token from an instance identity access token
{: #imd-token-exchange}
{: api}

To access IBM Cloud IAM-enabled services in the account, you can generate an IAM token from the instance identity access token by using trusted profile information. After you generate the IAM token, you can use it to access IAM-enabled services, such as {{site.data.keyword.cos_full_notm}}, Cloud Database Service, and the VPC APIs. You can reuse the token multiple times.

Make a `POST /instance_identity/v1/iam_token` request and specify the ID of the trusted profile. This request uses the instance identity access token and a trusted profile that is linked to a virtual server instance to generate an IAM access token. The trusted profile can be linked either when you create the instance or provided in the request body.

The IAM API used to pass the instance identity access token and generate an IAM token is being deprecated. Beta users must migrate to the metadata service API to generate an IAM token by using `POST /instance_identity/v1/iam_token`.
{: note}

Example request:

```json
iam_token=`curl -X POST "$vpc_metadata_api_endpoint/instance_identity/v1/iam_token?version=2024-11-12"\
   -H "Authorization: Bearer $instance_identity_token"\
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
  "created_at": "2024-11-12T14:10:15Z",
  "expires_at": "2024-11-12T15:10:15Z",
  "expires_in": 3600
}
```
{: codeblock}

For more information about trusted profiles, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Generating an instance identity certificate by using an instance identity access token
{: #imd-acquire-certificate}

Instance identity certificates are required to successfully enable and use encryption in transit between virtual server instances and {{site.data.keyword.filestorage_vpc_full}} shares. To generate an instance identity certificate for the instance, make a `POST /instance_identity/v1/certificates` call with the instance identity access token and a certificate signing request (CSR).

You can obtain the certificate signing requests (CSRs) from the open-source command-line toolkit, [OpenSSL](https://docs.openssl.org/){: external}.

   1. The following command generates a *Certificate Signing Request* (CSR) and *RSA Key Pair* by using openssl. When you run the command, replace the country code `US` with your two-digit country code in `'/C=US'`.
      ```sh
      openssl req -sha256 -newkey rsa:4096 -subj '/C=US' -out ./sslcert.csr -keyout file.key -nodes
      ```
      {: pre}

       If you're using a different software to create the CSR, you might be prompted to enter information about your location such as country code (C), state (ST), locality (L), your organization name (O), and organization unit (OU). Any one of these naming attributes can be used. Any other naming attributes, such as common name for example are rejected. CSRs with Common Name specified are rejected because when you make the request to the Metadata API, the system applies instance ID values to the subject Common Name for the instance identity certificates. CSRs with extensions are also rejected.
      {: important}

   2. Format the csr before you make an API call to metadata service by using the following command.
      ```sh
      awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' sslcert.csr
      ```
      {: pre}

Then, you can make the API request to the Metadata service. See the following example. The `csr` value is required, the `expires_in` value is optional. The default value for expiration is 3600, which equals 1 hour.

```sh
curl -X POST "$vpc_metadata_api_endpoint/instance_identity/v1/certificates?version=2024-11-12" \
 -H "Authorization: Bearer $instance_identity_token" \
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
{: screen}

For more information, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).

## Creating a trusted profile for the instance
{: #imd-trusted-profile-config}

Trusted profiles for compute resource identities help you assign an {{site.data.keyword.cloud}} IAM identity to an {{site.data.keyword.cloud}} resource, such as a virtual server instance. You can call any IAM-enabled service from an instance without having to manage and distribute IAM secrets to the instance. You can create a trusted profile when you generate an IAM token from an instance identity access token and link it to the instance. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Enabling or disabling the instance metadata service
{: #imd-metadata-service-enable}

The instance metadata service is disabled by default. To retrieve metadata from an instance, enable the service on new instances or existing instances by using the VPC UI, CLI, or API.

### Enable or disable instance metadata by using the UI
{: #imd-enable-service-ui}
{: ui}

From the {{site.data.keyword.cloud}} console, you can enable or disable the instance metadata service.

#### Enable the metadata service for an existing instance by using the UI
{: #imd-enable-on-instance-ui}

Use the UI to enable the metadata service on an existing instance.

1. Go to the list of instances. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

2. Click the name of an instance to go to the details page.

3. On the details page, scroll to **Metadata**.

4. Click the toggle (appears green).

#### Enable the metadata service when you create a new instance by using the UI
{: #imd-enable-new-instance-ui}

The following procedure shows how to enable the metadata service when you create a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

2. Click **Create**.

3. [Provision the instance](/docs/vpc?topic=vpc-creating-virtual-servers).

4. Navigate to **Advanced options** and enable **Metadata**.

 For more information about creating virtual server instances, see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

### Disable the metadata service by using the UI
{: #imd-disable-new-instance}

This procedure shows how to disable the metadata service for an instance on which it is enabled. By default, the metadata service is disabled when you create a new instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

2. Click an instance from the list to go to its details page.

3. Under **Metadata**, the click the toggle button off (appears gray).

#### Enable or disable the metadata service on instance templates by using the UI
{: #imd-enable-instance-template-ui}

When you create an [instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui#creating-instance-template), the enable metadata service toggle is disabled by default. Click the toggle to enable the service.

When you view the details of an existing instance template, the page indicates whether metadata was enabled or not for the template. You can't change the instance template metadata setting after you create the template.

### Enable or disable instance metadata from the CLI
{: #imd-metadata-service-enable-cli}
{: cli}

Use the CLI to enable the metadata service when you create a new instance or on an existing instance.

Before you begin:

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

1. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli#create-a-vpc-cli).

#### Enable or disable the metadata service when you create an instance by using the CLI
{: #imd-enable-on-instance-cli}

Run the `ibmcloud is instance-create` command and set the `metadata-service` property to `true`. The metadata service is disabled by default. In the response, you see `Metadata service enabled` set to `true`.

```json
ibmcloud is instance-create test-instance-1 7002c1cd-9b0b-43ee-8112-5124dedbe84b us-south-1  bx2-2x8  0711-08206578-d749-49ea-86c9-1014622d1c6f --image-id 9f0050d0-636b-4fe6-82ea-931664fd9d91 --metadata-service true

Creating instance test under account VPC1 as user myuser@mycompany.com...

ID                         264060c2-e5e9-44d4-994f-eea4a6688172
Name                       test-instance-1
CRN                        crn:v1:public:is:us-south-1:a/a1234567::instance:264060c2-e5e9-44d4-994f-eea4a6688172
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

#### Enable or disable the metadata service for an existing instance from the CLI
{: #imd-enable-on-existing-instance-cli}

Run the `ibmcloud is instance-update` command and specify the instance ID. To enable the metadata service, set the `metadata-service` parameter to `true`; to disable, set it to `false`. An example command for enabling the service looks like this:

```sh
ibmcloud is instance-update e219a883-41f2-4680-810e-ee63ade35f98 --metadata-service true
```
{: codeblock}

#### Enable or disable the metadata service when you create new instance templates by using the CLI
{: #imd-enable-instance-template-cli}

When you create an [instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=cli#creating-instance-template-cli) from the CLI, you can indicate whether metadata is collected for instances created based on this template.

Use the `ibmcloud is instance-create-from-template` command and specify `metadata-service true` to enable or `metadata-service false` to disable. After you set the template value, you can't change it.

For example, to create an instance template with the metadata service enabled, run this command:

```sh
ibmcloud is instance-template-create my-template-name {template_id} us-south-1 mx2-2x16 {subnet_id} --image-id {image_id} --metadata-service true
```
{: pre}

When you create an instance from this template, specify `metadata-service true` again to enable the service on the new instance:

```sh
ibmcloud is instance-create-from-template --template-id {template_id} --name my-instance --metadata-service true
```
{: pre}

If you override the instance template by running the `ibmcloud is instance-template-create-override-source-template` command, you can enable or disable the metadata service by specifying the `metadata-service` parameter with `true` or `false`.

For more information about these commands, see the [VPC CLI Reference](/docs/vpc?topic=vpc-set-up-environment&interface=cli). For more information about creating an instance template from the CLI, see [Creating an instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=cli#creating-instance-template-cli).

### Enable or disable instance metadata from the API
{: #imd-metadata-service-enable-api}
{: api}

#### Enable or disable the metadata service when you create a new instance by using the API
{: #imd-disable-on-instance-api}

The metadata service is disabled by default when you create a new instance by making a `POST /instances` call.

You can enable the service by specifying the `metadata_service` parameter and setting `enabled` to `true`.

This example shows enabling the metadata service at instance creation:

```sh
curl -X POST "$vpc_api_endpoint/v1/instances?version=2024-11-12&generation=2"\
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
      "name": "my-instance",
      "metadata_service": {
         "enabled": true
      },
      .
      .
      .
   }'
```
{: codeblock}

The response shows that the metadata parameter set to `true` when you enable the service. You can also verify the metadata service setting by making a `GET /instance/{id}` call.

#### Enable or disable the metadata service for an existing instance from the API
{: #imd-enable-on-instance-api}

To enable or disable the service from an existing instance, make a `PATCH /instance/{instance_id}` call and specify the `metadata_service` parameter. By default, the `enabled` property is set to `false`. To enable the service, set it to `true`.

This example call shows enabling the metadata service for an instance:

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-11-12&generation=2"\
    -H "Authorization: Bearer $iam_token"\
    -d '{
          "metadata_service": {
            "enabled": true
          }
      }'
```
{: codeblock}

The response shows that the metadata parameter set to `true` when you enable the service. You can also verify the metadata service setting by making a `GET /instance/{id}` call.

#### Enable or disable the metadata service when you create new instance templates by using the API
{: #imd-enable-instance-template-api}

When you create an instance template, you can set this value by making a `POST /instance/templates` call. By default, the `enabled` property is set to `false`. To enable it, set it to `true`.

For example,

```sh
curl -X POST "$vpc_api_endpoint/v1/instance/templates?version=2024-11-12&generation=2"\
    -H "Authorization: Bearer $iam_token"\
    -d '{
         "image": {
           "id": "3f9a2d96-830e-4100-9b4c-663225a3f872"
         },
         "keys": [
           {
             "id": "363f6d70-0000-0001-0000-00000013b96c"
           }
         ],
         "name": "my-instance-template",
         "metadata_service": {
             "enabled": true
         },
         "primary_network_interface": {
           "subnet": {
             "id": "0d933c75-492a-4756-9832-1200585dfa79"
           }
         },
         "profile": {
           "name": "bx2-2x8"
         },
         "vpc": {
           "id": "dc201ab2-8536-4904-86a8-084d84582133"
         },
         "zone": {
           "name": "us-south-1"
         }
       }'
```
{: pre}

You can't use the API to change the `metadata-service` setting after the instance template is created. If you disabled it for a template, create a new instance template with the `metadata-service` enabled set to `true`.

## Configure metadata settings by using the UI
{: #metadata-config-ui}
{: ui}

You can configure features of the metadata service by using the UI. When the metadata service is enabled, expand the metadata window to access the metadata service settings.

### Select a trusted profile by using the UI
{: #select-trusted-profile-ui}
{: ui}

You can select an existing trusted profile for the instance metadata service.

**Select a trusted profile when you provision an instance** To select a trusted profile, navigate to the Default trusted profile option and select a trusted profile from a list of preexisting trusted profiles.

For more information, see [Create a trusted profile for the instance](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-trusted-profile-config).

### Toggle auto link by using the UI
{: #auto-link-ui}
{: ui}

You can toggle auto link for the instance metadata service. When autolink is enabled, the specified trusted profile is automatically linked to the virtual server instance when the instance is provisioned. On instances provisioned with auto link, the trusted profile is available to the instance immediatley at startup. When auto link is disabled, the specified trusted profile must be linked to the instance for it to be used by the instance.

**Toggle auto link when you provision an instance** To toggle auto link when you provision an instance, navigate to the Secure access setting in the Metadata window on the instance provision page. Toggle the secure access switch so that it displays `Enabled`.

### Enable secure access by using the UI
{: #secure-access-ui}
{: ui}

You can enable secure access to the instance metadata service. When secure access is enabled, the metadata service is accessible only to the virtual server instance by encrypted HTTP secure protocol (HTTPS). When secure access is disabled, the metadata service is accessible only to the virtual server instance by unencrypted HTTP protocol. Secure access is disabled by default.

Additional properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside an instance with secure access to the instance metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}

#### Enable secure access when you provision an instance
{: #secure-access-ui-provisioning}
{: ui}

To enable secure access when provisioning an instance, navigate to the Secure access setting in the Metadata window when you **Create** an instance from the **Virtual server instances for VPC** page. Toggle the secure access switch so that it displays `Enabled`.

#### Enable secure access on an existing instance
{: #secure-access-ui-existing}
{: ui}

To enable secure access on an existing instance, navigate to the Secure access setting on the **Instance details** page of your instance.

### Set the metadata hop limit by using the UI
{: #set-hop-limit-ui}
{: ui}

You can set the hop limit for IP response packets from the metadata service. The hop limit can be any value from 1 (default) to 64. The metadata service must be enabled.

#### Set the metadata hop limit when you provision an instance
{: #set-hop-limit-ui-provisioning}
{: ui}

To set the hop limit when you provision an instance, go to the Hop limit setting in the Metadata window when you **Create** an instance from the **Virtual server instances for VPC** page. Specify a hop limit value between 1 and 64.

#### Set the metadata hop limit on an existing instance
{: #set-hop-limit-ui-existing}
{: ui}

To set the hop limit on an existing instance, go to the Hop limit setting on the **Instance details** page of your instance. Specify a hop limit value between 1 and 64.

## Configure metadata settings by using the CLI
{: #metadata-config-cli}
{: cli}

You can enable and disable features of the metadata service using the CLI.

The following example shows an instance with the metadata service enabled.

```sh
$ ibmcloud is instance instance-name -q

ID                                    0716_9cc6d74d-4b77-4cca-b1f4-31cc6edefe01
Name                                  instance-name
CRN                                   crn:v1:bluemix:public:is:us-south-1:a/a1234567::instance:0716_9cc6d74d-4b77-4cca-b1f4-31cc6edefe01
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

### Disable auto link
{: #auto-link-cli}
{: cli}

You can disable auto link for the instance metadata service when provisioning an instance. Auto link is enabled by default when a trusted profile is selected. When autolink is enabled, the specified trusted profile is automatically linked to the virtual server instance when the instance is provisioned. An instance provisioned with auto link the trusted profile is available to the instance immediatley at startup. When auto link is disabled, the specified trusted profile must be linked to the instance for it to be used by the instance.

To disable auto link, set the `--default-trusted-profile-auto-link` option to `true` when provisioning an instance. The following example shows instance the provision command with auto link set to `false`.

```sh
ibmcloud is instance-create .... --default-trusted-profile "Profile-9fd84246-7df4-4667-94e4-8ecde51d5ac5" --default-trusted-profile-auto-link false
```

### Enable secure access by using the CLI
{: #secure-access-cli}
{: cli}

You can enable secure access to the instance metadata service. When secure access is enabled, the metadata service is accessible only to the virtual server instance by encrypted HTTP Secure protocol (HTTPS). When secure access is disabled the metadata service is accessible only to the virtual server instance by unencrypted HTTP protocol. Secure access is disabled by default.

Certain properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside an instance with secure access to the instance metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}.

#### Enable secure access when you provision an instance
{: #secure-access-cli-provision}
{: cli}

To enable secure access when you provision an instance, specify a value for the `--metadata-service-protocol` option when you use the `instance-create` command. For secure access specify `https`. The default setting is unencrypted `http`.

```sh
ibmcloud is instance-create INSTANCE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET ... [--metadata-service-protocol http | https] ...

--metadata-service-protocol value : The communication protocol to use for the metadata service
  endpoint. Applies only when the metadata service is enabled. One of: http, https. (default: "http")
```

To enable secure access on an existing instance, specify a value for the `protocol` sub-property of the `metadata service` when you use the `instance-update` command.

### Set the metadata hop limit by using the CLI
{: #set-hop-limit-cli}
{: cli}

You can set a hop limit for IP response packets from the metadata service by specifying a hop limit value between `1` (default) and `64` for the `Response hop limit` sub-property of the `Metadata service` property for your instance.

#### Set the metadata hop limit when you provision an instance
{: #set-hop-limit-cli-when-provisioning}
{: cli}

To set the metadata hop limit when you provision an instance, specify a hop limit value between `1` (default) and `64` for the `--metadata-service-response-hop-limit` option when you use the `instance-create` command.

```sh
ibmcloud is instance-create INSTANCE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET ... [--metadata-service-response-hop-limit METADATA-SERVICE-RESPONSE-HOP-LIMIT] ...

--metadata-service-response-hop-limit value : The hop limit (IP time to live) for IP response packets
  from the metadata service. (default: 1)
```

## Configure metadata settings by using the API
{: #metadata-config-api}
{: api}

You can enable and disable features of the metadata service by using the API.

### Disable auto link by using the API
{: #auto-link-api}
{: api}

You can disable auto link for the instance metadata service when provisioning an instance. Auto link is enabled automatically when a trusted profile is selected. When autolink is enabled, the specified trusted profile is automatically linked to the virtual server instance when the instance is provisioned. The trusted profile that is associated with an instance that is provisioned with auto link is available to the instance immediately at startup. When auto link is disabled, the specified trusted profile must be linked to the instance for it to be used by the instance.

To disable auto link by using the API, the `auto_link` value for the `default_trusted_profile` property must be set to `false`. The following example shows the `default_trusted_profile` property with the `auto_link` option disabled.

```json
"default_trusted_profile": {
      "auto_link": false,
   "target": {
       "id": "Profile-9fd84246-7df4-4667-94e4-8ecde51d5ac5"
   }
  },
```

### Enable secure access by using the API
{: #secure-access-api}
{: api}

You can enable secure access to the instance metadata service. When secure access is enabled, the metadata service is accessible only to the virtual server instance by encrypted HTTP Secure protocol (HTTPS). When secure access is disabled the metadata service is accessible only to the virtual server instance by unencrypted HTTP protocol. Secure access is disabled by default.
Certain properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside an instance with secure access to the instance metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}.

#### Enable secure access when you provision an instance
{: #secure-access-api-provision}
{: api}

To enable secure access, when you provision an instance with the [POST /instances](/apidocs/vpc/latest#create-instance) method, specify a value for the `metadata_service.protocol` property for your instance. For secure access specify `https`. The default setting is unencrypted `http`.

#### Enable secure access on an existing instance
{: #secure-access-api-existing}
{: api}

To enable secure access on an existing instance, use the [PATCH /instances/{id}](/apidocs/vpc/latest#update-instance) method to update the instance. Specify a value for the `metadata_service.protocol` property for your instance. For secure access specify `https`. The default setting is unencrypted `http`.

### Set the metadata hop limit by using the API
{: #set-hop-limit-api}
{: api}

You can set the hop limit for IP response packets from the metadata service using the `metadata_service.response_hop_limit` property

This property applies only when the metadata service is enabled by setting `metadata_service.enabled` to `true`. The default is `false`.

#### Set the metadata hop limit when you provision an instance
{: #set-hop-limit-api-when-provisioning}
{: api}

To set the response when you provision an instance, call the [POST /instances method](/apidocs/vpc/latest#create-instance) method, and specify the `metadata_service.response_hop_limit` property value between `1` (default) and `64`.

This property applies only when the metadata service is enabled by setting `metadata_service.enabled` to `true`. The default is `false`.

#### Set the metadata hop limit on an existing instance
{: #set-hop-limit-api-on-existing-instance}
{: api}

To set the response hop limit on an existing instance, call the [PATCH /instances/{id}](/apidocs/vpc/latest#update-instance) method, and specify the `metadata_service.response_hop_limit` property value between `1` (default) and `64`.

## Activity Tracker events for instance metadata
{: #imd-at-events}
{: ui}

Activity Tracker events are triggered when you get an [instance access identity token](#imd-json-token) and then [use the service](/docs/vpc?topic=vpc-imd-get-metadata). For more information about these events, see [Instance Metadata service events](/docs/vpc?topic=vpc-at_events#events-metadata).

## Next steps
{: #imd-token-next}

After you create an instance identity access token and enable the metadata service, you can retrieve metadata for the instance, SSH keys, and placement groups. For more information, see [Use the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
