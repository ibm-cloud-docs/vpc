---

copyright:
  years: 2019, 2023
lastupdated: "2023-06-27"


keywords: api, change log, metadata, new features, restrictions, migration, versioned change

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC Instance Metadata API change log
{: #metadata-api-change-log}

Read the API change log to learn about updates and improvements to the {{site.data.keyword.vpc_full}} (VPC) [Instance Metadata API](/apidocs/vpc-metadata). The change log lists changes that are ordered by the date they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{: shortdesc}

By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code. If backward-incompatible changes require non-trivial client code changes to use an API version, the API change log might provide links to instructions, tips, or best practices for updating client code.
{: note}

Some changes, such as new response properties or new optional request parameters, are considered backward compatible. Other changes, such as new required request parameters, are not considered backward compatible. To avoid disruption from changes to the API, use the following best practices when you call the API:

- Catch and log any `4xx` or `5xx` HTTP status code, along with the included `trace` property
- Follow HTTP redirect rules for any `3xx` HTTP status code
- Consume only the resources and properties your application needs to function
- Avoid depending on behavior that is not explicitly documented

## 27 June 2023
{: #27-june-2023-metadata}

### For all version dates
{: #27-june-2023-all-version-dates-metadata}

**Extended SSH key encryption.** The `type` property now includes  `ed25519` when [listing](/apidocs/vpc-metadata#list-keys) and [retrieving](/apidocs/vpc-metadata#get-key) public SSH keys. For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys&interface=api). See also [Extended SSH key encryption](/docs/vpc?topic=vpc-api-change-log#27-june-2023) in the VPC API change log.

## 14 February 2023
{: #14-february-2023-metadata}

### For all version dates
{: #14-february-2023-all-version-dates-metadata}

**VPC instance metadata new endpoint URL.** You can now use the fully qualified domain name (FQDN) `api.metadata.cloud.ibm.com` for the metadata service endpoint. The FQDN resolves to the link-local IP address `169.254.169.254` without requiring the application of special configurations. For more information, see [Endpoint URLs](/apidocs/vpc-metadata#endpoint-url) in the VPC Instance Metadata API.

**VPC instance metadata communication protocol and hop limit.** You can now view the communication protocol and hop limit for IP response packets used by the [VPC Instance Metadata service](/docs/vpc?topic=vpc-imd-about). When you [retrieve the instance](/apidocs/vpc-metadata#get-instance), the new `protocol` and `response_hop_limit` properties will be shown. For more information, see [Configure the instance metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=api).

## 27 September 2022
{: #27-september-2022-metadata}

### For all version dates
{: #27-september-2022-all-version-dates-metadata}

**Sharing images across an enterprise account.** If a virtual server instance was provisioned from a catalog offering, [retrieving the instance metadata](/apidocs/vpc-metadata#get-instance) will now include a `catalog_offering` property in the response. For more information, see the [Virtual Private Cloud Instance Metadata API](/apidocs/vpc-metadata).

See also the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial) and the [Import offering](/apidocs/resource-catalog/private-catalog#import-offering){: external} method in the Catalog Management API.
