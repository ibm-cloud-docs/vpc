---

copyright:
  years: 2021, 2025
lastupdated: "2024-12-17"

keywords: api, change log, beta, metadata

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Beta VPC Instance Metadata API change log
{: #metadata-beta-api-change-log}

Read the API change log to learn about updates and improvements to the {{site.data.keyword.vpc_full}} (VPC) [Beta Instance Metadata API](/apidocs/vpc-metadata-beta). The change log lists changes that are ordered by the date they were released.
{: shortdesc}

Some beta features are for accounts that have been granted special approval to preview a particular beta feature. Contact your IBM sales representative if you are interested in getting access.
{: beta}

There are no backward-compatibility guarantees as a feature progresses through its beta phase or from the final beta release to its initial GA release. Using non-GA-mature features could introduce the risk of corrupting resources in your account. IBM strongly recommends that you do not use non-GA-mature features on production accounts.
{: important}

To review the change log of generally available metadata API features, see the [VPC Instance Metadata API change log](/docs/vpc?topic=vpc-metadata-api-change-log).

## 17 December 2024
{: #17-december-2024-metadata-beta}

### For all version dates
{: #17-december-2024-all-version-dates-metadata-beta}

**Enhanced confidential computing capabilities.** If [Intel&reg; Trust Domain Extensions](/docs/vpc?topic=vpc-about-confidential-computing-vpc) are enabled on an instance or instance template, you can view the `confidential_compute_mode` property new value `tdx`, which indicates that confidential compute mode is enabled for this virtual server instance. For more information, see the [Beta VPC API change log](/docs/vpc?topic=vpc-api-change-log-beta#17-december-2024-beta).

## 11 July 2023
{: #11-july-2023-metadata-beta}

### For all version dates
{: #11-july-2023-all-version-dates-metadata-beta}

**Instance identity certificates.** You can now use the instance identity access token and a Certificate Signing Request (CSR) to [create](/apidocs/vpc-metadata-beta/initial#create-certificate) an instance identity certificate. For more information, see [Generating an instance identity certificate by using an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-acquire-certificate). Instance identity certificates can be used when the traffic between an authorized client and the mounted file share is [encrypted in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit). For more information, see [Data encryption in transit for file shares](/docs/vpc?topic=vpc-api-change-log-beta#11-july-2023-beta).

This feature is now generally available. See the [VPC Instance Metadata API change log](/docs/vpc?topic=vpc-metadata-api-change-log#15-august-2023-metadata).

## 16 August 2022
{: #16-august-2022-metadata-beta}

### For all version dates
{: #16-august-2022-all-version-dates-metadata-beta}

**Sharing images across an enterprise account.** For accounts that have been granted special approval to preview this feature, if a virtual server instance was provisioned from a catalog offering, [retrieving the instance](/apidocs/vpc-metadata#get-instance) will now include a `catalog_offering` property in the response. For more information, see the [Virtual Private Cloud Instance Metadata API](/apidocs/vpc-metadata).

See also [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial) and the [Import offering](/apidocs/resource-catalog/private-catalog#import-offering){: external} method in the Catalog Management API.

This feature is now generally available. See the [VPC Instance Metadata API change log](/docs/vpc?topic=vpc-metadata-api-change-log#27-september-2022-metadata).

## 1 March 2022
{: #1-march-2022-metadata-beta}

### For all version dates
{: #1-march-2022-all-version-dates-metadata-beta}

**VPC instance metadata service.** The [VPC Instance Metadata API](/apidocs/vpc-metadata) is now generally available in all regions. See [About VPC Instance Metadata](/docs/vpc?topic=vpc-imd-about) and [Known issues](/docs/vpc?topic=vpc-known-issues).

## 2 November 2021
{: #2-november-2021-metadata-beta}

### For all version dates
{: #2-november-2021-all-version-dates-metadata-beta}

The metadata service introduces a [new method](/apidocs/vpc-metadata-beta/initial#create-iam-token) (`POST /instance_identity/v1/iam_token`) to [generate an IAM token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange) from an instance identity access token. This method uses the instance identity access token and a trusted profile linked to a virtual server instance to generate an IAM access token. Beta users should migrate to this new method. Using the IAM API to pass the instance identity access token and generate an IAM token is deprecated.
