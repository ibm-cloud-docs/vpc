---

copyright:
  years: 2019, 2026
lastupdated: "2025-12-16"

keywords: api, change log, metadata, new features, restrictions, migration, versioned change

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC Metadata API change log
{: #metadata-api-change-log}

Read the API change log to learn about updates and improvements to the {{site.data.keyword.vpc_full}} (VPC) [Metadata API](/apidocs/vpc-metadata). Change log announcements are ordered by the date that they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{: shortdesc}

By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code. If backward-incompatible changes require non-trivial client code changes to use an API version, the API change log might provide links to instructions, tips, or best practices for updating client code.
{: note}

Some changes, such as new response properties or new optional request parameters, are considered compatible with earlier versions. Other changes, such as new required request parameters, are not considered backward compatible. To avoid disruption from changes to the API, use the following best practices when you call the API:

- Catch and log any `4xx` or `5xx` HTTP status code, along with the included `trace` property
- Follow HTTP redirect rules for any `3xx` HTTP status code
- Consume only the resources and properties that your application needs to function
- Avoid depending on behavior that is not explicitly documented

## 16 December 2025
 {: #16-december-2025-metadata}

 **Burstable (shared core) instances.** When [retrieving an instance](/apidocs/vpc-metadata#get-instance), the `vcpu.percentage` property is now included in the response. This property indicates the percentage of vcpu time available to your instance. Additionally, the `vcpu.burst.limit` property is included in the response. This property indicates the percentage limit that the instance can burst. For more information, see [burstable virtual server instances](/docs/vpc?topic=vpc-burstable-virtual-servers).

## 30 September 2025
{: #23-september-2025-metadata}

### For all version dates
{: #30-september-2025-all-version-dates-metadata}

**Dynamic volume bandwidth allocation.** When [retrieving an instance](/apidocs/vpc-metadata#get-instance), the `volume_bandwidth_qos_mode` property is now included in the response. This property indicates which volume bandwidth QoS mode is enabled for this virtual server instance. Its value can be either `weighted` or `pooled`. For more information, see the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#30-september-2025) and [volume bandwidth QoS mode](/docs/vpc?topic=vpc-block-storage-bandwidth).

## 26 August 2025
{: #26-august-2025-metadata}

### For all version dates
{: #26-august-2025-all-version-dates-metadata}

**Access tokens and Certificates methods.** The existing access tokens and certificates methods in the [VPC Metadata API](/apidocs/vpc-metadata) are documented in the new [VPC Identity API](/apidocs/vpc-identity). For details, see the [VPC Identity change log](/docs/vpc?topic=vpc-identity-api-change-log#26-august-2025-identity).

## 1 April 2025
{: #1-april-2025-metadata}

### For all version dates
{: #1-april-2025-all-version-dates-metadata}

**Trust Domain Extensions for confidential computing.** In select regions, you can now enable [Intel&reg; Trust Domain Extensions (TDX)](/docs/vpc?topic=vpc-about-confidential-computing-vpc#confidential-computing-vpc-with-tdx). If it is enabled for this virtual server instance, then when [retrieving an instance](/apidocs/vpc-metadata#get-instance), the `confidential_compute_mode` property has the value `tdx`. For more information, see the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#1-april-2025).

## 12 November 2024
{: #12-november-2024-metadata}

### For all version dates
{: #12-november-2024-all-version-dates-metadata}

**Instance cluster network attachments.** If a virtual server instance has cluster network attachments, [retrieving the instance](/apidocs/vpc-metadata#get-instance) now includes a `cluster_network_attachments` property in the response. Additionally, you can [retrieve an instance cluster network attachment](/apidocs/vpc-metadata#get-instance-cluster-network-attachment) for more details on the instance's cluster network attachments. For more information, see the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#12-november-2024).

## 4 June 2024
{: #4-june-2024-metadata}

### For all version dates
{: #4-june-2024-all-version-dates-metadata}

**Enhancements in support of catalog offering plans.** When [retrieving an instance](/apidocs/vpc-metadata#get-instance) that was provisioned with a [billed catalog offering](/docs/vpc?topic=vpc-getting-started-images-on-vpc-catalog&interface=ui#images-on-vpc-catalog-images), the new `catalog_offering.plan.crn` property provides the associated billing plan. For more information, see [Provisioning instances with IBM Cloud billable catalog offering](/docs/vpc?topic=vpc-api-change-log#4-june-2024) in the VPC API change log.

## 28 May 2024
{: #28-may-2024-metadata}

### For all version dates
{: #28-may-2024-all-version-dates-metadata}

**Virtual network interface protocol state filtering mode.** When [retrieving](/apidocs/vpc-metadata#get-virtual-network-interface) or [listing](/apidocs/vpc-metadata#list-virtual-network-interfaces) virtual network interfaces, the new `protocol_state_filtering_mode` property denotes the protocol state filtering mode that is used for this virtual network interface. If the value is `auto`, protocol state packet filtering is enabled or disabled based on the virtual network interface's `target` resource type. For more information, see [Virtual network interfaces protocol state filtering](/docs/vpc?topic=vpc-api-change-log#28-may-2024) in the VPC API change log.

This release introduces the following updates for accounts that have special approval to preview these features:

**Confidential computing capabilities.** If [Intel&reg; Software Guard Extensions](/docs/vpc?topic=vpc-about-confidential-computing-vpc) are enabled on an instance or instance template, you can view the new `confidential_compute_mode` property, which indicates that confidential compute mode is enabled for this virtual server instance. For more information, see the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#28-may-2024).

**Secure boot capabilities.** If [secure boot](/docs/vpc?topic=vpc-confidential-computing-with-secure-boot-vpc) is enabled on an instance or instance template, you can view the new `enable_secure_boot` property when [retrieving an instance](/apidocs/vpc-metadata#get-instance). This property indicates whether secure boot is enabled for this virtual server instance. For more information, see the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#28-may-2024).

## 26 March 2024
{: #26-march-2024-metadata}

### For all version dates
{: #26-march-2024-all-version-dates-metadata}

**Reservations for Virtual Servers for VPC.** You can now purchase a [capacity reservation](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc) for a specified instance profile in a specified zone. When [retrieving an instance](/apidocs/vpc-metadata#get-instance), the new `reservation` and `reservation_affinity` properties indicate the reservation and reservation affinity policy in effect for the virtual server instance. The new `health_state` property indicates the instance's overall health state, while an accompanying `health_reasons` property indicates the reason for any unhealthy health states, such as a failed reservation.

For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#26-march-2024) and [Provisioning reserved capacity for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).

## 30 January 2024
{: #30-january-2024-metadata}

### For all version dates
{: #30-january-2024-all-version-dates-metadata}

**Reservations for Virtual Servers for VPC.** Accounts that have special approval to preview this feature can now purchase a [capacity reservation](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc) for a specified instance profile in a specified zone. When [retrieving an instance](/apidocs/vpc-metadata#get-instance), the new `reservation` and `reservation_affinity` properties indicate the reservation and reservation affinity policy in effect for the virtual server instance. The new `health_state` property indicates the instance's overall health state, while an accompanying `health_reasons` property indicates the reason for any unhealthy health states, such as a failed reservation.

For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#30-january-2024) and [Provisioning reserved capacity for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).

This feature is now generally available. See the [26 March 2024](#26-march-2024-metadata) announcement.

## 19 December 2023
{: #19-december-2023-metadata}

### For all version dates
{: #19-december-2023-all-version-dates-metadata}

**Virtual network interface support.** Accounts that have special approval can preview a new feature that expands the support for [virtual network interfaces](/docs/vpc?topic=vpc-vni-about). You can now [list](/apidocs/vpc-metadata#list-instance-network-attachments) or [view](/apidocs/vpc-metadata#get-instance-network-attachment) an instance's network attachments, and you can [list](/apidocs/vpc-metadata#list-virtual-network-interfaces) or [view](/apidocs/vpc-metadata#get-virtual-network-interface) virtual network interfaces targeting the instance. For compatibility with existing clients, an instance with virtual network interfaces now includes a read-only representation of its network attachments and virtual network interfaces as legacy network interface child resources. For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#19-december-2023), and learn about [support for old API clients](/docs/vpc?topic=vpc-vni-about&interface=ui#vni-old-api-clients).

## 17 October 2023
{: #17-october-2023-metadata}

### For all version dates
{: #17-october-all-version-dates-metadata}

**Non-uniform memory access (NUMA) awareness on instances.** When [retrieving an instance](/apidocs/vpc-metadata#get-instance), the new `numa_count` property indicates the number of NUMA nodes on which a virtual server instance is provisioned. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=api#next-gen-profiles). See also [Non-uniform memory access (NUMA) awareness on dedicated hosts and instances](/docs/vpc?topic=vpc-api-change-log#17-october-2023) in the VPC API change log.

## 15 August 2023
{: #15-august-2023-metadata}

### For all version dates
{: #15-august-2023-all-version-dates-metadata}

**Instance identity certificates.** You can now use the instance identity access token and a certificate signing request (CSR) to [create](/apidocs/vpc-metadata#create-certificate) an instance identity certificate. For more information, see [Generating an instance identity certificate by using an instance identity access token](/docs/vpc?topic=vpc-imd-identity-operations&interface=api#imd-acquire-certificate). Instance identity certificates are used when the traffic between an authorized client and the mounted file share is [encrypted in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit).

## 27 June 2023
{: #27-june-2023-metadata}

### For all version dates
{: #27-june-2023-all-version-dates-metadata}

**Extended SSH key encryption.** The `type` property now includes `ed25519` when [retrieving](/apidocs/vpc-metadata#get-key) or [listing](/apidocs/vpc-metadata#list-keys) public SSH keys. For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys&interface=api). See also [Extended SSH key encryption](/docs/vpc?topic=vpc-api-change-log#27-june-2023) in the VPC API change log.

## 14 February 2023
{: #14-february-2023-metadata}

### For all version dates
{: #14-february-2023-all-version-dates-metadata}

**VPC Metadata new endpoint URL.** You can now use the fully qualified domain name (FQDN) `api.metadata.cloud.ibm.com` for the VPC Metadata service endpoint. The FQDN resolves to the link-local IP address `169.254.169.254` without requiring the application of special configurations. For more information, see [Endpoint URLs](/apidocs/vpc-metadata#endpoint-url-metadata) in the VPC Metadata API.

**VPC Metadata communication protocol and hop limit.** You can now view the communication protocol and hop limit for IP response packets that are used by the [VPC Metadata service](/docs/vpc?topic=vpc-imd-about). When [retrieving the instance](/apidocs/vpc-metadata#get-instance), the new `protocol` and `response_hop_limit` properties are shown. For more information, see [Configure the Metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=api).

## 27 September 2022
{: #27-september-2022-metadata}

### For all version dates
{: #27-september-2022-all-version-dates-metadata}

**Sharing images across an enterprise account.** If a virtual server instance was provisioned from a catalog offering, [retrieving the metadata](/apidocs/vpc-metadata#get-instance) now includes a `catalog_offering` property in the response. For more information, see the [Virtual Private Cloud Metadata API](/apidocs/vpc-metadata).

See also the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial) and the [Import offering](/apidocs/resource-catalog/private-catalog#import-offering){: external} method in the Catalog Management API.
