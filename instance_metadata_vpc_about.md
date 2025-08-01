---

copyright:
  years: 2022, 2025
lastupdated: "2025-08-01"

keywords:

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# About {{site.data.keyword.vpc_full}} (VPC) Metadata on virtual server instances
{: #imd-about}

The VPC Metadata service is a free service that you can use to access information about your virtual server instances. You can invoke the service through a REST API within an instance to get information about that instance. Access to the API is unavailable from outside the instance. Before you can access the metadata, you need to generate an instance identity access token. You can use the instance identity token to optionally get an IAM token that can be used to access all IAM-enabled services.
{: shortdesc}

## Metadata service concepts
{: #imd-concepts}

You can programmatically access metadata about a virtual server instance and use it to initialize new instances. Metadata that is provided by API services pertains only to the instance from which the request is made. You can't use the metadata service within an instance to obtain information about another instance or to obtain information about resources that are not currently associated with the instance.

Metadata that you can access includes the instance name, CRN, resource groups, user data, as well as information about SSH keys and placement groups. For more information about all metadata that is returned from the service, see the [summary of data returned by the metadata service](/docs/vpc?topic=vpc-imd-metadata-summary).

The metadata service uses a REST API and the instance's host name to retrieve metadata. The metadata service is disabled by default. You can enable the metadata services on all instances with the VPC API, from the CLI, or in the console.

The metadata service has two components:

* **Identity API**: The service provides identity tokens, identity certificates, and IAM access tokens. You need an instance identity token to acess and retrieve metadata information. To access other {{site.data.keyword.cloud_notm}} IAM-enabled services, you can generate an IAM token from the instance identity token. For example, starting an instance might require a call to {{site.data.keyword.cloud_notm}} Database to set up a database server. Lastly, an identity certificate is used to establish in-transit encryption between the virtual server instance and a mounted file share.

* **Metadata API** provides access to the instance's metadata. By calling the metadata service APIs, you can get instance initialization data, network interface, volume attachment, public SSH key, and placement group information.

![Figure showing the metadata user workflows.](/images/metadata-service-user-workflow.png "Figure showing the metadata user workflows."){: caption="User scenarios for the metadata service" caption-side="bottom"}

### Instance identity API 
{: #imd-vpc-access-token}

With the instance identity API, you can generate an instance identity access token to access the metadata service. You generate the token by calling the endpoint: `http://api.metadata.cloud.ibm.com/instance_identity/v1/token`, or `https://api.metadata.cloud.ibm.com/instance_identity/v1/token` if secure communication is enabled. 

This endpoint is accessible to all commands, processes, and software applications that are running within the virtual server instance. Access to the API endpoint is not available outside of the virtual server instance.

Use the instance identity access token when you call the metadata service. The token is valid until it reaches expiration, which is 5 minutes by default. For more information, see [Acquire an instance identity access token](/docs/vpc?topic=vpc-imd-identity-operations#imd-json-token).

You can also generate an IAM token from the instance identity access token. This IAM token can be used to access all IAM-enabled services that are based on access rights and policy settings for IAM trusted profiles linked to the instance. It has its own expiration date, based on the IAM token service default. [Generate an IAM token from an instance identity access token](/docs/vpc?topic=vpc-imd-identity-operations#imd-token-exchange).

### Metadata service
{: #imd-service}

You can enable the metadata service on existing and new instances to retrieve data about the instances. You can [enable the metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-metadata-service-enable) in the console, from the CLI, or with the API.

Metadata is obtained by calling REST APIs that provide instance-specific information. You make `GET` requests to endpoints that are accessible within a running virtual server instance. You can retrieve metadata about instances, keys, and placement groups by using the following endpoints:

* `https://api.metadata.cloud.ibm.com/metadata/v1/instance`
* `https://api.metadata.cloud.ibm.com/metadata/v1/keys`
* `https://api.metadata.cloud.ibm.com/metadata/v1/placement_groups`

For more information about all the endpoints that you can use to access metadata, see [Summary of data returned by the metadata service](/docs/vpc?topic=vpc-imd-metadata-summary).
{: tip}

The metadata service intercepts all requests to the service's IP, and then routes them to the specific services to handle these requests. As part of the request to the metadata service, you must include the instance identity access token.

#### Compute resource identities
{: #imd-comp-res-identity}

A Trusted Profile in {{site.data.keyword.cloud_notm}} is an identity within a specific account that serves as a gateway for federated users or compute resources to work within that account without the need for an API key. Compute resource identity refers to the ability to use a trusted profile for fine-grained authorization for applications running in compute resources.

Trusted profiles can be members of IAM access groups and have assigned access privileges, and can also have direct access assigned to them. They are particularly useful for administrative work with a given set of privileges under a specific identity for identities or resources identified by a set of properties configured as part of the trusted profile.

The trusted profile is created through the {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) service. Specifically, it is done by navigating to **Manage > Access (IAM)** and selecting **Trusted profiles**, then clicking **Create profile**. For more information, see [Creating trusted profiles](/docs/account?topic=account-create-trusted-profile).

Using trusted profiles eliminates the need to create service IDs or API keys for the compute resources. It allows applications to access {{site.data.keyword.cloud_notm}} services without requiring individual authentication for each compute resource.

#### User data
{: #imd-user-data}

The metadata that you access from the instance includes _user data_. The user data is data that you specified when you provisioned the instance, and that is available when you provision other instances. (It's the same user data that is available from cloud-init for VPC instances.) For example, information to load database software, custom software for worker nodes, or information to make decisions about how to initialize the instance are provided in user data. For more information, see the topic on [User data](/docs/vpc?topic=vpc-user-data).

The metadata that you access from within the instance is not protected by any encryption method. Any user with access to the instance and metadata service can potentially see the metadata. As a precaution, do not store sensitive data, such as passwords or customer encryption keys, as user data.
{: important}

## Data security
{: #imd-security}

IBM takes precaution to assure your data is secure. Metadata is retrieved only from the instance to which you have access. Data that is common to multiple instances is retrieved but specific IP addresses of other instances are not exposed.

The default protocol for the metadata service is HTTP. Calls to the metadata service are secure before they leave the compute host from which they originate. Data is wrapped by using a secure protocol before the data leaves the compute host. The initial call to the metadata service is over an HTTP connection. The purpose of the call is to acquire an identity token to access the metadata service. These services use a link-local URL. Although you see a nonsecure connection, the VPC compute service takes extra actions to secure the token and metadata services.

The metadata service can also be configured to use HTTP Secure protocol (HTTPS). For more information, see [Enable secure access by using the UI](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#secure-access-ui).

The metadata service is not intended for sensitive data. The service end-point is open to all processes on the instance. Information that is exposed through this service is to be considered as shared information to all applications that are running inside the virtual server instance.
{: note}

For more information about extra security measures that you can take, see [Security best practices for the metadata service](/docs/vpc?topic=vpc-imd-security-best-practices).

## Activity tracking events
{: #imd-auditing}

When you make API requests to the metadata service, events are triggered that can be used for auditing. For more information, see [Metadata service events](/docs/vpc?topic=vpc-at_events#events-metadata).

## Next steps
{: #imd-next-steps-about}

* [Create an instance identity access token for accessing the metadata service](/docs/vpc?topic=vpc-imd-identity-operations).

* [Retrieve instance-specific data by using the metadata service](/docs/vpc?topic=vpc-imd-access-instance-metadata).

* [Using the metadata service to identify disks in your virtual server instance](https://www.ibm.com/products/tutorials/using-the-metadata-service-to-identify-disks-in-your-vsi-with-ibm-cloud-vpc){: external}.
