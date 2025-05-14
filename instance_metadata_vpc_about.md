---

copyright:
  years: 2022, 2025
lastupdated: "2025-05-14"

keywords:

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# About VPC Instance Metadata
{: #imd-about}

The VPC Instance Metadata service is a free service that you can use to access information about your virtual server instances. You can invoke the service through a REST API within an instance to get information about that instance. Access to the API is unavailable from outside the instance. Before you can access the metadata, you need to generate an instance identity access token. You can use the instance identity token to optionally get an IAM token that can be used to access all IAM-enabled services.
{: shortdesc}

## Metadata service concepts
{: #imd-concepts}

You can programmatically access metadata about a virtual server instance and use it to initialize new instances. Metadata that is provided by API services pertains only to the instance from which the request is made. You can't use the metadata service within an instance to obtain information about another instance or to obtain information about resources that are not currently associated with the instance.

Metadata that you can access includes the instance name, CRN, resource groups, user data, as well as information about SSH keys and placement groups. For more information about all metadata that is returned from the service, see the [summary of data returned by the metadata service](/docs/vpc?topic=vpc-imd-metadata-summary).

The instance metadata service uses a REST API and the instance's host name to retrieve metadata. The metadata service is disabled by default. You can enable the metadata services on all instances with the VPC API, from the CLI, or in the console.

The instance metadata service has two components:

* **[Instance identity token service](#imd-vpc-access-token)** that lets you acquire a JSON web token to access the VPC metadata service. To access other IBM Cloud IAM-enabled services, you can generate an IAM token from the instance identity token. For example, starting an instance might require a call to IBM Cloud Database to set up a database server.

* **[Instance metadata service](#imd-service)** that lets make API calls to retrieve the instance metadata. By calling the metadata service APIs, you can get instance initialization data, network interface, volume attachment, public SSH key, and placement group information.

### Instance identity token service
{: #imd-vpc-access-token}

With the instance identity token service, you can generate an instance identity access token to access the metadata service. You generate it by calling the endpoint: `http://api.metadata.cloud.ibm.com/instance_identity/v1/token`, or `https://api.metadata.cloud.ibm.com/instance_identity/v1/token` if secure communication is enabled. This endpoint is accessible to all commands, processes, and software applications that are running within the virtual server instance. Access to the API endpoint is not available outside of the virtual server instance.

Use the instance identity access token when you call the metadata service. The token is valid until it reaches expiration, which is 5 minutes by default. For more information, see [Acquire an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service#imd-json-token).

You can also generate an IAM token from the instance identity access token. This IAM token can be used to access all IAM-enabled services that are based on access rights and policy settings for IAM trusted profiles linked to the instance. It has its own expiration date, based on the IAM token service default. [Generate an IAM token from an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange).

### Metadata service
{: #imd-service}

You can enable the metadata service on all new instances to retrieve data about the instances.

Metadata is obtained by calling REST APIs that provide instance-specific information. You make `GET` calls to endpoints that are accessible within a running virtual server instance. You can retrieve metadata about instances, keys, and placement groups by making calls to invoke the hostname:

* `https://api.metadata.cloud.ibm.com/metadata/v1/instance`
* `https://api.metadata.cloud.ibm.com/metadata/v1/keys`
* `https://api.metadata.cloud.ibm.com/metadata/v1/placement_groups`

For more information about all the endpoints that you can use to access instance metadata, see [Summary of data returned by the metadata service](/docs/vpc?topic=vpc-imd-metadata-summary).
{: tip}

You [enable the metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-metadata-service-enable) by setting a toggle in the VPC UI, CLI, or API. You can enable the service when you create an instance or update an existing instance.

The metadata service intercepts all requests to the service's IP, and then routes them to the specific services to handle these requests. As part of the request to the metadata service, you have to include the instance identity access token.

#### Compute resource identities
{: #imd-comp-res-identity}

Through IAM, you can also assign access rights to instances by creating a [compute resource identity](/docs/vpc?topic=vpc-imd-trusted-profile-metadata), and then configure access rights to IBM-enabled services that are using that identity.

The compute resource identity service creates a trusted profile, against which you can assign access rights to enable the instance to call IAM-enabled services, such as {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.keymanagementservicefull_notm}}. You create a trusted profile within the virtual server instance. Trusted profiles define authorization for all applications that are running on the instance.

#### User data
{: #imd-user-data}

The metadata that you access from the instance includes _user data_. The user data is data that you specified when you provisioned the instance, and that is available when you provision other instances. (It's the same user data that is available from cloud-init for VPC instances.) For example, information to load database software, custom software for worker nodes, or information to make decisions about how to initialize the instance are provided in user data. For more information, see the topic on [User data](/docs/vpc?topic=vpc-user-data).

The metadata that you access from within the instance is not protected by any encryption method. Any user with access to the instance and metadata service can potentially see the metadata. As a precaution, do not store sensitive data, such as passwords or customer encryption keys, as user data.
{: important}

## Scenarios for using the metadata service
{: #imd-metadata-scenarios}

You can use the metadata service in two ways:

* Access the metadata from within the instance.

    In this scenario, you retrieve metadata from within the instance to bootstrap the instance. For example, you might want to specify custom user data when you create the instance and then, retrieve that custom user data when the instance is initialized. For more information, see [Accessing metadata from an instance](/docs/vpc?topic=vpc-imd-access-instance-metadata).

* Access an instance identity IAM token and call IAM-enabled services.

    In this scenario, you create a trusted profile for a compute resource identity and assign access rights for IAM-enabled services to a virtual server instance. Make an API request to create a new instance, which is configured with the metadata service, then link the instance to the trusted profile. Call the metadata service to get an instance identity access token, then [generate an IAM token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange) from that token. You can then call IAM-enabled services from the instance. Use this option when you want to call IAM-enabled services as part of instance bootstrapping. For example, you might want to set up a new {{site.data.keyword.cos_short}} bucket to be used by the instance workload. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

Figure 1 illustrates these scenarios.

![Figure showing the instance metadata user workflows.](/images/metadata-service-user-workflow.png "Figure showing the instance metadata user workflows."){: caption="User scenarios for the instance metadata service" caption-side="bottom"}

## Data security
{: #imd-security}

IBM takes precaution to assure your data is secure. Metadata is retrieved only from the instance to which you have access. Data that is common to multiple instances is retrieved but specific IP addresses of other instances are not exposed.

The default protocol for the metadata service is HTTP. Calls to the metadata service are secure before they leave the compute host from which they originate. Data is wrapped by using a secure protocol before the data leaves the compute host. The initial call to the instance metadata service is over an HTTP connection. The purpose of the call is to acquire an identity token to access the metadata service. These services use a well-known URL. Although you see a nonsecure connection, the VPC compute service takes extra actions to secure the token and metadata services.

The metadata service can also be configured to use HTTP Secure protocol (HTTPS). For more information, see [Enable secure access by using the UI](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#secure-access-ui).

The instance metadata service is not intended for sensitive data. The service end-point is open to all processes on the instance. Information that is exposed through this service is to be considered as shared information to all applications that are running inside the virtual server instance.
{: note}

For more information about extra security measures that you can take, see [Security best practices for the Instance Metadata service](/docs/vpc?topic=vpc-imd-security-best-practices).

## Next steps
{: #imd-next-steps-about}

* [Create an instance identity access token for accessing the metadata service](/docs/vpc?topic=vpc-imd-configure-service#imd-get-token).

* [Retrieve data by using the metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
