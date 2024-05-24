---

copyright:
  years: 2019, 2024
lastupdated: "2024-05-28"

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

## 28 May 2024
{: #28-may-2024-metadata}

### For all version dates
{: #28-may-2024-all-version-dates-metadata}

This release introduces the following updates for accounts that have been granted special approval to preview these features:

**Confidential computing capabilities.** If [Intel&reg; Software Guard Extensions](/docs/vpc?topic=vpc-about-sgx-vpc) are enabled on an instance or instance template, you can view the new `confidential_compute_mode` property, which indicates that confidential compute mode is enabled for this virtual server instance. For more information, see the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#28-may-2024).

**Secure boot capabilities.** If [secure boot](/docs/vpc?topic=vpc-confidential-computing-with-secure-boot-vpc) is enabled on an instance or instance template, you can view the new `enable_secure_boot` property when [retrieving an instance](/apidocs/vpc-metadata#get-instance). This property indicates if secure boot is enabled for this virtual server instance. For more information, see the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#28-may-2024).

## 26 March 2024
{: #26-march-2024-metadata}

### For all version dates
{: #26-march-2024-all-version-dates-metadata}

**Reservations for Virtual Servers for VPC.** You can now purchase a [capacity reservation](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc) for a specified instance profile in a specified zone. When [retrieving an instance](/apidocs/vpc-metadata#get-instance), the new `reservation` and  `reservation_affinity` properties indicate the reservation and reservation affinity policy in effect for the virtual server instance. The new `health_state` property indicates the instance's overall health state, while an accompanying `health_reasons` property indicates the reason for any unhealthy health states, such as a failed reservation.

For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#26-march-2024) and [Provisioning reserved capacity for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).

## 30 January 2024
{: #30-january-2024-metadata}

### For all version dates
{: #30-january-2024-all-version-dates-metadata}

**Reservations for Virtual Servers for VPC.** Accounts that have been granted special approval to preview this feature can now purchase a [capacity reservation](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc) for a specified instance profile in a specified zone. When [retrieving an instance](/apidocs/vpc-metadata#get-instance), the new `reservation` and  `reservation_affinity` properties indicate the reservation and reservation affinity policy in effect for the virtual server instance. The new `health_state` property indicates the instance's overall health state, while an accompanying `health_reasons` property indicates the reason for any unhealthy health states, such as a failed reservation.

For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#30-january-2024) and [Provisioning reserved capacity for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).

This feature is now generally available. See the [26 March 2024](#26-march-2024-metadata) announcement.

## 19 December 2023
{: #19-december-2023-metadata}

### For all version dates
{: #19-december-2023-all-version-dates-metadata}

**Virtual network interface support.** Accounts that have been granted special approval can preview a new feature that expands the support for [virtual network interfaces](/docs/vpc?topic=vpc-vni-about). You can now [list](/apidocs/vpc-metadata#list-instance-network-attachments) and [view](/apidocs/vpc-metadata#get-instance-network-attachment) an instance's network attachments, and you can [list](/apidocs/vpc-metadata#list-virtual-network-interfaces) and [view](/apidocs/vpc-metadata#get-virtual-network-interface) virtual network interfaces targeting the instance. For compatibility with existing clients, an instance with virtual network interfaces now includes a read-only representation of its network attachments and virtual network interfaces as legacy network interface child resources. For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#19-december-2023), and learn about [support for old API clients](/docs/vpc?topic=vpc-vni-about&interface=ui#vni-old-api-clients).

## 17 October 2023
{: #17-october-2023-metadata}

### For all version dates
{: #17-october-all-version-dates-metadata}

**Non-uniform memory access (NUMA) awareness on instances.**  When [retrieving an instance](/apidocs/vpc-metadata#get-instance), the new `numa_count` property indicates the number of NUMA nodes on which a virtual server instance is provisioned. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=api#next-gen-profiles). See also [Non-uniform memory access (NUMA) awareness on dedicated hosts and instances](/docs/vpc?topic=vpc-api-change-log#17-october-2023) in the VPC API change log.

## 15 August 2023
{: #15-august-2023-metadata}

### For all version dates
{: #15-august-2023-all-version-dates-metadata}

**Instance identity certificates.** You can now use the instance identity access token and a certificate signing request (CSR) to [create](/apidocs/vpc-metadata#create-certificate) an instance identity certificate. For more information, see [Generating an instance identity certificate by using an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-acquire-certificate). Instance identity certificates are used when the traffic between an authorized client and the mounted file share is [encrypted in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit).

## 27 June 2023
{: #27-june-2023-metadata}

### For all version dates
{: #27-june-2023-all-version-dates-metadata}

**Extended SSH key encryption.** The `type` property now includes `ed25519` when [listing](/apidocs/vpc-metadata#list-keys) and [retrieving](/apidocs/vpc-metadata#get-key) public SSH keys. For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys&interface=api). See also [Extended SSH key encryption](/docs/vpc?topic=vpc-api-change-log#27-june-2023) in the VPC API change log.

## 14 February 2023
{: #14-february-2023-metadata}

### For all version dates
{: #14-february-2023-all-version-dates-metadata}

**VPC instance metadata new endpoint URL.** You can now use the fully qualified domain name (FQDN) `api.metadata.cloud.ibm.com` for the metadata service endpoint. The FQDN resolves to the link-local IP address `169.254.169.254` without requiring the application of special configurations. For more information, see [Endpoint URLs](/apidocs/vpc-metadata#endpoint-url-metadata) in the VPC Instance Metadata API.

**VPC instance metadata communication protocol and hop limit.** You can now view the communication protocol and hop limit for IP response packets used by the [VPC Instance Metadata service](/docs/vpc?topic=vpc-imd-about). When you [retrieve the instance](/apidocs/vpc-metadata#get-instance), the new `protocol` and `response_hop_limit` properties will be shown. For more information, see [Configure the instance metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=api).

## 27 September 2022
{: #27-september-2022-metadata}

### For all version dates
{: #27-september-2022-all-version-dates-metadata}

**Sharing images across an enterprise account.** If a virtual server instance was provisioned from a catalog offering, [retrieving the instance metadata](/apidocs/vpc-metadata#get-instance) will now include a `catalog_offering` property in the response. For more information, see the [Virtual Private Cloud Instance Metadata API](/apidocs/vpc-metadata).

See also the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial) and the [Import offering](/apidocs/resource-catalog/private-catalog#import-offering){: external} method in the Catalog Management API.
