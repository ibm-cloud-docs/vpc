---

copyright:
  years: 2025, 2025
lastupdated: "2025-11-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using the metadata service for bare metal servers
{: #get-metadata-bare-metal}

After you obtain a bare metal server identity access token, you can access the metadata service and retrieve metadata about a bare metal server. The following information describes how to make calls to the API to access bare metal server metadata such as initialization data, network interfaces, volume attachments, public SSH keys, and placement groups.
{: shortdesc}

When you make API calls to the bare metal server metadata service, activity tracking events are triggered. For more information, see [Bare metal server events](/docs/vpc?topic=vpc-at_events#events-compute-bm).

## Before you begin
{: #metadata-prereqs-bare-metal}

To access metadata service, you must have a bare metal server identity access token. If you don't have one, see [Acquire a bare metal server identity access token](/docs/vpc?topic=vpc-get-metadata-bare-metal&interface=api#metadata-token-exchange-usemd-bare-metal).

The metadata service is disabled by default. To enable it, see [Enable or disable the metadata service](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-service-enable-bare-metal).

For more information about these API calls and examples, see the [Metadata service API reference](/apidocs/vpc-identity-beta).

Windows users have extra requirements to access and use the metadata service. For more information, see [Setting up windows servers for using the metadata service](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-service-enable-bare-metal).
{: note}

## Accessing the metadata service by using the bare metal server identity access token service
{: #metadata-get-token-usemd-bare-metal}

To access the bare metal server metadata service, you must first obtain a bare metal server identity access token. You can later generate IAM tokens from the bare metal server identity access token and then use them to access IAM-enabled services.

Windows users have extra requirements to access and use the metadata service. For more information, see [Setting up windows servers for using the metadata service](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-service-enable-bare-metal).
{: note}

### Bare metal server identity access token concepts
{: #metadata-token-concepts-usemd-bare-metal}

A bare metal server identity access token provides a security credential for accessing the metadata service. It's a signed token with a set of claims based on information about the bare metal server and information that is passed in the token request.

To access the bare metal server identity, make a `PUT "http://api.metadata.cloud.ibm.com /identity/v1/token` call by using the [Metadata service API](/apidocs/vpc-identity-beta/initial#create-access-token) that invokes the bare metal server hostname. Communication between the bare metal server and metadata service never departs the host. You acquire the token from within the bare metal server. If secure access to the bare metal server metadata service is enabled on your bare metal server, use the `https` protocol instead of the `http` protocol.

In the request, you specify an expiration time for the token. The default is 5 minutes, but you can specify that it expires sooner or later (5 seconds to 1 hour).

The response (a JSON payload) contains the bare metal server identity access token. Use this token to access the metadata service.

You can also generate an IAM token from this token and use the API to call IAM-enabled services. For more information, see [Generate an IAM token from a bare metal server identity access token](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-token-exchange-bare-metal).

### Acquiring a bare metal server identity access token
{: #metadata-json-token-usemd-bare-metal}

Using the Metadata service API, make `PUT "http://api.metadata.cloud.ibm.com /identity/v1/token` call to get a bare metal server identity access token. The following example uses `jq` to parse the JSON API response and then extract the bare metal server identity access token value. You can use your preferred JSON parser.

In the example, the return value of the CURL command is the bare metal server identity access token, which is extracted by `jq` and placed in the `identity_token` environment variable. You use specify this variable in a `GET` call to the metadata service to reach the metadata endpoint. For more information, see [Retrieve metadata from your running bare metal server](/docs/vpc?topic=vpc-get-metadata-bare-metal&interface=api).

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create a bare metal server. You might need to install `jq` before use or use any parser of your choice.
{: note}

```json
identity_token=`curl -X PUT "http://api.metadata.cloud.ibm.com /identity/v1/token?version=2025-06-30"\
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
  "created_at": "2022-08-08T11:08:39.363Z",
  "expires_at": "2022-08-08T11:13:39.363Z",
  "expires_in": 3600
}
```
{: codeblock}

## Generating an IAM token from a bare metal server identity access token
{: #metadata-token-exchange-usemd-bare-metal}
{: api}

To access {{site.data.keyword.cloud_notm}} IAM-enabled services in the account, you can generate an IAM token from the bare metal server identity access token by using trusted profile information. After you generate the IAM token, you can use it to access IAM-enabled services, such as {{site.data.keyword.cos_full_notm}}, Cloud Database Service, and the VPC APIs. You can reuse the token multiple times.

Make a `POST /identity/v1/iam_tokens` call and specify the ID of the trusted profile. This request uses the bare metal server identity access token and a trusted profile that is linked to a bare metal server to generate an IAM access token. The trusted profile can be linked either when you create the bare metal server or provided in the request body.

The IAM API used to pass the bare metal server identity access token and generate an IAM token is being deprecated. Beta users must migrate to the metadata service API to generate an IAM token by using `POST /identity/v1/iam_tokens`.
{: note}

Example request:

```json
iam_token=`curl -X POST "http://api.metadata.cloud.ibm.com/identity/v1/iam_tokens?version=2025-06-30"\
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
  "created_at": "2022-08-08T14:10:15Z",
  "expires_at": "2022-08-08T15:10:15Z",
  "expires_in": 3600
}
```
{: codeblock}

For more information about trusted profiles, see [Using a trusted profile to call IAM-enabled services for bare metal servers](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-service-enable-bare-metal).

## Create a trusted profile for the bare metal server
{: #metadata-trusted-profile-config-usemd-bare-metal}

Trusted profiles for compute resource identities help you assign an {{site.data.keyword.cloud}} IAM identity to an {{site.data.keyword.cloud}} resource, such as a bare metal server. You can call any IAM-enabled service from a bare metal server without having to manage and distribute IAM secrets to the bare metal server. You can create a trusted profile when you generate an IAM token from a bare metal server identity access token and link it to the bare metal server. For more information, see [Using a trusted profile to call IAM-enabled services for bare metal servers](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-service-enable-bare-metal).

## Next steps
{: #metadata-next-steps-md-bare-metal}

[Create and use a trusted profile](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-service-enable-bare-metal) to access IAM-enabled services.
