---

copyright:
  years: 2022, 2025
lastupdated: "2025-07-31"

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for the metadata service
{: #faqs-for-rmds}

The following questions often arise about the VPC metadata service. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links.
{: shortdesc}

## What is the metadata service?
{: #faq-rmds-1}
{: faq}

The metadata service provides REST APIs that you can call within an instance to get information about that instance at no cost. Access to the API is unavailable from outside the instance. Before you can access the metadata, you must generate an instance identity access token for accessing the metadata service. You can optionally get an IAM token from this token to access all IAM-enabled services.

The metadata service uses link-local IP address to retrieve metadata such as the instance name, CRN, resource groups, user data, as well as SSH key and placement group information. You can use the initialization metadata to configure and start new instances. For more information, see [About metadata for VPC](/docs/vpc?topic=vpc-imd-about).

The VPC metadata service is supported only on x86 systems.

## How does the metadata service work?
{: #faq-rmds-2}
{: faq}

By calling the [metadata service APIs](/apidocs/vpc-metadata-beta) from within an instance, you can get the instance's initialization data, network interface, volume attachment, public SSH key, and placement group information.

To use the metadata service, you need an instance identity access token. By using the instance identity token service, you can access the metadata service. For more information, see this [FAQ](#faq-rmds-4).

The metadata service is disabled by default. You can enable it for new and existing instances in the console, from the CLI, or with the API.

## How do I enable the metadata service? Can I later disable it?
{: #faq-rdms-6}

The metadata service is disabled by default. You can enable it on new and existing instances in the console, from the CLI, or with the API. For more information, see [Enable or disable the metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=cli#imd-metadata-service-enable).

When you enable the metadata service for a virtual server instance that was created with the metadata service disabled, you must stop and restart the instance to fully enable the metadata service.
{: note}

## What information can I get from the metadata service?
{: #faq-rmds-3}
{: faq}

The metadata service provides information about your running virtual server instance: instance initialization data, network interface, volume attachment, public SSH key, and placement group information. For a complete list of all information provided, see [Summary of data that is returned by the metadata service](/docs/vpc?topic=vpc-imd-metadata-summary).

## What is the Identity API?
{: #faq-rmds-4}
{: faq}

You use the identity API to generate an identity access token that provides a security credential for accessing the metadata. To interact with the identity service, you make a REST API call to the service by using a link-local, nonroutable IP address. You access the token from within the instance. For more information, see [Instance identity API](/docs/vpc?topic=vpc-imd-about#imd-vpc-access-token).

You can also generate an IAM token from the instance identity access token, and then use the IAM token to access all IAM-enabled services. For more information, see [Generate an IAM token from an instance identity access token](/docs/vpc?topic=vpc-imd-identity-operations#imd-token-exchange).

## How do I get metadata for booting up a new instance?
{: #faq-rdms-5}

Make a `GET "http://api.metadata.cloud.ibm.com/metadata/v1/instance/initialization"` request to retrieve initialization information for the instance. For more information about instance initialization, see [Retrieving metadata from an instance](/docs/vpc?topic=vpc-imd-access-instance-metadata).

## How secure is the metadata service? Are there any extra security measures that I need to take?
{: #faq-rdms-7}

Metadata is retrieved only from the instance to which you have access. Data common to multiple instances is retrieved but specific IP addresses of other instances are not exposed. Further, calls to the metadata service are secure on the originating compute host. While access is over HTTP, data is wrapped into a secure protocol before it leaves the compute host. For more information, see [data security](/docs/vpc?topic=vpc-imd-about#imd-security) and [security best practices](/docs/vpc?topic=vpc-imd-security-best-practices).

## What is a compute resource identity and how does it relate to the metadata service?
{: #faq-rdms-9}

Compute resource identities assign an IBM Cloud IAM identity to an individual compute resource. Instead of creating a service ID, generating an API key, then storing and validating that key, you can assign an IAM identity directly to the instance and acquire access tokens.

## What is a trusted profile?
{: #faq-rdms-trusted-profile}

A trusted profile is an IAM object that is created by the compute resource identity service, against which you assign access rights to enable the instance to call IAM-enabled service. You can use a trusted profile when you're accessing the metadata service. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Can I use the link-local IP address and the fully qualified domain name interchangeably?
{: #faq-link-local}

The link-local address of the metadata service is `169.254.169.254`. However, when the API request to the metadata service is made, it is preferred to use `api.metadata.cloud.ibm.com`. When secure access to the metadata service is enabled, the use of `api.metadata.cloud.ibm.com` is required.
