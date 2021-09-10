---

copyright:
  years: 2021
lastupdated: "2021-09-10"

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}
{:beta .beta}

# FAQs for the instance metadata service (Beta)
{: #faqs-for-rmds}

The instance metadata service is available only to accounts with special approval to preview this beta feature.
{: beta}

## What is the instance metadata service?
{: #faq-rmds-1}
{: faq}

The instance metadata service is free service that uses a REST API that you invoke to get information about your running virtual server instance. Before you can access the metadata, the service lets you generate an access token, which is an instance identity token providing access to the metadata service. You can optionally exchange this token for an IAM token to access all IAM-enabled services. 

The metadata service uses well-known IP address to retrieve instance metadata such as the instance name, CRN, resource groups, user data, as well as SSH key and placement group information. Use the initialization metadata to configure and launch new instances. For more information, see [About Instance Metadata for VPC](/docs/vpc?topic=vpc-imd-about).

## How does the metadata service work?
{: #faq-rmds-2}
{: faq}

By calling the [metadata service APIs](/apidocs/vpc-metadata-beta), you can get instance initialization data, network interface, volume attachment, public SSH key, and placement group information.

To use the metadata service, you need an instance identity access token. The instance identity token service lets you access the metadata service. For more information, see this [FAQ](#faq-rmds-4).

## What information can I get from the metadata service?
{: #faq-rmds-3}
{: faq}

The metadata service provides information about your running virtual server instance: instance initialization data, network interface, volume attachment, public SSH key, and placement group information. For a complete list of all information provided, see [Summary of data returned by the metadata service](/docs/vpc?topic=vpc-imd-metadata-summary).

## What is the instance identity token service? 
{: #faq-rmds-4}
{: faq}

The instance identity token service lets you generate an access token that provides a security credential for accessing the metadata. To interact with the instance identity token service, you make a REST API call to the service using a well-known, non-routable IP address. You access the token from within the instance. For more information, see [Instance identity token service](/docs/vpc?topic=vpc-imd-about#imd-vpc-access-token).

## How do I get metadata for booting up a new instance?
{: #faq-rdms-5}

Make a `GET "http://169.254.169.254/metadata/v1/instance/initialization"` call to retrieve initialization information for the instance. For more information about instance initialization, see [Retrieve instance metadata from your running virtual server instance](/docs/vpc?topic=vpc-imd-get-metadata#imd-retrieve-instance-data). For an end-to-end procedure, also see [Accessing metadata from an instance](/docs/vpc?topic=vpc-imd-access-instance-metadata).

## How do I enable the metadata service? Can I later disable it?
{: #faq-rdms-6}

For Beta, the metadata service is enabled for instances you create on accounts authorized to use the service. You can disable the service by making a `PATCH /instance` call using the VPC API and setting `enabled` parameter to `false`. For information, see [Enable or disable the instance metadata service](/docs/vpc?topic=vpc-imd-configure-service#imd-metadata-service-enable).

## How secure is the metadata service? Are their additional security measures I need to take?
{: #faq-rdms-7}

Metadata is retrieved only from the instance to which you have access. Data common to multiple instances is retrieved but specific IP addresses of other instances are not exposed. Further, calls to the metadata service are secure prior to leaving the compute host from which they originate. While access is over HTTP, data is wrapped into a secure protocol prior to leaving the compute host. For more information, see this section on [data security](/docs/vpc?topic=vpc-imd-about#imd-security) and these additional [security best practices](/docs/vpc?topic=vpc-imd-security-best-practices).

## Can I access metadata about an instance from the UI?
{: #faq-rdms-8}

The Beta release of the instance metadata service is API only.

## What is a compute resource identity and how does it relate to the metadata service?
{: #faq-rdms-9}

Compute resource identities assign an IBM Cloud IAM identity to an individual compute resource. Instead of having to create a service ID, generate an API key, get the application to store and validate that key, you can assign IAM identity directly to the instance and acquire access tokens. The compute resource identity service creates IAM object called a _trusted profile_, against which you assign access rights to enable the instance to call IAM-enabled service. Use the trusted profile when accessing the instance metadata service. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).
