---

copyright:
  years: 2022
lastupdated: "2022-11-08"

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# FAQs for the instance metadata service
{: #faqs-for-rmds}

The following questions often arise about the VPC Instance Metadata service. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links.
{: shortdesc}

## What is the instance metadata service?
{: #faq-rmds-1}
{: faq}

The instance metadata service is free service that provides a REST API you invoke within an instance to get information about that instance. Access to the API is unavailable from outside the instance. Before you can access the metadata, the service lets you generate an instance identity access token, for accessing the metadata service. You can optionally get an IAM token from this token to access all IAM-enabled services.

The metadata service uses well-known IP address to retrieve instance metadata such as the instance name, CRN, resource groups, user data, as well as SSH key and placement group information. Use the initialization metadata to configure and launch new instances. For more information, see [About Instance Metadata for VPC](/docs/vpc?topic=vpc-imd-about).

The VPC Instance Metadata service is currently supported only on x86 systems.

## How does the metadata service work?
{: #faq-rmds-2}
{: faq}

By calling the [metadata service APIs](/apidocs/vpc-metadata-beta) from within an instance, you can get the instance's initialization data, network interface, volume attachment, public SSH key, and placement group information.

To use the metadata service, you need an instance identity access token. The instance identity token service lets you access the metadata service. For more information, see this [FAQ](#faq-rmds-4).

The metadata service is disabled by default. You can enable it for new and existing instances in the UI, CLI, or API.

## How do I enable the metadata service? Can I later disable it?
{: #faq-rdms-6}

The metadata service is disabled by default. You can enable it on new and existing instances from the UI, CLI, or API. For more information, see [Enable or disable the instance metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=cli#imd-metadata-service-enable).

For this release, for instances created with metadata services disabled at time of creation and later updated to enable the metadata service, you must stop and restart the instance to fully enable the metadata service.
{: note}

## What information can I get from the metadata service?
{: #faq-rmds-3}
{: faq}

The metadata service provides information about your running virtual server instance: instance initialization data, network interface, volume attachment, public SSH key, and placement group information. For a complete list of all information provided, see [Summary of data returned by the metadata service](/docs/vpc?topic=vpc-imd-metadata-summary).

## What is the instance identity token service?
{: #faq-rmds-4}
{: faq}

The instance identity token service lets you generate an instance identity access token that provides a security credential for accessing the metadata. To interact with the instance identity token service, you make a REST API call to the service using a well-known, non-routable IP address. You access the token from within the instance. For more information, see [Instance identity token service](/docs/vpc?topic=vpc-imd-about#imd-vpc-access-token).

You can also generate an IAM token from the instance identity access token, and then use the IAM token to access all IAM-enabled services. For more information, see [Generate an IAM token from an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=cli#imd-token-exchange).

## How do I get metadata for booting up a new instance?
{: #faq-rdms-5}

Make a `GET "http://169.254.169.254/metadata/v1/instance/initialization"` call to retrieve initialization information for the instance. For more information about instance initialization, see [Retrieve instance metadata from your running virtual server instance](/docs/vpc?topic=vpc-imd-get-metadata#imd-retrieve-instance-data). For an end-to-end procedure, also see [Accessing metadata from an instance](/docs/vpc?topic=vpc-imd-access-instance-metadata).

## How secure is the metadata service? Are their additional security measures I need to take?
{: #faq-rdms-7}

Metadata is retrieved only from the instance to which you have access. Data common to multiple instances is retrieved but specific IP addresses of other instances are not exposed. Further, calls to the metadata service are secure prior to leaving the compute host from which they originate. While access is over HTTP, data is wrapped into a secure protocol prior to leaving the compute host. For more information, see this section on [data security](/docs/vpc?topic=vpc-imd-about#imd-security) and these additional [security best practices](/docs/vpc?topic=vpc-imd-security-best-practices).

## What is a compute resource identity and how does it relate to the metadata service?
{: #faq-rdms-9}

Compute resource identities assign an IBM Cloud IAM identity to an individual compute resource. Instead of having to create a service ID, generate an API key, get the application to store and validate that key, you can assign IAM identity directly to the instance and acquire access tokens.

## What is a trusted profile?
{: #faq-rdms-trusted-profile}

A trusted profile is an IAM object created by the compute resource identity service, against which you assign access rights to enable the instance to call IAM-enabled service. You can use a trusted profile when accessing the instance metadata service. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).
