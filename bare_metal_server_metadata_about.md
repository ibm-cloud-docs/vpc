---

copyright:
  years: 2025, 2025
lastupdated: "2025-08-29"

keywords:

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# About {{site.data.keyword.vpc_full}} (VPC) Metadata on bare metal servers
{: #bare-metal-server-metadata-about}

The {{site.data.keyword.vpc_full}} (VPC) Metadata service on bare metal servers is a free service you can invoke through a REST API within a bare metal server to get bare metal server identity tokens and bare metal server identity certificates.
{: shortdesc}

 Access to the API is unavailable from outside the bare metal server. Before you can access the metadata, you need to generate a bare metal server identity access token. You can use the bare metal server identity token to optionally get an IAM token that can be used to access all IAM-enabled services. For information about the REST API, see [VPC Identity API](/apidocs/vpc-identity) documentation.

## Metadata service concepts
{: #metadata-concepts-bare-metal}

You can programmatically access metadata about a bare metal server and use it to initialize new bare metal servers. Metadata that is provided by API services pertains only to the bare metal server from which the request is made. You can't use the metadata service within a bare metal server to obtain information about another bare metal server or to obtain information about resources that are not currently associated with the bare metal server.

Metadata that you can access is the bare metal server identity tokens and bare metal server identity certificates. The metadata service is disabled by default. You can enable the metadata services on all bare metal servers with the VPC API, from the CLI, or in the console.

### Bare metal server identity token service
{: #metadata-access-token-bare-metal}

Bare metal server identity token service lets you acquire a JSON web token to access the VPC metadata service. To access other IBM Cloud IAM-enabled services, you can generate an IAM token from the bare metal server identity token. For example, starting a bare metal server might require a call to IBM Cloud Database to set up a database server.

With the bare metal server identity token service, you can generate a bare metal server identity access token to access the metadata service. You generate it by calling the endpoint: `http://api.metadata.cloud.ibm.com/identity/v1/token`, or `https://api.metadata.cloud.ibm.com/identity/v1/token` if secure communication is enabled. This endpoint is accessible to all commands, processes, and software applications that are running within the bare metal server. Access to the API endpoint is not available outside of the bare metal server.

Use the bare metal server identity access token when you call the metadata service. The token is valid until it reaches expiration, which is 5 minutes by default. For more information, see [Acquire a bare metal server identity access token](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-json-token-bare-metal)

You can also generate an IAM token from the bare metal server identity access token. This IAM token can be used to access all IAM-enabled services that are based on access rights and policy settings for IAM trusted profiles linked to the bare metal server. It has its own expiration date, based on the IAM token service default. [Generate an IAM token from a bare metal server identity access token](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-token-exchange-bare-metal).

#### Compute resource identities
{: #metadata-comp-res-identity-bare-metal}

Through IAM, you can also assign access rights to bare metal servers by creating a [compute resource identity](/docs/vpc?topic=vpc-metadata-trusted-profile-bare-metal&interface=api#metadata-compute-res-identity-bare-metal), and then configure access rights to IBM-enabled services that are using that identity.

The compute resource identity service creates a trusted profile, against which you can assign access rights to enable the bare metal server to call IAM-enabled services, such as {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.keymanagementservicefull_notm}}. You create a trusted profile within the bare metal server. Trusted profiles define authorization for all applications that are running on the bare metal server.

## Data security
{: #metadata-security-bare-metal}

IBM takes precaution to assure your data is secure. The bare metal server identity token service is retrieved only from the bare metal server to which you have access. Data that is common to multiple bare metal servers is retrieved but specific IP addresses of other bare metal servers are not exposed.

The default protocol for the bare metal server identity token service is HTTP. Calls to the bare metal server identity token service are secure before they leave the compute host from which they originate. Data is wrapped by using a secure protocol before the data leaves the compute host. The initial call to the bare metal server identity token service is over an HTTP connection. The purpose of the call is to acquire an identity token to access the mbare metal server identity token service. These services use a well-known URL. Although you see a nonsecure connection, the VPC compute service takes extra actions to secure the token and bare metal server identity token service.

The bare metal server identity token service can also be configured to use HTTP Secure protocol (HTTPS). For more information, see [Enable secure access by using the UI](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-service-enable-bare-metal).

The bare metal server identity token service is not intended for sensitive data. The service end-point is open to all processes on the bare metal server. Information that is exposed through this service is to be considered as shared information to all applications that are running inside the bare metal server.
{: note}

For more information about extra security measures that you can take, see [Security best practices for the metadata service for bare metal servers](/docs/vpc?topic=vpc-metadata-security-best-practices-bare-metal).

## Next steps
{: #metadata-next-steps-about}

[Create an bare metal server identity access token for accessing the metadata service](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal#metadata-get-token-bare-metal).
