---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-24"

keywords:

subcollection: vpc

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for {{site.data.keyword.vpc_short}}
{: #release-notes}

Use the release notes to learn the latest updates to {{site.data.keyword.vpc_full}} that are grouped by date.
{: shortdesc}

For more information about changes to the {{site.data.keyword.vpc_short}} API, see [{{site.data.keyword.vpc_short}} API change log](/docs/vpc?topic=vpc-api-change-log).

For more information about changes to the {{site.data.keyword.vpc_short}} command-line interface (CLI), see [{{site.data.keyword.vpc_short}} CLI release notes](/docs/vpc?topic=vpc-vpc-cli-rn).

## February 2026
{: #vpc-feb26}

### 20 February 2026
{: #vpc-feb2026}
{: release-note}

Service deprecation: IBM Cloud LinuxONE and Hyper Protect Virtual Server offerings
:   IBM announces the deprecation of the following services:
  
   - [IBM Cloud LinuxONE Virtual Server for VPC (s390x architecture)](/docs/vpc?group=virtual-servers)
   - [IBM Cloud Dedicated Host for VPC (s390x architecture)](/docs/vpc?group=dedicated-hosts)
   - [IBM Cloud Hyper Protect Virtual Server for VPC](https://www.ibm.com/products/hyper-protect-virtual-servers/cloud)
   - [IBM Cloud Wazi as a Service](https://www.ibm.com/products/wazi-as-a-service)

   The End of Marketing (EOM) date is 28 February 2026. From EOM, IBM will disable the provisioning of new instances; however, any instances that were provisioned before this date will continue to be supported until the End of Service date.

   The End of Service (EOS) date is 20 February 2027, on which IBM will terminate all active instances.

   For more information, see the [Service deprecation announcement](/docs/vpc?topic=vpc-ichpcs_deprecated_anmt).

### 12 February 2026
{: #vpc-feb1226}
{: release-note}

Spot instances (GA)
:   [Spot instances](/docs/vpc?topic=vpc-spot-instances-virtual-servers) are highly discounted versions of the standard instances. They are designed to use available compute resources for interruptible or stateless workloads. Spot instances can be preempted (or evicted) at any time.

### 11 February 2026
{: #vpc-feb1126}
{: release-note}

4th generation instance profiles available in Dallas (`us-south`) region (select availability)
:   The 4th generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are available in the Dallas (`us-south`) region. This new generation features virtual server profile families hosted exclusively on 6th Generation Intel&reg; Xeon&reg; Scalable processors. For more information, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui). General purpose profiles are available in the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced-intel-x86-64-gen4), [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute-intel-x86-64-gen4), and [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory-intel-x86-64-gen4) families. 4th generation dedicated host profiles are also available. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen4-intel&interface=ui#general-purpose-dh-profiles-gen4).

## January 2026
{: #vpc-jan26}

### 22 January 2026
{: #vpc-jan2226}
{: release-note}

Spot instances (Select availability)
:   [Spot instances](/docs/vpc?topic=vpc-spot-instances-virtual-servers) are highly discounted versions of the standard instances. They are designed to use available compute resources for interruptible or stateless workloads. Spot instances can be preempted (or evicted) at any time.

### 16 January 2026
{: #vpc-jan1626}
{: release-note}

BYOL available for x86 SUSE Linux Enterprise Server (SLES) versions 15 and 16
:   Bring your own license (BYOL) is now supported for x86 SUSE Linux Enterprise Server (SLES), versions 15 and 16. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about&interface=ui).

### 14 January 2026
{: #vpc-jan1426}
{: release-note}

Confidential computing with Intel Trusted Domain Extension (TDX) for Virtual Servers for VPC is available in Dallas (select availability)
:   Confidential computing with Intel&reg; Trusted Domain Extension (TDX) for VPC is now available in the Dallas (us-south) region. This region is in addition to the existing Washington DC (us-east) and Frankfurt (eu-de) regions. Confidential computing with Intel TDX offers confidentiality to virtual machines by providing CPU enhancements that are leveraged by the firmware and hardware to provide confidentiality and integrity. For more information, see [Confidential computing for x86 Virtual Servers for VPC](/docs/vpc?topic=vpc-about-confidential-computing-vpc).

### 09 January 2026
{: #vpc-jan0926}
{: release-note}

Local-access virtual private endpoint gateway support for DNS sharing VPC topology
:   You can now create local-access Virtual Private Endpoints (VPEs) in DNS-shared VPCs when using a DNS sharing VPC topology for Cloud Object Storage. This feature lets you send network traffic locally between the DNS-shared VPC where the local-access VPE gateway is created and the service endpoint, improving performance and allowing finer control of Context-Based Restrictions (CBR) and security rules. For more information, see [Local-access VPE gateway support](/docs/vpc?topic=vpc-about-vpe#multi-tenant-endpoint-support).

## December 2025
{: #vpc-dec25}

### 19 December 2025
{: #vpc-dec1925}
{: release-note}

4th generation instance profiles available in Dallas (`us-south`) region (select availability)
:   The 4th generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are available to customers with special access in the Dallas (`us-south`) region. This new generation features virtual server profile families hosted exclusively on 6th Generation Intel&reg; Xeon&reg; Scalable processors. For more information, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui). General purpose profiles are available in the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced-intel-x86-64-gen4), [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute-intel-x86-64-gen4), and [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory-intel-x86-64-gen4) families. 4th generation dedicated host profiles are also available. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen4-intel&interface=ui#general-purpose-dh-profiles-gen4).

### 18 December 2025
{: #vpc-dec1825}
{: release-note}

Accelerated l4 profile now available in Chennai region (GA)
:   The l4 GPU profile is now available in zone 1 (`in-che-1`) of the Chennai (`in-che`) region. For more information, see [NVIDIA L4 instance profiles](/docs/vpc?topic=vpc-accelerated-profile-family#l4-profiles). For more information about regions, see [IBM Cloud region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 12 December 2025
{: #vpc-dec1225}
{: release-note}

Burstable Flex virtual servers (GA)
:   Burstable Flex virtual servers are designed to provide flexible CPU performance so workloads can operate at a smaller baseline level and opportunistically burst to higher performance when needed. Flex virtual servers are ideal for applications that require a baseline of CPU performance and experience intermittent spikes in CPU demand but don't require sustained high performance. For more information, see [Burstable virtual servers](/docs/vpc?topic=vpc-burstable-virtual-servers).

Support for all IPv4 protocols for Network ACL and Security Group rules (GA)
:   You can now apply any [IPv4 protocol listed in IANA](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) to Network Access Control List rules and Security Group rules. This allows you to bring custom topologies and use cases from on-prem, multi-cloud and Classic infrastructure that leverage protocols outside of TCP, UDP, and ICMP. For more information, see [Understanding Internet Communication Protocols](/docs/vpc?topic=vpc-understanding-icp&interface=ui).

### 11 December 2025
{: #vpc-dec1125}
{: release-note}

Cross-regional copy of snapshots that are larger than 10 TB (GA)
:   Customers can now create cross-regional copies of snapshots of second-generation storage volumes that exceed 10 TB. Such cross-regional copies can be created on-demand in the console, from the CLI, with the API, or Terraform or on-schedule with the Backup for VPC service. For more information, see [About Block Storage for VPC snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots_vpc_crossregion_copy).

### 05 December 2025
{: #vpc-dec0525}
{: release-note}

Names for security group rules
:   You can now assign a name to a security group rule. Each rule must have a name that is unique within its security group. If you don’t provide one, the system generates a default name that you can update later. Existing rules (and the default rules created with a new VPC) now have system-assigned names. For more information, see [Defining security group rules](/docs/vpc?topic=vpc-using-security-groups#defining-security-group-rules).

### 04 December 2025
{: #vpc-dec0425}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-25` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-25`, new attestation certificate is available. For more information, see [Attestation certificate](/docs/vpc?topic=vpc-about-attestation).

Cross-regional copy of snapshots 10 TB+ (beta)
:   Customers with special access can now create cross-regional copies of snapshots of second-generation storage volumes that exceed 10 TB. Such cross-regional copies can be created on-demand in the console, from the CLI, with the API, or Terraform or on-schedule with the Backup for VPC service. For more information, see [About Block Storage for VPC snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots_vpc_crossregion_copy).

## November 2025
{: #vpc-nov25}

### 20 November 2025
{: #vpc-nov2025}

Regional file shares now available in Sao Paulo (Select availability)
:   Customers with special access can now create and use file shares with regional data availability in Sao Paulo (`br-sao`) MZR.

### 19 November 2025
{: #vpc-nov1925}
{: release-note}

Backup for VPC support for regional file shares (Select availability)
:   Customers with special access to preview the regional file share offering can automate the creation and deletion of snapshots for these file shares with the Backup for VPC service. You can manage your backup policies for second-generation shares in the console, from the CLI, or with the API. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

### 06 November 2025
{: #vpc-nov0625}
{: release-note}

Site-to-site dynamic route-based VPN connections (GA)
:   You can now configure dynamic route-based VPN connections for site-to-site VPNs. This configuration learns routes dynamically and establishes connections between your on-premises peer network and the IBM Cloud VPN gateway. Dynamic routing improves network management and provides high availability allowing services like Power Virtual Server to seamlessly connect through VPN gateway connections. For more information, see [Creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway&interface=ui)

Regional file shares - Base bandwidth and pricing changes (Select availability)
:   Customers with special access can now provision regional file shares with the baseline bandwidth of 800 Mbps. The bandwidth value can be adjusted between the preset value and the maximum of 8192 Mbps in the console, from the CLI, or with the API. You pay extra only for the bandwidth that exceeds the preset 800 Mbps.

## October 2025
{: #vpc-oct25}

### 31 October 2025
{: #vpc-oct3125}
{: release-note}

Chennai region now available
:   The Chennai region is now available for provisioning the 3rd generation of [virtual servers](/docs/vpc?topic=vpc-profiles), [dedicated hosts](/docs/vpc?topic=vpc-dh-profiles), and [bare metal servers](/docs/vpc?topic=vpc-bare-metal-servers-profile).
   - You can provision virtual servers in the Chennai region from the Balanced, Compute, Memory, Very High Memory, and Flex profile families. For more information, see [General purpose - Gen 3 profile details](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen3-intel) and [General purpose - Flex profile details](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers).
   - You can provision select bare metal server profiles in the Chennai region. For more information, see [Generation 3 (x3) bare metal profiles availability by region](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui#bare-metal-profile-availability-by-region-gen3).
   - First-generation storage services are also available, except for the cross-regional replication feature for file shares and cross-regional copy feature for block volume snapshots. Block volume snapshots that are taken in the Chennai region are encrypted by using a {{site.data.keyword.keymanagementserviceshort}} instance from the London (`eu-gb`) region temporarily. For more information, see [Known issues](/docs/vpc?topic=vpc-storage-known-issues#snapshot-COS-upload-IN-CHE-EU-GB).
   - For more information about the Chennai region and zones, see [IBM Cloud region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 23 October 2025
{: #vpc-oct2325}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-24` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-24`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

`initcontainer` in contract
:   The contract now supports `initcontainer` under the `play` section with SHA256 based reference format. For more information, see [The `play` subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_play).

Certificate chain support for Syslog
:   Certificate chain support for Syslog is now available. For more information, see [Certificate chain support for Syslog](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc#syslog-chainsupport).

Workload update for Hyper Protect Secure Build
:   The `workload` section of the Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-24`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb). Clone the latest Secure-Build-Cli to create a Hyper Protect Secure Build server.

Updated seed policies
:   The `env` and `workload` seed policies are updated. For more information, see [The workload - volumes subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_volumes).

### 01 October 2025
{: #vpc-oct0125}
{: release-note}

Confidential computing with Intel Trusted Domain Extension (TDX) for Virtual Servers for VPC is available in Frankfurt (select availability)
:   Confidential computing with Intel&reg; Trusted Domain Extension (TDX) for VPC is now available in the Frankfurt (eu-de) region. This region is in addition to the existing Washington DC (us-east) region. Confidential computing with Intel TDX offers confidentiality to virtual machines by providing CPU enhancements that are leveraged by the firmware and hardware to provide confidentiality and integrity. For more information, see [Confidential computing for x86 Virtual Servers for VPC](/docs/vpc?topic=vpc-about-confidential-computing-vpc). When you create a virtual server instance with a confidential computing profile and Intel Trusted Domain Extension (TDX), you can create that virtual server instance only in the Frankfurt (eu-de) or Washington DC (us-east) regions. You can’t create a virtual server instance with TDX in any other region, including Dallas (us-south).

## September 2025
{: #vpc-sep25}

### 30 September 2025
{: #vpc-sep3025}
{: release-note}

Pooled bandwidth allocation for block storage data volumes
:   [Select compute profiles](/docs/vpc?group=profile-details) now support pooled bandwidth allocation for attached data volumes that improves bandwidth sharing between volumes. You can enable and disable pooled bandwidth allocation in the console, from the CLI, and with the API. For more information about the new storage QoS setting, see [Bandwidth Allocation for Data Volumes](/docs/vpc?topic=vpc-block-storage-bandwidth&interface=ui#attached-block-vol-bandwidth).

### 26 September 2025
{: #vpc-sep2625}
{: release-note}

Flex profiles (GA)
:   The general-purpose Flex virtual server profiles (nano, balanced, compute, and memory) are built atop the 2nd and 4th Generation Intel® Xeon® Scalable processors and AMD’s 3rd generation EPYC processors. You can place Flex profiles on any available generation of these processors in a specified region. For more information, see [Flex profiles](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers).

### 25 September 2025
{: #vpc-sep2525}
{: release-note}

Public address ranges (GA)
:   You can now create and manage public address ranges. A public address range is a contiguous block of public IP addresses that can be bound to a specific zone in a VPC. Ranges can be bound at creation or updated later to associate with a different zone or VPC. To enable inbound traffic from the internet, you must configure VPC routes to direct traffic to resources in the bound zone. For more information, see [About public address ranges](/docs/vpc?topic=vpc-about-par).

New targets for Private Path network load balancer pools
:   You can now add reserved IP addresses as members of a back-end pool attached to a Private Path Network Load Balancer. A reserved IP member can be bound to a bare metal server, primary or secondary interface of a virtual server instance, or a virtual network interface. You still have the options to add an application load balancer or virtual server instances as members of a back-end pool. For more information, see [Creating a Private Path network load balancer](/docs/vpc?topic=vpc-ppnlb-ui-creating-private-path-network-load-balancer&interface=ui).

### 23 September 2025
{: #vpc-sep2325}
{: release-note}

Second-generation block storage volumes (GA)
:   Second-generation block storage volumes and snapshots are now generally available in Dallas, Frankfurt, London, Madrid, Osaka, Sao Paulo, Sydney, Tokyo, Toronto, and Washington, DC. All customers can use the `sdp` volume profile to create boot and data volumes for their virtual server instances. For a comparison between first and second-generation block storage features, see [VPC storage services overview](/docs/vpc?topic=vpc-storage-overview). For more information about the block storage service and the new volume profile, see [About Block Storage for VPC](/docs/vpc?topic=vpc-block-storage-about) and [Block Storage for VPC profiles](/docs/vpc?topic=vpc-block-storage-profiles).

Second-generation block storage volume snapshots (GA)
:   Customers can take and manage snapshots of their second-generation volumes in the console, from the CLI, or with API. Customers can also automate the creation and deletion of second-generation snapshots with the Backup for VPC service. Fast restore clones and cross-regional copies are available in all regions where second-generation volumes are supported. The only exception where cross-regional snapshot copies are not supported are the snapshots of volumes that exceed 10 TB. Consistency groups for Gen 2 snapshots are not supported. For more information, see [About Block Storage for VPC snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) and [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

Regional file shares (Select availability)
:   The new `rfs` share profile is now available for customers with special access in Dallas, Frankfurt, London, Madrid, Osaka, Sydney, Tokyo, Toronto, and Washington, DC. Shares that are created with this profile offer regional data availability and support adjustable share bandwidth. File share data can be protected with customer-managed encryption keys, and in-transit encryption is supported by the use of stunnel. Cross-account access is now supported. Transit encryption settings can be enforced by the share owner. For regional shares, the available allowed transit encryption modes are `stunnel`, `none`, or both. For more information, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about).

Regional file share snapshots (Select availability)
:   On-demand snapshots are now available for regional file shares. You can create snapshots in the console, from the CLI, and with the API. You can use the snapshots to create other regional file shares in the same region. When you create a file share from a snapshot, the share profile of the new file share must match the share profile of the snapshot's parent file share. You can use the snapshots to restore individual files, the regional snapshots can be accessed in the `.snap` directory. For more information, see [About File Storage for VPC snapshots](/docs/vpc?topic=vpc-fs-snapshots-about).

Change in allowed transit encryption mode value of zonal file shares
:   Previously, when you created a file share with the `dp2` profile, you could select `customer-managed` and `none` as the allowed transit encryption modes. When a mount target is created for the file share, its transit encryption value must match one of the values that the file share allows. For zonal file shares and mount targets, the `user-managed` value is now changed to `ipsec` in all user interfaces. Before you adopt the release version `2025-09-23` or later, review the changes in the migration guidance that might require you to update your client or scripts. For more information, see [Updating to the latest API code](/docs/vpc?group=updating-api) and [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).

### 11 September 2025
{: #vpc-sep1125}
{: release-note}

IBM Cloud Security Posture Management (CSPM) on VPC
:   You can add CSPM to a virtual server instance or bare metal server. When this option is selected, a workload protection instance or bare metal server is created with the configurations to provide CSPM to all the resources. If a workload protection instance already exists, this option is not available. For more information, see [About IBM Cloud Security Posture Management (CSPM)](/docs/workload-protection?topic=workload-protection-about&interface=ui).

    - To add CSPM to your virtual server instance, see the Advanced options in [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui) and [Managing virtual server instances: Adding CSPM in the console](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#cloud-security-posture-management).

    - To add CSPM to your bare metal server, see the Advanced options in [Creating Bare Metal Servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=ui) and [Managing Bare Metal Servers for VPC: Adding CSPM in the console](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=ui#cloud-security-posture-management-ui)

### 04 September 2025
{: #vpc-sep0425}
{: release-note}

Customer-managed encryption keys for regional file shares
:   Customers with special access to preview file shares with regional availability can now use customer-managed encryption keys to secure their regional file shares. For more information, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption) and [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

### 02 September 2025
{: #vpc-sep0225}
{: release-note}

Manage BGP route selection in VPC
:   Previously, VPC didn't honor AS path length when choosing the best route, and this required using a Transit Gateway between Direct Link and VPC in certain topologies to achieve specific routing behavior and high availability. With this enhancement, you can now influence routing decisions by using AS Path length to choose specific paths—ideal for managing primary and backup Direct Link connections, or configuring failover in hybrid and multicloud environments. For more information, see [Using AS Prepends with VPC connections](/docs/dl?topic=dl-dl-planning-considerations#as-prepends-routes).

### 01 September 2025
{: #vpc-sep0125}
{: release-note}

Removing limit on rules targeting remote security groups
:   The service limit of 15 unique remote security groups across all rules per security group, targeting remote security groups, has been removed. With this change, customers can maintain consistent security policies for all the security groups of a VPC. For more information, see [Service limits for VPC services](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services).

## August 2025
{: #vpc-aug25}

### 28 August 2025
{: #vpc-aug2825}
{: release-note}

VPC Metadata on bare metal servers (GA)
:   The VPC Metadata service is available on bare metal servers. It is a free service that provides a REST API that you invoke within a bare metal server to get bare metal server identity tokens and bare metal server identity certificates. Access to the API is unavailable from outside the bare metal server. The service is disabled by default. You can optionally use bare metal server identity tokens to get IAM access tokens for access to all IAM-enabled services. For more information, see [About {{site.data.keyword.vpc_full}} (VPC) Metadata on bare metal servers](/docs/vpc?topic=vpc-bare-metal-server-metadata-about).

   For information about the REST API, see the following:

   - [API change log](/docs/vpc?topic=vpc-api-change-log#26-august-2025)
   - [API Identity change log](/docs/vpc?topic=vpc-identity-api-change-log#26-august-2025-identity)
   - [API Metadata change log](/docs/vpc?topic=vpc-metadata-api-change-log#26-august-2025-metadata)
   - [VPC Identity API](/apidocs/vpc-identity)

   If you currently use the `/instance_identity/v1/token` method and want to adopt the API release version 2025-08-26 or later, review the changes that are described in the migration guidance: [Updating to the `2025-08-26` version of the VPC Identity API](/docs/vpc?topic=vpc-2025-08-26-migration-metadata-identity#changed-paths-metadata-identity).

Encryption-in-transit support for bare metal servers
:   Customers can use the VPC Identity API to obtain bare metal server identity certificates. The bare metal server identity certificate can be used to establish an encrypted connection between the bare metal server and the file share that is mounted on it. For more information, see [Encryption in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit).

Mount Helper utility supports mounting file shares on bare metal servers with EIT
:   The Mount Helper utility is updated to support installation on bare metal servers. Download version 0.2.1 or later to help you mount a zonal file share on your bare metal server with encryption-in-transit. For more information, see the [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) topic.

### 26 August 2025
{: #vpc-aug2625}
{: release-note}

Cross-regional copies of second-generation block volume snapshots (select availability)
:   Customers with special access to preview the second-generation block storage volumes can now create cross-regional copies of their snapshots in every region where second-generation block volumes are supported. Cross-regional copies can be created in the console, from the CLI, or with the API. For more information about this feature and its limitations, see [About Block Storage for VPC snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about).

### 22 August 2025
{: #vpc-aug2225}
{: release-note}

Increasing limit on NACLs rules
:   The default service limit of `100` rules for Network ACLs is increasing to `200` rules. By creating rules in Network ACLs (Access Control Lists), network administrators can define granular access policies for inbound and outbound traffic, ensuring that only authorized communication is permitted while blocking unwanted or potentially harmful traffic. For more information, see [Quotas for Access Control Lists](/docs/vpc?topic=vpc-quotas#acl-quotas).

Burstable Flex virtual servers (beta)
:   Burstable Flex virtual servers are designed to provide flexible CPU performance so workloads can operate at a smaller baseline level and opportunistically burst to higher performance when needed. Flex virtual servers are ideal for applications that require a baseline of CPU performance and experience intermittent spikes in CPU demand but don't require sustained high performance. For more information, see [Burstable virtual servers](/docs/vpc?topic=vpc-burstable-virtual-servers).

### 14 August 2025
{: #vpc-aug1425}
{: release-note}

UI enhancement for bare metal servers: Filter instance profiles by business scenario
:   When creating a bare metal server, you can now use the By scenario tab on the Select a server profile page to narrow the results to include only applicable bare metal server profiles. For example, you can filter profiles by the following business scenarios: SAP; Web Development and Test; HPC; Confidential computing; and Storage optimized. When a specific filter is selected, the profile results display only the profiles related to the defined business scenario.

## July 2025
{: #vpc-jul25}

### 31 July 2025
{: #vpc-jul3125}
{: release-note}

Cross-regional replication of file shares is available in Montreal
:   Customers can now create cross-regional file share replicas in the `ca-mon` region, or use their file shares in the `ca-mon` region as the source share. For more information about cross-regional replication, see [About file share replication](/docs/vpc?topic=vpc-file-storage-replication).

### 23 July 2025
{: #vpc-jul2325}
{: release-note}

Cross-regional copies of second-generation block volume snapshots (beta release)
:   Customers with special access to preview the second-generation block storage volumes can now create cross-regional copies of their snapshots in London (`eu-gb`), Osaka (`js-osa`), Sao Paulo (`br-sao`), and Sydney (`au-syd`) regions in the console, from the CLI, or with the API. For more information about this feature and its limitations, see [About Block Storage for VPC snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui).

New file share profile with regional data availability (beta release)
:   The File Storage for VPC offering includes a new profile in the Defined Performance profile family. Customers with special access can use this profile to create regional file shares that provide data availability across all 3 zones of a region. The regional shares can be created with capacity 1 - 32000 GB, adjustable throughput up to 8192 Mbps, and a fixed IOPS value of 35000. The regional shares can be encrypted with provider-managed or customer-managed encryption keys. You can mount these file shares by using a regional mount target and opt to encrypt your data in transit with stunnel. For more information, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-cross-account-mount).

Mount Helper utility supports mounting regional file shares with stunnel
:   Customers with special access to preview the regional file share profile, can use the Mount Helper utility to mount their regional shares. The utility is upgraded to include the `--stunnel` option in the installation script. Download version 0.1.8 or later to create a secure mount with in-transit encryption capability. For more information, see the [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility).

### 21 July 2025
{: #vpc-jul2125}
{: release-note}

Secure boot for Virtual Servers for VPC is now optional
:   When you select a [confidential computing instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles), Secure boot for Virtual Servers for VPC is selected automatically. You now have the option to disable secure boot and still utilize the confidential computing profile. For more information, see [Secure boot for Virtual Servers for VPC](/docs/vpc?topic=vpc-confidential-computing-with-secure-boot-vpc) and [Managing virtual server instances: Disable or enable secure boot](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#disable-secure-boot-ui).

### 17 July 2025
{: #vpc-july1725}
{: release-note}

UI enhancement: Profile tables now include categories that are collapsible and expandable
:   When provisioning a virtual server or bare metal server, you can now view profiles by category within the table on the Select a profile page. Click the category heading to expand or collapse the section. For virtual server profiles you can view profiles in the following categories: Balanced, Compute, Memory, GPU & Accelerated, Storage optimized, and High memory. For bare metal servers profiles you can view profiles in the following categories: Balanced, Compute, Memory, and Ultra high memory. For more information about profile families, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui).

### 15 July 2025
{: #vpc-jul1525}
{: release-note}

Allowed-use expressions now available for custom images (GA)
:   You can use allowed-use expressions to configure which profiles, images, and server settings can be used together when you provision a virtual server instance or bare metal server. When you make selections to create a virtual server instance or bare metal server, if you use a custom image with defined allowed-use expressions, you can then determine which profiles are compatible with those expressions. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).

Backup for VPC support for second-generation block volumes (select availability)
:   Customers with special access to preview the second-generation block storage volumes can automate the creation and deletion of snapshots for these volumes with the Backup for VPC service. You can manage your backup policies for second-generation volumes in the console, from the CLI, or with the API. For more information, see [Backups for second-generation block storage volumes](/docs/vpc?topic=vpc-backup-service-about).

### 11 July 2025
{: #vpc-jul1125}
{: release-note}

Public address ranges (select availability)
:   Customers with special access can now create and use public address ranges in the Frankfurt (eu-de) and Madrid (eu-es) regions. A public address range is a contiguous block of public IP addresses that can be bound to a zone in a VPC. For more information, see [About public address ranges](/docs/vpc?topic=vpc-about-par).

### 03 July 2025
{: #vpc-jul0325}
{: release-note}

Snapshot for second-generation block volumes (select availability)
:   The snapshots feature for second-generation volumes is now available in Dallas (`us-south`), Frankfurt (`eu-de`), London (`eu-gb`), Madrid (`eu-es`), Osaka (`js-osa`), Sao Paulo (`br-sao`), Sydney (`au-syd`), Tokyo (`jp-tok`), Toronto (`ca-tor`), and Washington (`us-east`) regions for allow-listed customers. For more information, see [About {{site.data.keyword.block_storage_is_short}} snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about).

## June 2025
{: #vpc-jun25}

### 27 June 2025
{: #vpc-jun2725}
{: release-note}

Flex profiles (beta release)
:   The general-purpose flex virtual server profiles (nano, balanced, compute, and memory) are built atop the 2nd and 4th Generation Intel® Xeon® Scalable processors and AMD’s 3rd generation EPYC processors. You can place Flex profiles on any available generation of these processors in a specified region. Customers with special access can preview flex virtual server profiles on any supported processor in a supported region. For more information, see [Flex profiles](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers).

### 19 June 2025
{: #vpc-jun1925}
{: release-note}

New Intel RHEL AI operating system images for GPU types
:  The new Red Hat Enterprise Linux&reg; (RHEL) operating system images are each tailored for their respective GPU type. RHEL AI for Intel is now available. When you select an RHEL AI 1.x image, make sure that you are using the correct RHEL AI image for the instance profile you chose. If you are using the Intel GPU profile, you must use the RHEL AI for Intel image. For more information about the GPU profiles you can use with RHEL AI images, see [Accelerated instance profiles - Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family). For information about RHEL AI, including information such as the supported use cases for RHEL AI, see [Red Hat Enterprise Linux AI](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_ai/1.5){: external}

### 05 June 2025
{: #vpc-jun0525}
{: release-note}

New Nvidia and AMD RHEL AI operating system images for GPU types
:  The new Red Hat Enterprise Linux&reg; (RHEL) operating system images are each tailored for their respective GPU type. RHEL AI for Nvidia and RHEL AI for AMD are now available. When you select an RHEL AI 1.x image, make sure that you are using the correct RHEL AI image for the instance profile you chose. If you are using the Nvidia GPU profile, you must use the RHEL AI for Nvidia image. If you are using the AMD GPU profile, you must use the RHEL AI for AMD image.  For more information about the GPU profiles you can use with RHEL AI images, see [[Accelerated instance profiles - Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family). For information about RHEL AI, including information such as the supported use cases for RHEL AI, see [Red Hat Enterprise Linux AI](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_ai/1.5){: external}.


### 03 June 2025
{: #vpc-jun0325}
{: release-note}

New regions for second-generation block storage volumes (select availability)
:   The `sdp` profile is now available in Madrid (`eu-es`), Osaka (`jp-osa`), Sao Paulo (`br-sao`), Sydney (`au-syd`), and Toronto (`ca-tor`) regions. For more information, see [The SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-about#block-storage-sdp-intro).

## May 2025
{: #vpc-may25}

### 29 May 2025
{: #vpc-may2925}
{: release-note}

AMD Instinct MI300X accelerated virtual server profile now available in Washington DC (us-east-wdc07-a) and Frankfurt (eu-de) (select availability)
:   The AMD Instinct MI300X accelerated virtual server profile is now available in Washington DC (us-east-wdc07-a) and Frankfurt (eu-de-fra02-a, eu-de-fra05-a), in addition to Washington DC (us-east-wdc06-a). The profile runs on an [AMD Instinct™ MI300X Accelerator](https://www.amd.com/en/products/accelerators/instinct/mi300/mi300x.html){: external} that is tuned for AI workloads, including inferencing and fine-tuning. For more information about the `gx3d-208x1792x8mi300x` profile, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles and [Accelerated instance profiles](/docs/vpc?topic=vpc-accelerated-profile-family&interface=ui).

### 27 May 2025
{: #vpc-may2725}
{: release-note}

New region for second-generation block storage volumes (select availability)
:   The `sdp` profile is now available in the Dallas (`us-south`) region for allow-listed customers. For more information, see [The SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-about#block-storage-sdp-intro).

### 15 May 2025
{: #vpc-may1525}
{: release-note}

GPU A100 PCIe virtual server profiles now available (select availability)
:   The A100 PCIe GPU virtual server profiles are now available in the Washington DC (us-east), Frankfurt (eu-de), London (eu-gb), and Tokyo (jp-tok) regions. The profiles run on an [NVIDIA A100 Tensor Core GPU](https://www.nvidia.com/en-us/data-center/a100/) that is tuned for HPC and inferencing workloads. For more information, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles and [NVIDIA A100 instance profiles](/docs/vpc?topic=vpc-accelerated-profile-family&interface=ui#a100-profiles).

### 14 May 2025
{: #vpc-may1425}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-23` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-23`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

Updated `workload` section for Hyper Protect Secure Build
:   The `workload` section for Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-23`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb).

### 12 May 2025
{: #vpc-may1225}
{: release-note}

Red Hat Enterprise Linux AI stock images (GA)
:   The Red Hat Enterprise Linux AI (RHEL AI) 1.x operating system is now available as a stock image. When using the {{site.data.keyword.IBM_notm}}-supplied stock image, the RHEL AI license cost is included in the monthly instance cost. In-place updates of the virtual server instance are not supported. If updates are needed, you need to create a new virtual server with the most recent RHEL AI image, which contains the recent image updates. For more information, see [FAQs about Red Hat and {{site.data.keyword.cloud}} infrastructure](/docs/infrastructure-hub?topic=infrastructure-hub-faqs-for-red-hat-ibm-cloud). For more information about stock images, see [x86 virtual server images](/docs/vpc?topic=vpc-about-images) and [Lifecycle for guest operating systems](/docs/vpc?topic=vpc-guest-os-lifecycle).

### 09 May 2025
{: #vpc-may0925}
{: release-note}

Mount Helper utility supports new OS distributions
:   The IBM Cloud File Share Mount Helper utility can now be installed and used on virtual servers that are running CentOS Stream 9 or 10, and Debian 11 or 12 to establish encrypted connection to file shares. For more information, see the [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility).

### 06 May 2025
{: #vpc-may0625}
{: release-note}

Dynamic network bandwidth control available for Cascade Lake (x2) x86-64 bare metal server profiles
:   Dynamic network bandwidth control is available for Cascade Lake (x2) x86-64 bare metal server profiles. The total network bandwidth for these profiles is now 200 Gbps. For more information, see [x86-64 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui).

AMD Instinct MI300X accelerated virtual server profile now available in Washington DC (`us-east`)(select availability)
:   The AMD Instinct MI300X accelerated virtual server profile is now available in the Washington DC (`us-east`) region. The profile runs on an [AMD Instinct™ MI300X Accelerator](https://www.amd.com/en/products/accelerators/instinct/mi300/mi300x.html){: external} that is tuned for AI workloads, including inferencing and fine-tuning. For more information about the `gx3d-208x1792x8mi300x` profile, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles and [Accelerated instance profiles](/docs/vpc?topic=vpc-accelerated-profile-family&interface=ui).

## April 2025
{: #vpc-apr25}

### 30 April 2025
{: #vpc-apr3025}
{: release-note}

Private path connectivity from IBM Cloud to on-premises locations
:   You can now connect a consumer service running in IBM Cloud to an on-premises provider service, by using an ALB as part of a Private Path NLB pool. This enables access to on-premises resources, while maintaining a private path across IBM Cloud by using the console, CLI, API, or Terraform. For more information, see [Using an ALB with a Private Path NLB to host services outside a VPC](/docs/vpc?topic=vpc-private-path-service-intro&interface=ui#pps-use-case-5).

### 22 April 2025
{: #vpc-apr2225}
{: release-note}

Intel Gaudi 3 accelerated virtual server profile now available in Washington DC (`us-east-wdc07-a`) and Dallas (`us-south-dal12-a`) regions (select availability)
:   The Intel Gaudi 3 accelerated virtual server profile is now available in the Washington DC (`us-east-wdc07-a`) and Dallas (`us-south-dal12-a`) regions, in addition to the Washington DC (`us-east-wdc06-a`) and Frankfurt (`eu-de-fra02-a`) regions. The Intel Gaudi 3 profile runs on an [Intel Gaudi 3 AI Accelerator](https://www.intel.com/content/www/us/en/products/details/processors/ai-accelerators/gaudi.html){: external} that is tuned for AI workloads, including inferencing and fine-tuning. For more information about the `gx3d-160x1792x8gaudi3` profile, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles and [Intel Gaudi 3 instance profiles](/docs/vpc?topic=vpc-accelerated-profile-family&interface=ui#gaudi-3-profiles).

### 17 April 2025
{: #vpc-apr1725}
{: release-note}

NVIDIA Hopper-1 cluster network profile
:   The Hopper-1 cluster network profile is now available for IBM Cloud cluster networks. It provides isolated networks for Hopper HGX instances running workloads that require high-bandwidth, low-latency interconnectivity, such as AI training and large-scale simulations. The Hopper-1 network profile supports both H100 and H200 instance profiles. For more information, see the [NVIDIA Hopper-1 cluster network profile](/docs/vpc?topic=vpc-cluster-network-hopper-1-profile).

Public address ranges (beta release)
:   Accounts that have special approval to preview this feature can now [create a public address range](/docs/vpc?topic=vpc-par-creating&interface=cli). A public address range is a contiguous set of public IP addresses that you can reserve and bind to a VPC in an availability zone. For more information, see [About public address ranges](/docs/vpc?topic=vpc-about-par&interface=cli).

### 10 April 2025
{: #vpc-apr1025}
{: release-note}

Private path connectivity from IBM Cloud to on-premises locations (select availability)
:   Accounts with special approval can now connect a consumer service running in IBM Cloud to an on-premises provider service by using an ALB as a member of a Private Path NLB pool. This allows you to target on-premises resources while maintaining a private path across IBM Cloud. For more information, see [Using an ALB with a Private Path NLB to host services outside a VPC](/docs/vpc?topic=vpc-private-path-service-intro&interface=ui#pps-use-case-5).

### 08 April 2025
{: #vpc-apr825}
{: release-note}

SNI hostname layer-7 rule for application load balancers
:   A new layer 7 rule for policy matching with application load balancers is now available. The `SNI hostname` rule allows you to match policy requests when the server provided in the "server name indication" extension during TLS negotiation matches a specified SNI hostname. For more information, refer to [Policy-based load balancing](/docs/vpc?topic=vpc-layer-7-load-balancing).

Policy based layer-4 load balancing for application load balancers
:   You can now employ layer-4 load balancing policies when using application load balancers. Similar to layer-7 policies, a layer-4 policy is applied with the lowest priority first and only when all of its designated rules are matched. The following actions are supported for layer-4 policies:

   * **Forward To pool** - The request is sent to a specific back-end pool.
   * **Forward To listener** - The request is sent to a specific front-end listener.

   For more information, refer to [Layer-4 policies](/docs/vpc?topic=vpc-layer-7-load-balancing#layer-4-policy).

## March 2025
{: #vpc-mar25}

### 27 March 2025
{: #vpc-mar2725}
{: release-note}

Confidential computing with Intel Trusted Domain Extension (TDX) for Virtual Servers for VPC (select availability)
:   Confidential computing with Intel&reg; Trusted Domain Extension (TDX) for VPC is available only in the Washington DC (us-east) region. Confidential computing with Intel TDX offers confidentiality to virtual machines by providing CPU enhancements that are leveraged by the firmware and hardware to provide confidentiality and integrity. For more information, see [Confidential computing for x86 Virtual Servers for VPC](/docs/vpc?topic=vpc-about-confidential-computing-vpc). When you create a virtual server instance with a confidential computing profile and Intel Trusted Domain Extension (TDX), you can create that virtual server instance only in the Washington DC (us-east) region. You can’t create a virtual server instance with TDX in any other region, including Dallas (us-south) and Frankfurt (eu-de).

### 25 March 2025
{: #vpc-mar2525}
{: release-note}

{{site.data.keyword.logs_full_notm}} private endpoint support for HPVS logging
:   {{site.data.keyword.logs_full_notm}} private endpoint support is now added for HPVS logging. For more information, see [{{site.data.keyword.logs_full_notm}} (ICL)](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc#cloud-logs-provision-instance-ui_exmpl).

Adjustable throughput values for second-generation block volumes
:   Customers with special access to preview the second-generation block storage volumes can now specify custom bandwidth for their data volumes. For more information, see [Block storage volume profiles](/docs/vpc?topic=vpc-capacity-performance&interface=ui#iops-profiles) and [Adjusting the throughput limit of a volume](/docs/vpc?topic=vpc-adjusting-volume-throughput&interface=ui).

New regions for second-generation block storage volumes (select availability)
:   The `sdp` profile is now available in the Frankfurt (`eu-de`), and Tokyo (`jp-tok`) regions. For more information, see
[The SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-about#block-storage-sdp-intro).

Snapshots for second-generation block volumes
:   Customers with special access to preview the second-generation block storage volumes can now create snapshots of these volumes in `us-east` and `eu-de` regions in the console, from the CLI, or with the API. For more information, see [About Block Storage for VPC snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui).

### 19 March 2025
{: #vpc-mar1925}
{: release-note}

Intel Gaudi 3 accelerated virtual server profile now available in Washington DC (`us-east`) and Frankfurt (`eu-de`) regions (select availability)
:   The Intel Gaudi 3 accelerated virtual server profile is now available in the Washington DC (`us-east`) and Frankfurt (`eu-de`) regions. The Intel Gaudi 3 profile runs on an [Intel Gaudi 3 AI Accelerator](https://www.intel.com/content/www/us/en/products/details/processors/ai-accelerators/gaudi.html){: external} that is tuned for AI workloads, including inferencing and fine-tuning. For more information about the `gx3d-160x1792x8gaudi3` profile, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles and [Intel Gaudi 3 instance profiles](/docs/vpc?topic=vpc-accelerated-profile-family&interface=ui#gaudi-3-profiles).

### 17 March 2025
{: #vpc-mar1725}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-21` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-21`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

Updated `workload` section for Hyper Protect Secure Build
:   The `workload` section for Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-21`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb).

### 13 March 2025
{: #vpc-mar1325}
{: release-note}

Montreal region now available
:   The Montreal region is now available for provisioning the [3rd generation](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles) of virtual servers and dedicated hosts. You can provision second-generation [bare metal servers](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui) from the balanced family on Cascade Lakes hardware. For more information, see [IBM Cloud region and data center locations for resource deployment](/docs/overview?topic=overview-locations). Storage services are also available, except for the Cross-regional replication feature for file shares and cross-regional copy feature for block volume snapshots. Block volume snapshots are temporarily routed to Cloud Object Storage in the WDC MZR instead of Montreal to provide proper data protection until a local KMS service is provided in the region.

### 05 March 2025
{: #vpc-mar0525}
{: release-note}

Backup pool support for load balancers
:   You can now manage potential failures in your environment by assigning a failover pool to an existing pool. For more information, refer to [Creating an application load balancer in the console](/docs/vpc?topic=vpc-load-balancers&interface=ui#lb-ui-creating-application-load-balancer).

### 04 March 2025
{: #vpc-mar0425}
{: release-note}

Very High Memory (vx3d) profiles for SAP-HANA now available in Washington DC (select availability)
:  The Very High Memory (vx3d) profiles for SAP-HANA are now available in Washington DC (`us-east`) for both x86-64 profiles and x86-64 dedicated host profiles. This region is in addition to the Toronto (`ca-tor`) region. For more information, see [x86-64 Very High Memory profiles](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory) and [x86-64 dedicated host Very High Memory with instance storage](/docs/vpc?topic=vpc-dh-profiles&interface=ui#dh-vhmemory-intel-x86-64-vx3d). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

## February 2025
{: #vpc-feb25}

### 26 February 2025
{: #vpc-feb2625}
{: release-note}

Workload update for Hyper Protect Secure Build
:   The `workload` section of the Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-20`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb). Clone the latest Secure-Build-Cli to create a Hyper Protect Secure Build server.

### 18 February 2025
{: #vpc-feb1825}
{: release-note}

Storage_generation API property
:   An informational API property is introduced for Block Storage volume profiles, volumes, and snapshots to help identify which storage generation the volumes and snapshots belong to. When you [create a volume](/apidocs/vpc/latest#create-volume), it inherits the generation value from the volume profile that is selected. When you [create a snapshot](/apidocs/vpc/latest#create-snapshot) of a block volume, the snapshot inherits the `storage_generation` from the `source_volume`. Similarly, when a volume is created from a snapshot, it inherits the `storage_generation` value from the snapshot. For more information, see [Viewing available volume profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#using-api-iops-profiles).

Mount Helper utility - new region values
:   After you install the Mount Helper on your virtual server instance, you must specify the region where you want to use the utility to mount file shares. The accepted values for the region are changed to match the VPC region names. The old values are still accepted on existing instances. For more information, see the [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility).

### 14 February 2025
{: #vpc-feb1425}
{: release-note}

Storage optimized profiles are now available in all regions
:   Storage optimized profiles (ox2) are now available in all regions. For more information on Storage optimized profiles, see [x86-64 instance profiles: Storage Optimized](/docs/vpc?topic=vpc-profiles&interface=ui#storageopt).

### 12 February 2025
{: #vpc-feb1225}
{: release-note}

GPU H200 profile now available in Washington DC (`us-east`) and Frankfurt (`eu-de`) regions (select availability)
:   The GPU H200 profile is now available in the Washington DC (`us-east`) and Frankfurt (`eu-de`) regions. The GPU H200 profile is available on the latest generation GPU-enabled infrastructure for running machine learning (ML) and deep learning (DL) frameworks in support of AI initiatives. When you use the H200 virtual server profile, it runs on an [NVIDIA Hopper-based HGX](https://www.nvidia.com/en-us/data-center/hgx/){: external} server and is the sole tenant running on the host. For more information about the `gx3d-160x1792x8h200` profile, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles.

## January 2025
{: #vpc-jan25}

### 14 January 2025
{: #vpc-jan1425}
{: release-note}

Confidential computing with Intel Trusted Domain Extension (TDX) for Virtual Servers for VPC (beta release)
:   Confidential computing with Intel&reg; Trusted Domain Extension (TDX) for VPC is available for select customers. Contact IBM Sales if you are interested in being allowlisted and using this offering. Confidential computing with Intel TDX offers confidentiality to virtual machines by providing CPU enhancements that are leveraged by the firmware and hardware to provide confidentiality and integrity. Confidential computing with Intel TDX for VPC is available only in the Washington DC (us-east) region. For more information, see [Confidential computing for x86 Virtual Servers for VPC](/docs/vpc?topic=vpc-about-confidential-computing-vpc).

## December 2024
{: #vpc-dec24}

### 17 December 2024
{: #vpc-dec1724}
{: release-note}

File share snapshots
:   Snapshots are point-in-time copies of your file share. The file share snapshots can be used to restore individual files, or create other file shares in the same zone with the data that's captured by the snapshot. You can create snapshots manually in the console or from the CLI, and programmatically with the API. You can also schedule the snapshots to be created automatically at regular intervals by using the Backup for VPC service. For more information, see [About {{site.data.keyword.filestorage_vpc_short}} snapshots](/docs/vpc?topic=vpc-fs-snapshots-about).

### 10 December 2024
{: #vpc-dec1024}

Cluster networks for VPC (GA)
:   Cluster Networks for VPC are now generally available. Cluster networks provide high-bandwidth, low-latency networking for workloads such as AI training and large-scale simulations. Review [Planning considerations for cluster networks](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) before you [create a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=ui). Cluster network profiles define the cluster network performance characteristics and capabilities. Learn more about the [H100 cluster network profile](/docs/vpc?topic=vpc-profiles&interface=api#gpu), the first cluster network profile being introduced. It provides a specialized network that implements the RoCEv2 protocol to enable remote direct memory access for your workloads that are running on the `gx3d-160x1792x8h100` instance profile. For more information, see [About cluster networks](/docs/vpc?topic=vpc-about-cluster-network&interface=ui).

### 06 December 2024
{: #vpc-dec0624}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-20` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-20`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

Base64 support for rsyslog configuration
:  The syslog certificates and the key can be provided in Base64 format.
For more information, see [Syslog](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc#syslog).

Updated `workload` section for Hyper Protect Secure Build
:   The `workload` section for Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-20`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb).

## November 2024
{: #vpc-nov24}

### 18 November 2024
{: #vpc-nov1824}
{: release-note}

Reservations for Bare Metal Servers for VPC (GA)
:   Reservations for Bare Metal Servers for VPC is now generally available. A reservation is a great option if you want guaranteed resources for future deployments and cost savings. You can choose between either a 1 or 3-year contract term for your reservation. For more information about reservations, see [About Reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc).

Automatic attachments for Reservations (GA)
:   Automatic attachments for Reservations are now generally available. You can use automatic attachments to automatically attach resources to a reservation. For more information, see [Automatic attachments for Reservations](/docs/vpc?topic=vpc-automatic-reservation-vpc).

### 12 November 2024
{: #vpc-nov1224}
{: release-note}

Private Path services for VPC
: Private Path services are now generally available. Private Path services provide targeted and directional connectivity between VPCs and accounts, allowing only consumers to initiate connections to the provider's service endpoint. Explicit authorization gives providers full control of who can access their services.
A Private Path service requires a Private Path network load balancer to deploy a service on IBM Cloud, as well as a Virtual Private Endpoint (VPE) gateway for consumers to connect to the service. To get started, providers can create a [Private Path service](/docs/vpc?topic=vpc-private-path-service-about&interface=ui).
For more information, see the [Private Path solution guide](/docs/private-path?topic=private-path-overview).
{: note}

Cluster networks for VPC (select availability)
:   Cluster Networks for VPC is now selectively available for select customers. Contact IBM Support if you are interested in using this functionality. Cluster networks provide high-bandwidth, low-latency networking for workloads such as AI training and large-scale simulations. You can now [create cluster networks](/docs/vpc?topic=vpc-create-cluster-network&interface=ui) using a cluster network profile, which defines the cluster network performance characteristics and capabilities. The [H100 cluster network profile](/docs/vpc?topic=vpc-profiles&interface=api#gpu) is the first cluster network profile being introduced. It provides a specialized network that implements the RoCEv2 protocol to enable remote direct memory access for your workloads that are running on the `gx3d-160x1792x8h100` instance profile. For more information, see [About cluster networks](/docs/vpc?topic=vpc-about-cluster-network&interface=ui).

## October 2024
{: #vpc-oct24}

### 31 October 2024
{: #vpc-oct3124}
{: release-note}

End of Market (EOM) announcement for deprecation of Classic access VPC
:   Starting 31 October 2024, the “Classic access” option is no longer available in the IBM Cloud console (UI). If your account doesn't have any classic-access-enabled VPCs, as of 31 December 2024, you can't create VPCs with “Classic access” enabled (including through the CLI, Terraform, and the API). Instead, you can use [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started) to connect your VPCs to the Classic network. If your account has existing classic-access-enabled VPCs, those VPCs can still access the Classic network and you can continue to create classic-access-enabled VPCs in that account through the API and CLI.

### 29 October 2024
{: #vpc-oct2924}
{: release-note}

File share replication enhancement
:   You can now schedule to replicate your data between your source and replica file shares as often as every 15 minutes. This feature is available in the console, from the CLI, or with the API.

Hyper Protect Secure Build
:   The `workload` section of the Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-19`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb). Clone the latest Secure-Build-Cli to create a Hyper Protect Secure Build server.

### 24 October 2024
{: #vpc-oct2424}
{: release-note}

Allow SSH and Allow ping are not selected by default when creating a VPC in {{site.data.keyword.cloud_notm}} console
:   When you create a virtual private cloud by using {{site.data.keyword.cloud_notm}} console, the default security group settings for **Allow SSH** and **Allow ping** are now not selected by default, ensuring the most secure option by default. During VPC creation, you can select **Allow SSH** and **Allow ping** as needed for your VPC configuration. For more information, see [Creating a VPC and subnet](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console&interface=ui#creating-a-vpc-and-subnet).

### 18 October 2024
{: #vpc-oct1824}
{: release-note}

Deploy a route-based VPN in active/active mode
:   When creating connections for a route-based VPN, you can now enable the distribution of traffic between the `Up` tunnels of the VPN gateway connection when a VPC route's next hop is the VPN connection. To accomplish this, you must enable the "distribute traffic" feature when [creating](/docs/vpc?topic=vpc-vpn-create-gateway&interface=ui) or [adding a connection](/docs/vpc?topic=vpc-vpn-adding-connections&interface=ui) to a route-based VPN gateway. For more information, see the [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn&interface=ui#use-case-4-vpn) use case.

### 15 October 2024
{: #vpc-oct1524}
{: release-note}

Linux SysRq key now available to troubleshoot Linux virtual server instance
:   When a Linux virtual server instance becomes unresponsive, you can now use Linux SysRQ key to troubleshoot the virtual server from the serial console. For more information, see [How can I use Linux SysRq key to troubleshoot a Linux virtual server instance from the serial console?](/docs/vpc?topic=vpc-troubleshooting-your-virtual-servers-for-vpc&interface=cli#linux-vsi-serial-console).

### 08 October 2024
{: #vpc-oct0824}
{: release-note}

GPU H100 profile now available in Dallas (`us-south`) and Frankfurt (`eu-de`) regions (select availability)
:   The GPU H100 profile is now available in the Dallas (`us-south`) and Frankfurt (`eu-de`) regions, in addition to London (`eu-gb`), Sydney (`au-syd`), Toronto (`ca-tor`), Madrid (`eu-es`), Washington DC (`us-east`), Tokyo (`jp-tok`), and Sao Paulo (`br-sao`) regions. The GPU H100 profile is available on the latest generation GPU-enabled infrastructure for running machine learning (ML) and deep learning (DL) frameworks in support of AI initiatives. When you use the H100 virtual server profile, it runs on an [NVIDIA Hopper-based HGX](https://www.nvidia.com/en-us/data-center/hgx/){: external} server and is the sole tenant running on the host. For more information about the `gx3d-160x1792x8h100` profile, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles.

## September 2024
{: #vpc-sep24}

### 30 September 2024
{: #vpc-sep3024}
{: release-note}

Tagging VPC routing tables
:   VPC [routing tables](/docs/vpc?topic=vpc-about-custom-routes&interface=ui) now support a cloud resource name (CRN) as an identifier when [creating](/docs/vpc?topic=vpc-create-vpc-routing-table&interface=ui) and [updating](/docs/vpc?topic=vpc-update-vpc-routing-table&interface=ui) routing tables. When [reassigning](/docs/vpc?topic=vpc-attach-subnets-routing-table&interface=ui) the routing table for a subnet, the new routing table for the subnet can now optionally be specified by its CRN.

   Now that routing tables have a CRN, VPC routing tables can be [tagged](/docs/account?topic=account-tag), and access to VPC routing tables can be [controlled by using tags](/docs/account?topic=account-access-tags-tutorial).
   {: note}

### 26 September 2024
{: #vpc-sep2624}
{: release-note}

File Storage for VPC monitoring
:   You can monitor the read and write throughput, read and write IOPS, number of mount targets, and capacity usage of your share over time in the {{site.data.keyword.cloud_notm}} console. For more information, see [Getting started with monitoring](/docs/monitoring?topic=monitoring-getting-started) and [Monitoring metrics for {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-fs-vpc-monitoring-sysdig).

### 25 September 2024
{: #vpc-sep2524}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-19` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-19`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

HPVS supports IBM Cloud Logs
:   HPVS supports IBM Cloud Logs solution. For more information, see [ICL](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc#cloud-logs).

IBM log analysis is deprecated
:   Migrate HPVS instances to IBM Cloud log. For more information on migration, see [Steps to migrate for existing customers](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc#cloud-logs-migrate).

### 24 September 2024
{: #vpc-sep2424}
{: release-note}

Block Storage for VPC snapshots for cross-account restore
:   You can now share a snapshot with another account and allow the other account to create volumes with the snapshot. To do so, you must set up [cross-account authorization](/docs/vpc?topic=vpc-block-s2s-auth) in {{site.data.keyword.iamshort}}, and share the CRN of the snapshot with the other account. The other account's authorized storage administrator can use the CRN to create a volume in the console, from the CLI, with the API, or Terraform. For more information, see [Sharing a snapshot with another account in the console](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-s2s-ui) and [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

Defined performance profile for Block Storage for VPC
:   Select Availability: Customers with special approval to preview the defined performance profile can now provision block storage volumes with the `sdp` profile in WDC and LON MZRs. For more information, see [The defined performance profile](/docs/vpc?topic=vpc-block-storage-about#block-storage-sdp-intro).

### 23 September 2024
{: #vpc-sep2324}
{: release-note}

Backup service integration with Event Notifications
:   Backup jobs that create or delete backup snapshots run according to the backup plan and the retention policy. You can now set up a connection between the Backup service and {{site.data.keyword.en_short}} and receive notifications to your preferred destinations if a backup job fails. For more information, see [Enabling event notifications for Backup for VPC](/docs/vpc?topic=vpc-event-notifications-events).

### 20 September 2024
{: #vpc-sep2024}
{: release-note}

GPU H100 profile now available in Tokyo (`jp-tok`) and Sao Paulo (`br-sao`) regions (select availability)
:   The GPU H100 profile is now available in the Tokyo (`jp-tok`) and Sao Paulo (`br-sao`) regions, in addition to London (`eu-gb`), Sydney (`au-syd`), Toronto (`ca-tor`), Madrid (`eu-es`), and Washington DC (`us-east`). The GPU H100 profile is now available on the latest generation GPU-enabled infrastructure for running machine learning (ML) and deep learning (DL) frameworks in support of AI initiatives. When you use the H100 virtual server profile, it runs on an [NVIDIA Hopper-based HGX](https://www.nvidia.com/en-us/data-center/hgx/){: external} server and is the sole tenant running on the host. For more information about the `gx3d-160x1792x8h100` profile, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles.

Secure boot for Virtual Servers for VPC (GA)
:   Secure boot is now generally available. Secure boot is a security standard that makes sure that your server starts with trusted software by verifying the digital signatures for all code in the boot process. When a server starts in secure boot mode, the firmware checks the signature of the boot software, including UEFI firmware drivers, EFI applications, and the operating system. If the signatures are valid, the server boots, and the firmware grants control to the operating system. Which means that secure boot helps prevent malicious software from loading when your server starts. For more information, see [Secure boot for Virtual Servers for VPC](/docs/vpc?topic=vpc-confidential-computing-with-secure-boot-vpc&interface=ui).

### 18 September 2024
{: #vpc-sep1824}
{: release-note}

Next generation instance profiles available in Sao Paulo (br-sao) region (GA)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are now available in the Sao Paulo (`br-sao`) region, in addition to the Dallas (`us-south`), London (`eu-gb`), Frankfurt (`eu-de`), Washington DC (`us-east`), Toronto (`ca-tor`), Madrid (`eu-es`), Sydney (`au-syd`), Tokyo (`jp-tok`), and Osaka (`jp-osa`) regions. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 17 September 2024
{: #vpc-sep1724}
{: release-note}

Very High Memory profiles for SAP-HANA (select availability)
:   New Very High Memory profiles for SAP-HANA are now available. These profiles are only available in Toronto (`ca-tor`) region. For more information, see [x86-64 Very High Memory profiles](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

Updated API properties returned for volumes with the `sdp` profile (beta release)
:   The volume and volume profile property of `unattached_capacity_update_supported` changed to `adjustable_capacity_states`, and the volume and volume profile property of `unattached_iops_update_supported` changed to `adjustable_iops_states`. These changes applies when [listing](/apidocs/vpc-beta/initial#list-volume-profiles) or [retrieving](/apidocs/vpc-beta/initial#get-volume-profile) a volume profile, [creating](/apidocs/vpc-beta/initial#create-volume) or [updating](/apidocs/vpc-beta/initial#update-volume) a volume, [listing volumes](/apidocs/vpc-beta/initial#list-volumes), and [retrieving a volume](/apidocs/vpc-beta/initial#get-volume). For more information, see [Viewing available volume profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#view-iops-profiles).

### 11 September 2024
{: #vpc-sep1124}
{: release-note}

Hyper Protect Secure Build
:   The `workload` section for Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-18`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb).

### 10 September 2024
{: #vpc-sep1024}
{: release-note}

Zone Maps and Universal Names for Zones
:   Account specific zone mapping has been introduced in all regions. Zones have an extra name that can serve as a global, cross-account identifier. A zone's universal name is documented per region and can be viewed when you view or retrieve a zone. For more information, see [Zone mapping per account](/docs/overview?topic=overview-locations#zone-mapping).

### 05 September 2024
{: #vpc-sep0524}
{: release-note}

Red Hat Enterprise Linux AI BYOL custom images (GA)
:   The Red Hat Enterprise AI (RHEL AI) operating system can be imported as a bring your own license (BYOL). An RHEL AI qcow2 file is available directly from Red Hat. For more information, see [Red Hat Enterprise Linux AI BYOL custom images](/docs/vpc?topic=vpc-planning-custom-images#rhel-ai-byol-custom-images).

### 03 September 2024
{: #vpc-sep0324}
{: release-note}

GPU H100 profile available in select regions (select availability)
:   The GPU H100 profile is now available on the latest generation GPU-enabled infrastructure for running machine learning (ML) and deep learning (DL) frameworks in support of AI initiatives. The GPU H100 profile is available in the following regions: London (`eu-gb`), Sydney (`au-syd`), Toronto (`ca-tor`), Madrid (`eu-es`), and Washington DC (`us-east`). When you use the H100 virtual server profile, it runs on an [NVIDIA Hopper-based HGX](https://www.nvidia.com/en-us/data-center/hgx/){: external} server and is the sole tenant running on the host. For more information about the `gx3d-160x1792x8h100` profile, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles.

## August 2024
{: #vpc-aug24}

### 28 August 2024
{: #vpc-aug2824}

Reservations for Bare Metal Servers for VPC (beta release)
:   Reservations for Bare Metal Servers for VPC is now available as a beta feature. A reservation is a great option if you want guaranteed resources for future deployments and cost savings. You can choose between either a 1 or 3-year contract term for your reservation. For more information about reservations, see [About Reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc).

### 21 August 2024
{: #vpc-aug2124}
{: release-note}

Select availability for {{site.data.keyword.hpvs}} for VPC profiles
:  Instance profiles for the Hyper Protect Virtual Server instances are available in the Dallas (`us-south`), US East (Washington, DC), Toronto (`ca-tor`), São Paulo (`br-sao`), London (`eu-gb`), Frankfurt (`eu-de`), Madrid (`eu-es`), and Tokyo (`jp-tok`) regions.

### 15 August 2024
{: #vpc-aug1524}
{: release-note}

IBM Wazi as a Service available in Frankfurt (`eu-de`) region
:   {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) is now available in the Frankfurt (`eu-de`) region in {{site.data.keyword.cloud_notm}}. For more information, see [{{site.data.keyword.waziaas_full_notm}} product page](https://www.ibm.com/products/wazi-as-a-service){: external}.

### 07 August 2024
{: #vpc-aug0724}
{: release-note}

UI enhancement: Filter instance profiles by business scenario
:   When provisioning a virtual server, you can now use the **By scenario** tab on the Select an instance profile page to narrow the results to include only applicable instance profiles. For example, you can filter profiles by the following business scenarios: SAP; Web Development and Test; HPC; Confidential computing; AI, Deep learning & Machine learning; Visualizations, VDI; and Storage optimized. When a specific filter is selected, the profile results display only the profiles related to the defined business scenario.

### 05 August 2024
{: #vpc-aug0524}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-18` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-18`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

{{site.data.keyword.hpvs}} image now supports IBM Cloud Log analysis renewed certificate that is signed with Digicert Global Root G2.

## July 2024
{: #vpc-jul24}

### 22 July 2024
{: #vpc-jul2224}
{: release-note}

Hyper Protect Secure Build
:   The `workload` section for Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-17`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb).

### 18 July 2024
{: #vpc-jul1824}
{: release-note}

Reinitialization on Bare Metal Servers for VPC (GA)
:   With the new Reinitialization action on Bare Metal Servers for VPC, you can reinitialize the bare metal server. This action is not available if the status is Running or if the bare metal server was provisioned with a boot volume. You can only reinitialize a bare metal server that is stopped or failed. When the bare metal server is reinitialized, the contents of the boot disk are wiped and the specified operating system is installed. The server retains the same physical node, interfaces, IP addresses, and resource IDs. Data on secondary drives is preserved. For more information, see [Managing Bare Metal Servers for VPC](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=ui).

### 08 July 2024
{: #vpc-jul0824}
{: release-note}

Parameterized redirect for application load balancers
:  You can now redirect traffic on Uniform Resource Identifier (URI), as well as other customizable parameters, when creating [load balancer listener policies](/apidocs/vpc/latest#create-load-balancer-listener-policy) using the updated `Redirect to URL` action. You can redirect traffic to a dynamic URL through the application load balancer. You can also enter a static URL or retain the values from the incoming traffic request by using the default values of the URL parameters. This includes the protocol, port, host, path, and query, which, as a combination, makes the URL dynamic. For more information, refer to [Layer 7 load balancing](/docs/vpc?topic=vpc-layer-7-load-balancing#layer-7-policy).

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-17` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-17`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

## June 2024
{: #vpc-jun24}

### 28 June 2024
{: #vpc-jun2824}
{: release-note}

Hyper Protect Secure Build
:   The `workload` section for Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-17`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb).

### 27 June 2024
{: #vpc-jun2724}
{: release-note}

Sapphire Rapids (x3 and x3d) x86-64 bare metal server profiles (Select availability)
:   Sapphire Rapids (x3 and x3d) x86-64 bare metal server profiles are now available in the Dallas (`us-south`) region. For more information, see [x86-64 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 25 June 2024
{: #vpc-june2524}
{: release-note}

Sharing file share data between accounts and services
:   With this new feature, administrators with the correct [authorizations](/docs/vpc?topic=vpc-file-s2s-auth) can share an NFS file system across multiple accounts. It's useful for customers who manage multiple accounts and need to share data across different VPCs. Customer can also share their {{site.data.keyword.filestorage_vpc_short}} shares with the [IBM watsonX](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/welcome-main.html?context=wx){: external} service. For more information, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-cross-account-mount).

### 24 June 2024
{: #vpc-jun2424}
{: release-note}

Update firmware on Bare Metal Servers for VPC (GA)
:  The new Update firmware action on Bare Metal Servers for VPC is now generally available. You can see if a firmware update is available for your bare metal server and also initiate the update. You can use the UI, CLI, and API to update the firmware. In the console, this action is only visible if the server is stopped and there is a firmware update available. It is recommended to back up your bare metal server before any firmware update. For more information, see [Managing Bare Metal Servers for VPC](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=ui).

### 20 June 2024
{: #vpc-jun2024}
{: release-note}

UI Enhancements to Images for VPC
:   The Images for VPC UI includes multiple enhancements. When you click any image name, a side panel is displayed for that specific image. From this Details page, you can review both the Details and IDs for the selected image. You can also click **Continue to provisioning**, which takes you to **Virtual server for VPC**, where you can create a virtual server instance with the selected image.

   Images for VPC also now includes the ability to filter the list of images with the following options.

   - Region
   - Operating system
   - Architecture
   - Deployment target
   - Status

   When you select a catalog image on **Virtual server for VPC** to create a virtual server instance, you are now prompted to **Select version and pricing plan**. From here, you can select the version and pricing plan for the catalog image, which must be completed first. You can then select to **Save** the catalog image.

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-16` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-16`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

Base64 format of the attestationPublicKey
:  Besides the plain text format of the attestation public key, you can also use its base64 format to encrypt the attestation document during the contract preparation. For more information, see [Preparing the attestation](/docs/vpc?topic=vpc-about-contract_se#hpcr_attestation_prepare).

### 06 June 2024
{: #vpc-jun0624}
{: release-note}

Generic operating system custom images with Virtual Server Instances and Bare Metal Servers for VPC (GA)
:   Generic operating system custom images is now generally available. When you create a server on {{site.data.keyword.vpc_full}} (VPC) using an x86 profile, you can use an operating system that is not listed in IBM Cloud by specifying a generic operating system custom image. You can create this custom image by specifying one of the new operating systems with properties that indicate it is generic. When you provision a server by using a generic operating system custom image, most operating system-specific provisioning steps aren't performed, such as console setup and automatic registration. You must provide the appropriate user data if you want your generic operating system custom image to perform these steps. For more information, see [Generic operating system custom images](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#generic-os-custom-images) and [Creating a generic operating system custom image](/docs/vpc?topic=vpc-create-generic-os-custom-image&interface=ui).

Network boot of operating systems with Bare Metal Servers for VPC (GA)
:   Network boot of operating systems is now generally available. When you create a bare metal server on IBM Cloud® Virtual Private Cloud (VPC), you can select to network boot an operating system over the network. The operating system image can be hosted on your own server or on a public server. You can install the booted operating system to a disk or you can run the operating system without a disk. For more information, see [Network booting your own operating system with Bare Metal Servers on VPC](/docs/vpc?topic=vpc-network-boot-bare-metal-servers&interface=ui).

## May 2024
{: #vpc-may24}

### 31 May 2024
{: #vpc-may3124}
{: release-note}

Update firmware on Bare Metal Servers for VPC (beta release)
:   With the new Update firmware action on Bare Metal Servers for VPC, you can see if a firmware update is available for your bare metal server and also initiate the update. You can use the UI, CLI, and API to update the firmware. In the console, this action is only visible if the server is stopped and there is a firmware update available. It is recommended to back up your bare metal server before any firmware update. For more information, see [Managing Bare Metal Servers for VPC](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=ui).

Protocol state filtering on virtual network interfaces can be updated
:   Protocol state filtering works well if the packet forwarding path and the return path are the same, and if the packet forwarding path is never changed. However, the VPC routing table supports two-way ECMP routes. When a two-way ECMP route is configured, the forward path might differ from the return path and protocol state filtering can cause legitimate packets to drop. You can now disable protocol state filtering when intermittent routing issues occur. For more information, see [Protocol state filtering mode](/docs/vpc?topic=vpc-vni-about#protocol-state-filtering).

Third-party image billing and metering (GA)
:   When you select a catalog image, you now have associated billing plans to choose from. Catalog images are billed in one of the following ways.

   * Free trial
   * Useage-based billing
   * BYOL

### 30 May 2024
{: #vpc-may3024}
{: release-note}

Next generation instance profiles available in Tokyo and Osaka regions (select availability)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are now available as a select availability offering in the Tokyo (`jp-tok`) and Osaka (`jp-osa`) regions, in addition to the Dallas (`us-south`), London (`eu-gb`), Frankfurt (`eu-de`), Washington DC (`us-east`), Toronto (`ca-tor`), Madrid (`eu-es`), and Sydney (`au-syd`) regions. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

Security group support for secondary IP addresses (GA)
:   You can now attach both primary and secondary IP addresses to a security group to refine the binding of security groups rules to a particular port IP instead of all IP addresses belonging to the port. Also, security group rules now support both source and destination on ingress and egress rules. This allows customers with multiple, secondary private IP addresses associated with a single vNIC to to apply security group rules to source and destination IP addresses, thus enabling finer granularity in security rules. This enhancement provides the capability to secure the primary IP different from the secondary IP addresses, and also applies to VIP prefixes (custom routes) used with a vNIC with IP spoofing disabled. For more information, see [Applying security group rules to source and destination IP addresses](/docs/vpc?topic=vpc-security-groups-rules&interface=ui).

### 29 May 2024
{: #vpc-may2924}
{: release-note}

Confidential computing with Intel Software Guard Extensions (SGX) for Virtual Servers for VPC (select availability)
:   Confidential computing with Intel&reg; Software Guard Extensions (SGX) protects your data through hardware-based server security by using isolated memory regions that are known as encrypted enclaves. This hardware-based computation helps protect your data from disclosure or modification. Which means that your sensitive data is encrypted while it is in virtual server instance memory by allowing applications to run in private memory space. To use SGX, you must install the SGX drivers and platform software on SGX-capable worker nodes. Then, design your app to run in an SGX environment. Confidential computing with Intel SGX for VPC is available only in US-South (Dallas) region. For more information, see [Confidential computing with Intel Software Guard Extensions (SGX) for Virtual Servers for VPC](/docs/vpc?topic=vpc-about-confidential-computing-vpc).

### 14 May 2024
{: #vpc-may1424}
{: release-note}

Next generation instance profiles available in Sydney region (select availability)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are now available as a select availability offering in the Sydney (`au-syd`) region, in addition to the Dallas (`us-south`), London (`eu-gb`), Frankfurt (`eu-de`), Washington DC (`us-east`), Toronto (`ca-tor`), and Madrid (`eu-es`) regions. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 03 May 2024
{: #vpc-may0324}
{: release-note}

Advertise routes to transit gateway and direct link for ingress routing integration
:   VPN for VPC now allows route advertisement so that on-prem CIDR blocks can be advertised to other VPCs without creating address prefixes. For more information, see [Configuring route propagation for VPN gateways](/docs/vpc?topic=vpc-advertise-routes-s2s). For more information about migrating to advertised routes, see [VPN for VPC migration to advertise routes](/docs/vpc?topic=vpc-advertise-routes-s2s#migrate-to-advertise-routes-s2s).

   Client VPN for VPC now allows route advertisement so that on-prem CIDR blocks can be advertised to other VPCs by creating a `deliver` action instead of a `translate` action. For more information, see [Configuring route propagation for VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation). For more information about migrating to advertised routes, see [Client VPN for VPC migration to advertise routes](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation#advertise-routes-c2s).

VPN for VPC: Configurable IKE identity and peer FQDN
:    When you configure a VPN gateway connection, you can now specify a peer FQDN as the peer gateway address. This allows you to use a dynamic public IP on the peer gateway. The VPN gateway connection also supports configuring the IKE identity with supported types: IPv4 address, FQDN, Hostname, and Key ID. The default local IKE identity value is the public IP address of the active member of the VPN gateway while the default peer IKE identity value is the peer gateway address or FQDN.

   You can control which side initiates IKE protocol negotiations and rekeying processes on the VPN gateway connection. By default, the VPN gateway initiates IKE protocol negotiations and rekeying processes while also accepting IKE protocol negotiations or rekeying from the peer gateway. You can disable the VPN gateway from initiating IKE protocol negotiations and rekeying processes, and instead accept only the peer gateway to initiate IKE protocol negotiations and rekeying processes by setting Establish mode to `Peer only`. This enhancement enables you to connect the peer gateway behind a firewall and avoid conflicts in IKE negotiations. For more information, see [Creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway&interface=ui).

Ubuntu 24.04 now available to provision virtual servers
:    Support for the Ubuntu 24.04 release, codenamed "Noble Numbat", is now available. You can use the *ibm-ubuntu-24-04-minimal-amd64-1* stock image to provision virtual server instances. After the virtual server is created, you can also use it as a starting point for a custom image by creating an image from the boot volume of the virtual server.

### 02 May 2024
{: #vpc-may0224}
{: release-note}

GPU l4 and l40S profiles now available in Brazil region (GA)
:   The `l4` and `l40S` GPU profiles are now available in the São Paulo (`br-sao`) region. With this additional region, these profiles are now available in all regions. For more information, see [GPU x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

## April 2024
{: #vpc-apr24}

### 18 April 2024
{: #vpc-apr1824}
{: release-note}

Third-party image billing and metering (beta release)
:   When you select a catalog image, you now have associated billing plans to choose from. Catalog images are billed in one of the following ways:

   * Free trial
   * Usage-based billing
   * BYOL

### 09 April 2024
{: #vpc-apr0924}
{: release-note}

Generic operating system custom images with Virtual Server Instances and Bare Metal Servers for VPC (beta release)
:   When you create a server on {{site.data.keyword.vpc_full}} (VPC) using an x86 profile, you can use an operating system that is not listed in IBM Cloud by specifying a generic operating system custom image. You can create this custom image by specifying one of the new operating systems with properties that indicate it is generic. When you provision a server by using a generic operating system custom image, most operating system-specific provisioning steps aren't performed, such as console setup and automatic registration. You must provide the appropriate user data if you want your generic operating system custom image to perform these steps. For more information, see [Generic operating system custom images](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#generic-os-custom-images) and [Creating a generic operating system custom image](/docs/vpc?topic=vpc-create-generic-os-custom-image&interface=ui).

Network boot of operating systems with Bare Metal Servers for VPC (beta release)
:   When you create a bare metal server on {{site.data.keyword.vpc_full}} (VPC), you can select to network boot an operating system over the network. The operating system image can be hosted on your own server or on a public server. You can install the booted operating system to a disk or you can run the operating system without a disk. For more information, see [Network booting your own operating system with Bare Metal Servers on VPC](/docs/vpc?topic=vpc-network-boot-bare-metal-servers&interface=ui).

### 08 April 2024
{: #vpc-april0824}
{: release-note}

Next generation instance profiles available in Madrid region (select availability)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are now available as a select availability offering in the Madrid (`eu-es`) region, in addition to the Dallas (`us-south`), London (`eu-gb`), Frankfurt (`eu-de`), Washington DC (`us-east`), and Toronto (`ca-tor`) regions. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 03 April 2024
{: #vpc-april0324}
{: release-note}

Next generation instance profiles available in Toronto region (select availability)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are now available as a select availability offering in the Toronto (`ca-tor`) region, in addition to the Washington DC (`us-east`), Dallas (`us-south`), London (`eu-gb`), and Frankfurt (`eu-de`) regions. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

## March 2024
{: #vpc-mar24}

### 29 March 2024
{: #vpc-mar2924}
{: release-note}

Sharing DNS resolution for endpoint gateways across VPCs
:   When multiple VPCs are connected together using Transit Gateway, Direct Link, or other connectivity options, a VPC in the connected topology can now be enabled as a DNS hub to centralize the DNS resolution for Virtual Private Endpoint (VPE) gateways. For more information, see [About DNS sharing for VPE gateways](/docs/vpc?topic=vpc-vpe-dns-sharing).

### 28 March 2024
{: #vpc-mar2824}
{: release-note}

VMware ESXi 7 End of Market for Bare Metal for VPC
:   VMware ESXi image on Bare Metal Servers for VPC will no longer be available when you provision a bare metal for VPC servers. If looking to deploy a VMware solution in VPC, consider provisioning VMware Cloud Foundation (VCF) through IBM Cloud VMware Solutions. For more information about this solution, see the [VMware Cloud Foundation overview](/docs/vmwaresolutions?topic=vmwaresolutions-vpc-vcf-ovw). For more information on the updated packaging and pricing for VMware® portfolio, see [Packaging and pricing for VMware by Broadcom](/docs/vmwaresolutions?topic=vmwaresolutions-vmwaresol_packaging-pricing).

Next generation instance profiles available in Washington DC region (select availability)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are now available as a select availability offering in the Washington DC (`us-east`) region, in addition to the Dallas (`us-south`), London (`eu-gb`), and Frankfurt (`eu-de`) regions. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 26 March 2024
{: #vpc-mar2624}
{: release-note}

Security group support for secondary IP addresses (select availability)
:   Accounts that are granted special approval to preview this feature can now attach both primary and secondary IP addresses to a security group to refine the binding of security groups rules to a particular port IP instead of all IP addresses belonging to the port. Also, security group rules now support both source and destination on ingress and egress rules. This allows customers with multiple, secondary private IP addresses associated with a single vNIC to to apply security group rules to source and destination IP addresses, thus enabling finer granularity in security rules. This enhancement provides the capability to secure the primary IP different from the secondary IP addresses, and also applies to VIP prefixes (custom routes) used with a vNIC with IP spoofing disabled. For more information, see [Applying security group rules to source and destination IP addresses](/docs/vpc?topic=vpc-security-groups-rules&interface=ui).

### 25 March 2024
{: #vpc-mar2524}

Reservations for VPC (GA)
:   Reservations for VPC are now GA (generally available). A reservation is a great option if you want guaranteed resources for future deployments and cost savings. You can choose between either a 1 or 3-year contract term for your reservation. Reservations are available the Dallas (`us-south`), Washington DC (`us-east`), São Paulo (`br-sao`), Toronto (`ca-tor`), London (`eu-gb`), Frankfurt (`eu-de`), Madrid (`eu-es`), Osaka (`jp-osa`), Tokyo (`jp-tok`), and Sydney (`au-syd`) multizone regions (MZRs). For more information, see [About reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc).

### 22 March 2024
{: #vpc-mar2224}
{: release-note}

Private Path Services for VPC (beta release)
:   Accounts that are granted special approval to preview this feature can now create a [Private Path service](/docs/vpc?topic=vpc-private-path-service-intro&interface=ui) and [Private Path network load balancer](/docs/vpc?topic=vpc-ppnlb-ui-creating-private-path-network-load-balancer&interface=ui).

   Private Path services provide private connectivity for IBM Cloud and third-party services. A Private Path service requires a Private Path network load balancer to deploy a service on IBM Cloud and a Virtual Private Endpoint (VPE) gateway for consumers to connect to the service. Traffic stays on the IBM Cloud backbone without traversing the public internet.

   For more information, see the [Private Path solution guide](/docs/private-path?topic=private-path-overview). If you are interested in getting early access to this beta offering, contact your IBM Support representative.
   {: note}

### 21 March 2024
{: #vpc-mar2124}
{: release-note}

UI Enhancement to SSH Keys
:   The UI process for uploading SSH keys is improved where you **Select SSH key input method**. Improvements were made to the ability to copy and paste, drag and drop, and the upload buttons. Previously, these three actions were all individually located. These actions are now combined to use the same space within the console.

### 15 March 2024
{: #vpc-mar1524}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-15` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-15`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

:   You can also set the expiry of contract during signature. Contract signature is an optional feature that can be used with the contract. You can choose to sign a contract before it is passed as input. Contracts that are in plain text or encrypted can be signed. Certificate can also be parsed as base64 string. For more information, see [Contract Signature](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_sign)

:   Validation of the contract signature is done by the {{site.data.keyword.hpvs}} for VPC image. You can validate the certificates that you download for contract encryption and attestation. For more information, see [Validating the Certificates](/docs/vpc?topic=vpc-cert_validate)

Updated `workload` section for Hyper Protect Secure Build
:   The `workload` section for Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-15`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb).

From April 15, only the latest IBM Hyper Protect Container Runtime image will be available on IBM Cloud GUI. For more information, see [Downloading the encryption certificate and extracting the public key](/docs/vpc?topic=vpc-about-contract_se#encrypt_downloadcert)
{: note}

### 12 March 2024
{: #vpc-mar1224}
{: release-note}

Virtual Network Interfaces for VPC
:   A new feature is generally available in the Virtual Private Cloud (VPC) service that expands the support for [virtual network interfaces](/docs/vpc?topic=vpc-vni-about). The following features are available.
   - Virtual network interfaces have an independent lifecycle, which means that when you delete a resource to which the virtual network interface is attached, the virtual network interface persists and retains its IP address.
   - New instances and bare metal servers can be created with virtual network interfaces attached to new child resources called network attachments.
   - Virtual network interfaces support secondary IP addresses.
   - For compatibility with existing clients, instances and bare metal servers with virtual network interfaces include a read-only representation of their network attachments and virtual network interfaces as legacy network interface child resources.
   - For instances and bare metal servers with virtual network interfaces, the IAM permissions for options to allow IP spoofing or disable infrastructure NAT are managed on their attached virtual network interfaces.
   - Flow log collectors can target instance network attachments and virtual network interfaces.

   You can choose to defer access to this feature through [IBM Support](/unifiedsupport/supportcenter). Users in an account that has deferred access will not be able to create instances or bare metal servers with virtual network interfaces. If you need more time to assess, remediate, and test changes for virtual network interfaces, request deferral for your production accounts while you complete the mitigations using your test accounts.
   {: note}

GPU l4 and l40S profiles now available in US South region
:   The `l4` and `l40S` GPU profiles are now available Dallas (`us-south`) region. For the `l4` profiles, these regions are in addition to Washington DC (`us-east`), Toronto (`ca-tor`), London (`eu-gb`), Frankfurt (`eu-de`), Madrid (`eu-es`), Sydney (`au-syd`), and Tokyo (`jp-tok`) regions. For the `l40S` profiles, this region is in addition to Washington DC (`us-east`), Toronto (`ca-tor`), London (`eu-gb`), Frankfurt (`eu-de`), Madrid (`eu-es`), Sydney (`au-syd`), and Tokyo (`jp-tok`) regions. For more information, see [GPU x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 11 March 2024
{: #vpc-mar1124}
{: release-note}

GPU l40S profiles now available in Canada and United Kingdom regions
:   The `l40S` GPU profiles are now available in the Toronto (`ca-tor`) and London (`eu-gb`) regions. These regions are in addition to Washington DC (`us-east`), Frankfurt (`eu-de`), Madrid (`eu-es`), Sydney (`au-syd`), and Tokyo (`jp-tok`) regions. For more information, see [GPU x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 07 March 2024
{: #vpc-mar0724}
{: release-note}

UI update for Block storage
:    When you create a {{site.data.keyword.block_storage_is_short}} volume from the Block storage volumes for VPC list, you can now choose to import data from a snapshot and to apply a backup policy as part of the new **Optional configurations** section.

### 06 March 2024
{: #vpc-mar0624}
{: release-note}

GPU l4 and l40S profiles now available in Washington DC (`us-east`) region
:   The `l4` and `l40S` GPU profiles are now available in the Washington DC (`us-east`) region. For more information, see [GPU x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

## February 2024
{: #vpc-feb24}

### 29 February 2024
{: #vpc-feb2924}
{: release-note}

UI navigation change to Auto scale
:   Previously, Auto scale was found in VPC Infrastructure > Auto scale in the IBM Console navigation. This path is now changed. The new navigation path is **VPC Infrastructure > Compute**.

### 27 February 2024
{: #vpc-feb2724}
{: release-note}

GPU l40S profiles with PCIe now available
:   New `l40S` GPU profiles that include NVIDIA's L40S 48-GB GPU are now available in the Frankfurt (`eu-de`), Madrid (`eu-es`), Sydney (`au-syd`), and Tokyo (`jp-tok`) regions. For more information, see [GPU x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

GPU l4 profiles now available
:   New `l4` GPU profiles that include NVIDIA's L4 24-GB GPU are now available in the Toronto (`ca-tor`), London (`eu-gb`), Frankfurt (`eu-de`), Madrid (`eu-es`),  Sydney (`au-syd`), and Tokyo (`jp-tok`) regions. For more information, see [GPU x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 23 February 2024
{: #vpc-feb2324}
{: release-note}

London 1 AZ for bare metal servers
:   The London 1 availability zone (AZ) is now available for Bare Metal Servers for VPC.

### 20 February 2024
{: #vpc-feb2024}
{: release-note}

Virtual Network Interfaces for VPC (Select availability)
:   Accounts that have special approval can preview a new feature in the Virtual Private Cloud (VPC) service that expands the support for [virtual network interfaces](/docs/vpc?topic=vpc-vni-about). The following features are available.
   - Virtual network interfaces have an independent lifecycle, which means that when you delete a resource to which the virtual network interface is attached, the virtual network interface persists and retains its IP address.
   - New instances and bare metal servers can be created with virtual network interfaces attached to new child resources called network attachments.
   - Virtual network interfaces support secondary IP addresses.
   - For compatibility with existing clients, instances and bare metal servers with virtual network interfaces include a read-only representation of their network attachments and virtual network interfaces as legacy network interface child resources.
   - For instances and bare metal servers with virtual network interfaces, the IAM permissions for options to allow IP spoofing or disable infrastructure NAT are managed on their attached virtual network interfaces.
   - Flow log collectors can target instance network attachments and virtual network interfaces.

   If you have automation for managing your virtual network interfaces, bare metal servers, and file share mount targets, and you are not interested in expanded support for virtual network interfaces, you'll have the option to opt out when the feature becomes generally available.
   {: note}

Next generation instance profiles available in Frankfurt (`eu-de`) region (select availability)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are now available as a select availability offering in the Frankfurt (`eu-de`) region, in addition to the Dallas (`us-south`) and London (`eu-gb`) regions. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 13 February 2024
{: #vpc-feb1324}
{: release-note}

Dashboard template IDs
:  Template IDs and dashboard names for Virtual server for VPC, Flow logs for VPC, and VPC Infrastructure Service Resource Quota Overview are changed.
- *VPC service metric definitions dashboard names:*
   - `VPC VSI Gen 2 Overview` is changed to `Virtual Server for VPC Overview`
   - `VPC Flow Logs Overview` is changed to `Flow Logs for VPC Overview`
   - `VPC Resource Quota Overview` is changed to `VPC Infrastructure Service Resource Quota Overview`
- *VPC virtual server instances metrics definitions:*
   - `ibm_resource_name` is changed to `ibm_is_resource_name`
- *Monitoring flow logs for VPC metrics:*
   - `ibm_flow_log_collector_instance` is changed to `ibm_is_flow_log_collector_instance`
- *VPC Infrastructure Service Resource Quota Overview:*
   - `ibm_secondary_resource_id` is changed to `ibm_is_secondary_resource_id`
   - `ibm_resource_quota_name` is changed to `ibm_is_resource_quota_name`

For more information, see [IBM Cloud VPC monitoring dashboards](/docs/vpc?topic=vpc-ibm-monitoring#vpc-metric-definitions).

### 07 February 2024
{: #vpc-feb0724}
{: release-note}

Updated `workload` section for Hyper Protect Secure Build
:   The `workload` section for Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-14`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb).

UI enhancement: New SAP Certified filter when selecting an instance profile
:   When provisioning a virtual server, you can now use the **SAP Certified** filter on the Select an instance profile page to narrow the results to include only the available SAP profiles (SAP HANA, SAP NetWeaver, or SAP Business One). When the SAP Certified filter is selected, the profile results display the SAP certification status for the specific SAP enabled profiles.

### 06 February 2024
{: #vpc-feb0624}
{: release-note}

New Madrid (`eu-es`) region for ux2d profiles (GA)
:   The Ultra High Memory family of profiles are now available in the Madrid (`eu-es`) region. The addition of this region makes the ux2d profiles available in all regions. For more information, see the [Ultra High Memory](/docs/vpc?topic=vpc-profiles&interface=ui#uhmemory) profile information. For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

## January 2024
{: #vpc-jan24}

### 24 January 2024
{: #vpc-jan2424}
{: release-note}

Next generation instance profiles available in London (`eu-gb`) region (select availability)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are now available as a Select Availability offering in the London (`eu-gb`) region, in addition to the Dallas (`us-south`) region. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 16 January 2024
{: #vpc-jan1624}
{: release-note}

Reservations for VPC (select availability)
:   You can now provision reservations for VPC. A reservation is a great option if you want guaranteed resources for future deployments and cost savings. You can choose between either a 1 or 3-year contract term for your reservation. Reservations are available in only the Sydney (`au-syd`) region. For more information, see [About reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc).

### 11 January 2024
{: #vpc-jan1124}
{: release-note}

UI enhancement to VPC download button
:   Previously, when you downloaded a list of resources from a table, you could download only the current page if the resource list length was more than 200 records. With this UI enhancement, you can now download all the pages regardless of length of the resource list.

## December 2023
{: #vpc-dec23}

### 19 December 2023
{: #vpc-dec1923}
{: release-note}

Corrected events
:   The following table shows activity tracking events that have been corrected.

| Incorrect event | Corrected events  |
|:----------------|:-----------------------|
|`is.bare-metal-server.network-interface.floating-ip.attach`|  - `is.bare-metal-server.network-interface.attach` \n  - `is.floating-ip.floating-ip.attach`|
|`is.bare-metal-server.network-interface.floating-ip.detach`| - `is.bare-metal-server.network-interface.attach` \n  - `is.floating-ip.floating-ip.attach` |
|`is.instance.network-interface_floating-ip.detach`         | - `is.floating-ip.floating-ip.detach` \n  - `is.instance.network-interface.detach`  |
| `is.instance.network-interface_floating-ip.attach`        | - `is.floating-ip.floating-ip.attach` \n  - `is.instance.network-interface.attach`  |
| `is.subnet.public-gateway.detach`                         | - `is.public-gateway.public-gateway.detach` \n  - `is.subnet.subnet.detach` |
| `is.subnet.public-gateway.attach`                         | - `is.public-gateway.public-gateway.attach` \n  - `is.public-gateway.public-gateway.detach` \n  - `is.subnet.subnet.attach` \n  - `is.subnet.subnet.detach` |
| `is.subnet.routing-table.attach`                          |- `is.subnet.subnet.attach` \n  - `is.subnet.subnet.detach` \n  - `is.vpc.routing-table.attach` \n  - `is.vpc.routing-table.detach`  |
{: caption="Corrected events" caption-side="bottom"}

### 15 December 2023
{: #vpc-dec1523}
{: release-note}

IBM Wazi as a Service and LinuxONE (s390x processor architecture) dedicated host (select availability)
:   You can now create dedicated hosts with s390x memory profiles in the Madrid (`eu-es`) and Dallas (`us-south`) regions to carve out a single-tenant compute node and create virtual server instances according to your needs. For more information, see [s390x dedicated host profiles](/docs/vpc?topic=vpc-s390x-dh-profiles) and [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances).

VPC route advertisement to Direct Link and Transit Gateway
:  You can now advertise routes in VPC ingress routing tables to Direct Link, Transit Gateway, or both. For more information, see [VPC routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

### 14 December 2023
{: #vpc-december1423}
{: release-note}

Tokyo 2 AZ for bare metal servers
:  The Tokyo 2 availability zone (AZ) is now available for Bare Metal Servers for VPC.

### 12 December 2023
{: #vpc-december1223}
{: release-note}

File Storage for VPC - Cross-region Replication
:  Customers who have VPCs in multiple regions in the same geography can now create replicas of their file shares in another zone of a different region. For more information, see [About file share replication](/docs/vpc?topic=vpc-file-storage-replication).

### 08 December 2023
{: #vpc-dec0823}
{: release-note}

GPU A100 profiles with PCIe now available
:   Two additional `a100` GPU profiles that include NVIDIA's A100 PCIe GPU are now available in the Washington DC (`us-east`), Tokyo (`jp-tok`), and London (`eu-gb`) regions. For more information, see [GPU x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 05 December 2023
{: #vpc-dec0523}
{: release-note}

Snapshot consistency groups and consistency group backups
:   You can now create a snapshot consistency group to capture snapshots of multiple block storage volumes that are attached to a virtual server instance. You can include or exclude the boot volume. Instance storage is not included. You can later use the individual snapshots in the consistency group to restore multiple volumes of a virtual server instance. You can automate the creation and retention of consistency group snapshots with the Backup service. For more information, see [Snapshot consistency groups](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#multi-volume-snapshots) and [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-14` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-14`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

### 01 December 2023
{: #vpc-dec0123}
{: release-note}

Reserved Capacity for VPC (beta release)
:   You can now reserve capacity for VPC. Reserved capacity is a great option if you want guaranteed resources for future deployments and cost savings. You can choose between either a 1 or 3-year contract term for your reserved capacity. For more information, see [About Reserved Capacity for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc).

## November 2023
{: #vpc-nov23}

### 16 November 2023
{: #vpc-nov1623}
{: release-note}

Client VPN for VPC: Automate the client certificate authentication process for private certificates
: As a VPN server administrator, you were required to download the client profile, manually insert the private certificate into the client profile, and, finally, distribute it to users. Now, when a private certificate is used for client authentication, you can download the client profile with the merged private certificate and key for *all* or *selected* private certificates. There is also no need for the VPN client user to modify their client profile manually. For more information, see [Setting up a client VPN environment and connecting to a VPN server](/docs/vpc?topic=vpc-vpn-client-environment-setup).

Encryption in transit is now available in Madrid (`eu-es`) region
:   Encryption in transit for {{site.data.keyword.filestorage_vpc_short}} is now available in the Madrid (`eu-es`) region in {{site.data.keyword.cloud_notm}}. For more information, see [Encryption in transit](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-eit).

### 15 November 2023
{: #vpc-nov1523}
{: release-note}

GPU A100 profile available on Intel Ice Lake hardware in Washington DC (`us-east`) region (select availability)
:   The GPU `a100` profile is now available on the Intel&reg;'s quad processor Xeon® Gold 6342 Ice Lake with 96 cores that are running at a base speed of 2.8 GHz and an all-core turbo frequency of 3.5 GHz. The Ice Lake processor is available only in the Washington DC (`us-east`) region. For more information, see the [GPU profile family](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) documentation.

### 13 November 2023
{: #vpc-nov1323}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-13` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-13`, new certificates are available.
   - [Attestation certificate](/docs/vpc?topic=vpc-about-attestation)
   - [Encryption certificate](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt)
   - [Intermediate certificate](/docs/vpc?topic=vpc-cert_validate#download_cert)

:   You can attach multiple volumes when you bring up the virtual server instance. For more information, see [The workload - volumes subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_volumes), and [The env - volumes subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_env_vol).

Updated `workload` section for Hyper Protect Secure Build
:   The `workload` section for Hyper Protect Secure Build is updated based on the IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-13`. For more information, see [Configuring and using Hyper Protect Secure Build in {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-about-hpsb#hpvs_hpsb).

### 10 November 2023
{: #vpc-nov1023}
{: release-note}

IBM Wazi as a Service available in Dallas (`us-south`) region
:   {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) is now available in the Dallas (`us-south`) region in {{site.data.keyword.cloud_notm}}. For more information, see [{{site.data.keyword.waziaas_full_notm}} product page](https://www.ibm.com/products/wazi-as-a-service){: external}.

Dallas (`us-south`) region is now available for IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}}
:   You can create IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instances on LinuxONE (s390x processor architecture) in the Dallas (`us-south`) region, in addition to Tokyo (`jp-tok`), São Paulo (`br-sao`), Madrid (`eu-es`), Toronto (`ca-tor`), London (`eu-gb`), and Washington DC (`us-east`) regions. To create IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers), and [IBM Hyper Protect Container Runtime image](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime). A valid contract is required for creating an instance. For more information, see [About the contract](/docs/vpc?topic=vpc-about-contract_se&interface=ui).

Dallas (`us-south`) region is now available for LinuxONE (s390x processor architecture)
:   You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud in the Dallas (`us-south`) region, in addition to Tokyo (`jp-tok`), São Paulo (`br-sao`), Madrid (`eu-es`), Toronto (`ca-tor`), London (`eu-gb`), and Washington DC (`us-east`) regions. For more information about available LinuxONE (s390x processor architecture) profiles, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

UI enhancement: architecture and image selection when provisioning
:   When you create resources such as virtual server instances, bare metal servers, and instance templates, the image selection option is now available in an enhanced side panel when you click **Change image**. For virtual server instances and instance templates, you can select from stock images, custom images, catalog images, snapshots, and existing volumes. For bare metal servers, you can select from stock images and custom images. Additionally, architecture selection is now included on the image side panel.

## October 2023
{: #vpc-oct23}

### 24 October 2023
{: #vpc-oct2423}
{: release-note}

Security group integration for network load balancers
:   For enhanced security, network load balancers can now be associated with security groups. You can associate one or more security groups with a new network load balancer when creating it, as well as associate security groups with your existing network load balancers. For more information, see [Integrating an IBM Cloud Network Load Balancer for VPC with security groups](/docs/vpc?topic=vpc-nlb-integration-with-security-groups).

 Very High Memory profiles available in all regions (GA)
:   The Very High Memory family of profiles are now available in the Madrid (`eu-es`) region. This makes the vx2d profiles available in all regions. For more information about the Very High Memory profile family, see [Very High Memory](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 12 October 2023
{: #vpc-oct1223}
{: release-note}

VPNs for VPC: Diagnose unhealthy VPN gateways and servers
:   When you see an existing VPN gateway or server in a `degraded` or `faulted` state, you can now diagnose the issue. You are presented with reasons for the state and actions to resolve the issue. For more information, see [Diagnosing VPN gateway health](/docs/vpc?topic=vpc-vpn-health), [Diagnosing VPN gateway connection health](/docs/vpc?topic=vpc-vpn-health#vpn-connection-health), and [Diagnosing VPN server health](/docs/vpc?topic=vpc-vpn-server-health).

Next generation instance profiles available in Dallas (`us-south`) region (select availability)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are available as a Select Availability offering in the Dallas (`us-south`) region. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

UI enhancements
:   The following enhancements were made to the VPC UI.

   * General fixes and updates
   * VPN status reason and suspended status
   * VPN Route Advertisement

### 02 October 2023
{: #vpc-oct0223}
{: release-note}

New regions for ux2d profiles
:   The Ultra High Memory family of profiles are now available in the São Paulo (`br-sao`), Tokyo (`jp-tok`), Osaka (`jp-osa`), and Sydney (`au-syd`) regions. For more information, see the [Ultra High Memory](/docs/vpc?topic=vpc-profiles&interface=ui#uhmemory) profile information. For more information about the Multizone regions, [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).


## September 2023
{: #vpc-sep23}

### 29 September 2023
{: #vpc-september2923}
{: release-note}

Backup as a Service Enterprise enablement
:   As an enterprise account administrator, you can view and manage the backup policies and plans for the subaccounts for compliance reporting and billing from one place. Enterprise account users can see all backup policies and associated jobs. They can also see the reference of the backup snapshot that is created in the subaccount. Subaccounts can create and manage their backups as before. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

### 22 September 2023
{: #vpc-sep2223}
{: release-note}

IBM Wazi as a Service available in Madrid (`eu-es`) region
:   {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) is now available in the Madrid (`eu-es`) region in IBM Cloud. For more information, see [{{site.data.keyword.waziaas_full_notm}} product page](https://www.ibm.com/products/wazi-as-a-service){: external}.

### 21 September 2023
{: #vpc-sep2123}
{: release-note}

IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}}
:   You can now create IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instances on LinuxONE (s390x processor architecture) in the Madrid (`eu-es`) region, in addition to São Paulo (`br-sao`), Toronto (`ca-tor`), Tokyo (`jp-tok`), and Washington DC (`us-east`) regions. To create IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers), and [IBM Hyper Protect Container Runtime image](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime). A valid contract is required for creating an instance. For more information, see [About the contract](/docs/vpc?topic=vpc-about-contract_se&interface=ui).

LinuxONE (s390x processor architecture)
:   You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud in the Madrid (`eu-es`) region, in addition to Tokyo (`jp-tok`), São Paulo (`br-sao`), Toronto (`ca-tor`), and London (`eu-gb`) regions. For more information about available LinuxONE (s390x processor architecture) profiles, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

### 14 September 2023
{: #vpc-september2623}
{: release-note}

UI Enhancements for Backup plans
:   You can now specify both age and count when you define the retention policy of a backup plan.

### 05 September 2023
{: #vpc-sep0523}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-12` updates
:   For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-12`, new certificates are available.
   - Attestation certificate
   - Encryption certificate
   - Intermediate certificate

:   You can now roll or rotate the seeds that are used in the contract to improve the security posture or if the seed is compromised. For more information, see [The workload - volumes subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_volumes), and [The env - volumes subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_env_vol).

Ultra High Memory profiles are now available in the London (`eu-gb`) region
:   The Ultra High Memory profile family (`ux2d`) is now available in the United Kingdom (London) region. For more information about this profile family, see [Ultra High Memory](/docs/vpc?topic=vpc-profiles&interface=ui#uhmemory). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).


## August 2023
{: #vpc-august23}

### 31 August 2023
{: #vpc-august3123}
{: release-note}

Bare metal network hardware
:   Bare metal servers now use upgraded network cards. For network workloads that leverage very high packets per second for smaller packets, you can update your drivers to the latest available Pensando device drivers. For more information, see [Special considerations for bare metal network performance upgrade](/docs/vpc?topic=vpc-bare-metal-image#bare-metal-pensando-considerations) and [AMD Pensando Support](https://www.amd.com/en/support/accelerators/pensando.html){: external}.

File Storage for Bare metal servers for VPC
:   File Storage for VPC is now supported by Bare Metal Servers for VPC. Users can leverage file storage as an addition or alternative to local NVMe drives. [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about).

### 25 August 2023
{: #vpc-aug2523}
{: release-note}

Next generation instance profiles (beta release)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are available as a beta offering to select customers. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles) and the *bx3d* and *mx3d* profiles in the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) and [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) profile families.

### 22 August 2023
{: #vpc-august2223}
{: release-note}

VPC Status History
:   You can now customize notification settings for your VPC dashboard. When status history is enabled, your notification history is retained to help you find old error messages or track down when the creation or deletion of a resource occurred. To enable Status History, make sure that your browser allows local storage access. For more information, see [Enabling local storage in your browser](/docs/vpc?topic=vpc-enable-local-browser-storage).

### 15 August 2023
{: #vpc-august1523}
{: release-note}

Metadata Instance identity certificates
:   You can now use the instance identity access token and a Certificate Signing Request (CSR) to [create](/apidocs/vpc-metadata-beta/initial#create-certificate) an instance identity certificate with the Metadata API. For more information, see [Generating an instance identity certificate by using an instance identity access token](/docs/vpc?topic=vpc-imd-identity-operations&interface=api#imd-acquire-certificate). Instance identity certificates can be used when the traffic between an authorized client and the mounted file share is [encrypted in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit).

### 08 August 2023
{: #vpc-august0823}
{: release-note}

File Storage for VPC (GA)
:   NFS-based file shares for a zone within a region are now generally available. You can create and share file storage over multiple virtual service instances within the same zone across multiple VPCs. For more information about this service, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about).

## July 2023
{: #vpc-jul23}

## 21 July 2023
{: #vpc-july2123}
{: release-note}

VPC services using IBM Cloud Metrics Routing
:    You can use IBM Cloud Metrics Routing to manage metrics at the account-level by configuring targets and routes that define where data points are routed. This platform service can only route metrics that are generated in [supported regions](/docs/metrics-router?topic=metrics-router-regions) by enabled services. Other regions, where IBM Cloud Metrics Routing is not available, continue to manage metrics by using [IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-getting-started).

For a list of supported IBM Cloud VPC services, see [IBM Cloud services that generate metrics that are managed through IBM Cloud Metrics Routing](/docs/metrics-router?topic=metrics-router-cloud-services-mr#vpc). To learn more about IBM Cloud Metrics Routing, see [Getting started with IBM Cloud Metrics Routing](/docs/metrics-router?topic=metrics-router-getting-started).

### 14 July 2023
{: #vpc-july1423}
{: release-note}

Very High Memory (vx2d) profile family now available in all regions (GA)
:   The vx2d profile is now available in the São Paulo (`br-sao`) region. Adding this region makes this profile family available in all regions. For more information about the Very High Memory profile family, see [Very High Memory](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 11 July 2023
{: #vpc-jul1123}
{: release-note}

Image lifecycle management for custom images (GA)
:   This image lifecycle management feature is now generally available. You can use the UI, CLI, and API to manage the lifecycle of your custom images with the following three statuses. You can move the image back and forth through all the statuses. You can also schedule status changes to manage the entire lifecycle of the image. For more information, see [Custom image lifecycle](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#custom-image-lifecycle) in [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images&interface=ui).
    * `available`: The image can be used to create an instance.
    * `deprecated`: The image can be used to create an instance. Using the `deprecated` status can discourage use of the image before the status changes to `obsolete`.
    * `obsolete`: The image can't be used to create an instance.

## June 2023
{: #vpc-june23}

### 27 June 2023
{: #vpc-jun2723}
{: release-note}

Snapshots for VPC Cross Regional Copy GA
:   Customers can now create a copy of a snapshot in a different region, and later use that copy to restore a volume in the new region. This feature can be beneficial in disaster recovery scenarios when the customer needs to start their virtual server instance and data volumes in a different region. Customers can also use the remote copy to create storage volumes in a new region and expand their VPC in new locations. For more information, see [Cross Regional Snapshot copies](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_crossregion_copy).

Backup for VPC Cross Regional Copy (GA)
:   Customers can now save a copy of their backup in a different region. Customers can copy the backup snapshot to another region manually or add the copy option to their backup policy plans. Customers can manage and use the cross-regional copy in the target region independently from the parent volume or the original backup. For more information, see [Cross Regional backup copies](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-crc).

### 22 June 2023
{: #vpc-jun2223}
{: release-note}

New Ed25519 SSH key type is available
:  The Ed25519 SSH key type is a new, supported SSH key type and can be used as an alternative to the RSA SSH key type. The Ed25519 SSH key can be used with Linux operating systems, but is not supported for Windows or VMware images. For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys) and [Managing SSH Keys](/docs/vpc?topic=vpc-managing-ssh-keys).

Madrid multi zone region (MZR)
:  A new MZR is available for VPC and Classic infrastructures. Classic Virtual Servers will not be available in the Madrid MZR. The Madrid (`eu-es`) region supports only dedicated host profiles with instance storage. For more information, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations), [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure&interface=ui), and [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui).

### 20 June 2023
{: #vpc-jun2023}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-11` updates
:  For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-11`, new certificates are available.
   - Attestation certificate
   - Encryption certificate
   - Intermediate certificate

   Support for customer-managed keys through integration with Hyper Protect Crypto Services
   - **Without** the feature, the data volume that you attach to your instance is encrypted automatically with a LUKS passphrase generated by using the **two** seeds from the `workload` - `volumes` and `env` - `volumes` sections of the contract. **Starting from the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-11`**, Hyper Protect Virtual Servers support integration with the key management service (KMS) Hyper Protect Crypto Services. You can enable the integration by providing KMS configurations in the contract. Your Hyper Protect Virtual Server instance calls Hyper Protect Crypto Services to generate a random value as the third seed and wrap it with your root key. The wrapped seed is stored in the metadata partition of your data volume. The LUKS passphrase is generated by using **three** seeds - the seed in the metadata partition (unwrapped first) and the two seeds from the contract. For more information about how the integration works and detailed instructions, see [Securing your data](/docs/vpc?topic=vpc-hyper-protect-virtual-server-mng-data).

   Deploying multiple containers
   - In the `workload` section of the contract, you can define the workload through Pod descriptors. Each pod can contain one or more container definitions. Previously, only one container described by docker compose was supported. For more information about using Pod descriptors, see the [`play` subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_play). Container images described by Pod descriptors can be [validated by RedHat Simple Signing](/docs/vpc?topic=vpc-about-contract_se#container-image-by-pod-descriptors).

   Changes to the attestation document
   - In the attestation document **se-checksums.txt**, `user-data.decrypted` is removed, and `Machine Type/Plant/Serial` (the information required to identify the host machine) is added. For more information, see [Attestation](/docs/vpc?topic=vpc-about-attestation).

Instance group integration with network load balancers (GA)
:  Network Load Balancer for VPC is now integrated with instance groups to improve pool member scaling. When you create or update an instance group for auto scaling, you can now specify the Network Load Balancer pool for the instance group to manage. For more information, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

Access control modes and granular authorization for File Storage for VPC file shares (beta release)
:   For users with accounts that have access to file shares, you can now specify an access control mode to either restrict mounting a file share to a specific virtual server instance in the VPC or allow VPC-wide file share mounting. File share mount targets that were created before `20-June-2023` have a default of VPC-wide file share mounting. File shares that are created after that date can specify security group access control mode to restrict access to a specific instance. For this option, file shares must be based on the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#dp2-profile). From the UI, CLI, or API, you set the access control mode when you create or update file shares, and can see the setting when you list file shares and in the file share details. When you create a mount target for a file share with security group access mode, you can specify a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to be created and attached to the mount target with a [security group](/docs/vpc?topic=vpc-using-security-groups). For the virtual network interface, you can specify an existing reserved IP, or specify a subnet and allow the system to assign an IP address. When the mount target is attached and the share is mounted, the virtual network interface performs security group policy check to ensure only authorized virtual server instances can communicate with the share. For more information, see [Mount targets for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets).

Data encryption in transit for file shares (beta release)
:  For users with accounts that have access to file shares, you can enable secure end-to-end encryption of your data when using security group based access control on file shares and mount targets with virtual network interfaces. The traffic between the authorized virtual server and the file share can optionally be IPsec encapsulated by the client. By using an Internet Security Protocol (IPsec), you can establish an encrypted mount connection between the virtual server instance and a file share with the `dp2` profile. The {{site.data.keyword.cloud}} file service provides a [mount helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) to automate the complex tasks of configuring and maintaining the connection. For more information, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).

Virtual network interface (beta release)
:  Virtual network interfaces are now available in a beta release for use with file share mount targets. For more information, see [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about).

### 16 June 2023
{: #vpc-jun1623}
{: release-note}

VPC routing table authorizations
:   You can use the new VPC routing table authorizations to allow users to administer VPC routing tables but not allow them to administer the broader VPC. Routing table operations were updated to check for these new authorizations, instead of the broader VPC authorizations. The VPC Administrator, Editor, Operator, and Viewer IAM access roles were updated so that users with those roles function as before. However, custom roles that require access to routing tables must be updated. For more information, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started&interface=ui).

### 15 June 2023
{: #vpc-jun1523}
{: release-note}

New regions available for Ultra High Memory profiles:
:   Ultra High Memory (ux2d) profiles are now available in the Washington DC (`us-east`), Toronto (`ca-tor`), and Frankfurt (`eu-de`) regions. For more information, see the [Ultra High Memory](/docs/vpc?topic=vpc-profiles&interface=ui#uhmemory) profiles documentation. For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 08 June 2023
{: #vpc-jun0823}
{: release-note}

New stock images for bare metal servers
:    The following stock images are now available for bare metal servers:
    * CentOS 7.x is now supported as an image when you provision a bare metal server.
    * CentOS Stream 9.x is now supported as an image when you provision a bare metal server.
      For more information, see [Bare metal server images](/docs/vpc?topic=vpc-bare-metal-image).

### 01 June 2023
{: #vpc-jun0123}
{: release-note}

UI Enhancement to the List view
:    You can now export table data with the current table column format. The supported export formats are CSV and Microsoft Excel.

## May 2023
{: #vpc-may23}

### 30 May 2023
{: #vpc-may3023}
{: release-note}

{{site.data.keyword.filestorage_vpc_short}} file share activity tracking event name changes (beta release)
:    For users with accounts that have access to file shares, when making API requests using a `version` query parameter of `2023-05-30` or later, the shares `targets` property was changed to `mount_targets`. This change affects file share activity tracking events. Events generated when [creating](/apidocs/vpc-beta/initial#create-share-mount-target), [listing](/apidocs/vpc-beta/initial#list-share-mount-targets), [retrieving](/apidocs/vpc-beta/initial#get-share-mount-target), [deleting](/apidocs/vpc-beta/initial#delete-share-mount-target), and [updating](/apidocs/vpc-beta/initial#update-share-mount-target) mount targets for a file share are now `is.share.mount-target.create`, `is.share.mount-target.list`,`is.share.mount-target.read`, `is.share.mount-target.delete`, and `is.share.mount-target.update`. Events for `is.share.target.create`, `is.share.target.list`, `is.share.target.read`, `is.share.target.delete`, and `is.share.target.update` are deprecated and will be removed in a future API release per the VPC beta API [versioning policy](/apidocs/vpc-beta#api-versioning-beta).

### 19 May 2023
{: #vpc-may1923}
{: release-note}

Removal of weak VPN for VPC ciphers
:   Effective 18 May 2023, the following VPN IKE and IPsec ciphers are now removed:
    * Authentication algorithms `md5` and `sha1`
    * Encryption algorithm `triple_des`
    * Diffie–Hellman groups `2` and `5`

   After this date, you cannot create an IKE/IPsec policy or connection that includes a weak cipher, but you can still upgrade weak cipher suites on an existing policy or connection. Starting 10 July 2023, any existing connections with customized IKE or IPsec policies that contain weak ciphers will be disabled, and any connections with auto IKE or IPsec policies that were created before September 20, 2022 will be forced to upgrade to the [enhanced auto-negotiation policy](/docs/vpc?topic=vpc-using-vpn#policy-negotiation).

### 11 May 2023
{: #vpc-may1123}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-10`
:  For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-10`, new certificates are available.
   - Attestation certificate
   - Encryption certificate
   - Intermediate certificate

### 09 May 2023
{: #vpc-may0923}
{: release-note}

New `-a100` GPU profile is available (select availability)
:   There is a new profile available for customers with special approval to preview this service that is for provisioning instances based on NVIDIA's A100 Ampere GPU attached to a single virtual server instance. The `gx2-80x1280x8a100` profile supports artificial intelligence and machine language frameworks and includes instance storage. Only Redhat and Ubuntu are supported for this profile. This profile is currently only available in the Washington DC (`us-east`) region. For more information, see [GPU profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). To request access to this profile, you must open a [support case](https://cloud.ibm.com/unifiedsupport/supportcenter).

## April 2023
{: #vpc-apr23}

## 27 April 2023
{: #vpc-apr2723}
{: release-note}

Documentation enhancement: Deploying an application for financial transactions with Confidential Computing on Hyper Protect Virtual Server for VPC
:   The newly added [**Deploying an application on Hyper Protect Virtual Server for VPC** tutorial](/docs/vpc?topic=vpc-financial-transaction-confidential-computing-on-hyper-protect-virtual-server-for-vpc) walks you through how to deploy the PayNow application with Confidential Computing on Hyper Protect Virtual Server for VPC.

UI update to enable deletion of nested resources
:    When you delete a resource, such as a VPC or a subnet, any attached resources must be deleted first before you can delete the VPC. The UI is updated to display a side panel listing all the resources still attached to the VPC that must be deleted. You can delete these resources from this side panel and then continue with deleting the VPC.

Export custom images (GA)
:   The ability to export custom images to {{site.data.keyword.cos_full_notm}}, a feature previously available in beta, is now generally available. For more information, see [Exporting a custom image to IBM Cloud Object Storage](/docs/vpc?topic=vpc-managing-custom-images&interface=ui#custom-image-export-to-cos).

### 06 April 2023
{: #vpc-apr0623}
{: release-note}

Image lifecycle management for custom images (beta release)
:   For customers with special access to this feature, you can use the UI, CLI, and API to manage the lifecycle of your custom images with the following three statuses. You can move the image back and forth through all the statuses. You can also schedule status changes to manage the entire lifecycle of the image. For more information, see [Custom image lifecycle](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#custom-image-lifecycle) in [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images&interface=ui).
    * `available`: The image can be used to create an instance.
    * `deprecated`: The image can be used to create an instance. Using the `deprecated` status can discourage use of the image before the status changes to `obsolete`.
    * `obsolete`: The image can't be used to create an instance.

### 04 April 2023
{: #vpc-apr0423}
{: release-note}

{{site.data.keyword.filestorage_vpc_short}} high-performance profile (beta release)
:    For customers with special access to this feature, you can now create file shares by using the **dp2** high-performance profile. This profile provides higher IOPS and greater capacity than earlier profiles. You can also modify existing file shares to use this profile. For more information, see the [file share profiles overview](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#file-storage-profile-overview).

## March 2023
{: #vpc-mar23}

### 31 March 2023
{: #vpc-mar3123}
{: release-note}

New network latency dashboard to view latency between zones in a multi-zone region (MZR)
:   Not only can you view inter-region latency metrics, but you can now view inter-AZ metrics between all regions and availability zones (AZs) to help you plan the optimal selection for your cloud deployment. To view performance metrics, see the [Network latency dashboards](/docs/vpc?topic=vpc-network-latency-dashboard).

## 30 March 2023
{: #vpc-mar3023}
{: release-note}

Documentation enhancement: Encrypting log messages for {{site.data.keyword.hpvs}} for VPC
:   The newly added [**Encrypting log messages** tutorial](/docs/vpc?topic=vpc-encrypt-log-messages-for-hyper-protect-virtual-servers-for-vpc) walks you through how to encrypt log messages that are generated by your container workload in your Hyper Protect Virtual Server for VPC instance. You can use the tutorial as reference to encrypt log messages if your workload produces sensitive information.

### 29 March 2023
{: #vpc-mar2923}
{: release-note}

VCPU manufacturer support for instances and dedicated hosts (select availability)
:   For accounts authorized to preview this functionality, you can now choose between profiles from different processor manufacturers when you provision an instance or dedicated host in the Toronto (`ca-tor`) region. For more information, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#balanced), [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui#balanced-dh-pr).

### 28 March 2023
{: #vpc-mar2823}
{: release-note}

Private DNS zones for network load balancers
:    You can now use IBM Cloud Application and Network Load Balancers for VPC to bind DNS zones from [IBM Cloud DNS Services](/docs/dns-svcs?topic=dns-svcs-getting-started), which you can use to move all DNS resolutions into private networks. Private DNS zones are resolvable only on IBM Cloud, and only from explicitly permitted networks in an account or with cross account access. For more information, see [Integrating a load balancer with IBM Cloud DNS Services](/docs/vpc?topic=vpc-lb-dns).

Enhancements to viewing profiles - number of supported network interfaces
:   In {{site.data.keyword.cloud_notm}} console, when you click **View all profiles** on the **Create virtual server instance** page, the **Bandwidth** column values include a tooltip. The tooltip now includes the number of network interfaces that can be attached for the profile. A similar feature is now available in the API when you list instance profiles. For more information, see [Network interface configuration for instance profiles](/docs/vpc?topic=vpc-api-change-log&interface=cli#28-march-2023-all-version-dates).

### 23 March 2023
{: #vpc-mar2323}
{: release-note}

New network latency dashboard
:   Provides visibility into network latency between all regions to help you plan the optimal selection for your cloud deployment and plan for scenarios, such as data residency and performance. This dashboard provides the average network round-trip latency (round-trip time or RTT) for all pairs of regions in {{site.data.keyword.cloud_notm}} over a 30-day period.

   You can view and monitor performance in the following {{site.data.keyword.cloud_notm}} regions: Dallas (`us-south`), Toronto (`ca-tor`), Washington DC (`us-east`), Frankfurt (`eu-de`), London (`eu-gb`), Osaka (`jp-osa`), Sydney (`au-syd`), Tokyo (`jp-tok`), and São Paulo (`br-sao`).

   To view performance metrics, see the [Network latency dashboard](/docs/vpc?topic=vpc-network-latency-dashboard).

### 21 March 2023
{: #vpc-mar2123}
{: release-note}

Instance provision by volume
:   You can now reuse an existing boot volume to provision a virtual server instance. The specified volume must be unattached, be in the same zone as the instance profile, and must have an operating system with the same architecture as the instance profile. By default, a boot volume that was created as part of provisioning a virtual server instance is deleted when the instance is deleted. You can control this behavior when you create or update an instance. For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui), and [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui).

Designating VPC route priority
:   When multiple VPC routes exist for a destination, you can now control the priority of these routes (from `0` to `4`). New and existing routes, which are created without a priority value, are automatically set to the default priority (`2`). Smaller values have higher priority. For more information, see [Determining route preference](/docs/vpc?topic=vpc-cr-determining-route-preference).

   The route priority is considered on identical destinations only.
   {: note}

Modifying the next hop for VPC routes
:   You can now [update](/apidocs/vpc/latest#update-vpc-routing-table-route) the next-hop of a VPC route. For more information, see [Creating a route](/docs/vpc?topic=vpc-create-vpc-route).

### 20 March 2023
{: #vpc-mar2023}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-9` updates
:  For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-9`, new certificates are available.
   - Attestation certificate
   - Encryption certificate
   - Intermediate certificate

   Two partitions in new data volume
   - For new {{site.data.keyword.hpvs}} instances, the data volume is partitioned into two parts. The first partition (100Mib) is reserved for internal metadata; the second partition remains as the data volume for workload. Only new volumes are partitioned, and you can't use the partitioned volume with an older version of the HPCR image. Provisioning with an existing encrypted volume also works. The difference is that the existing volume does not get partitioned, and you can also go back to an older image with this volume.

   Support for using a dynamic registry reference
   - There exist use cases in which the registry is **not known** when the workload section is pre-encrypted, for example, when the workload provider wants to allow the deployer to use a registry mirror or a private container registry. In such a case, it's possible to dynamically override the registry and the pull credentials - which is a coordinated effort between the workload provider and the deployer. For more information, see [Using a dynamic registry reference](/docs/vpc?topic=vpc-hyper-protect-virtual-server-use-dynamic-registry-reference).

### 16 March 2023
{: #vpc-mar1623}
{: release-note}

Client-to-site VPN server private certificate support
:    VPN servers now support the use of Secrets Manager private certificates. Private certificates are SSL/TLS certificates that you can sign, issue, and manage in the Secrets Manager service. For VPN server considerations, see [Using a private certificate](/docs/vpc?topic=vpc-client-to-site-authentication#using-private-certificate). For Secrets Manager information, see [Creating a private certificate](/docs/secrets-manager?topic=secrets-manager-private-certificates&interface=ui).

### 10 March 2023
{: #vpc-mar1023}
{: release-note}

Client-server timeout for application load balancers
:    You can now configure the client and server timeout parameters for IBM Cloud Application Load Balancer for VPC by using the UI, CLI, and API. The maximum timeout period for each listener is 2 hours, and the minimum is 50 seconds. If you need a timeout amount greater than 2 hours, open a [support case](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) with IBM Support. For more information about setting the client and server timeout period, refer to [Creating an application load balancer](/docs/vpc?topic=vpc-load-balancers&interface=ui). This functionality is only available for application load balancers.

## February 2023
{: #vpc-feb23}

### 14 February 2023
{: #vpc-feb1423}
{: release-note}

VPC metadata communication protocol and hop limit
:    You can now control the communication protocol and hop limit for IP response packets that are used by the [VPC Metadata service](/docs/vpc?topic=vpc-imd-about). When you provision or update an instance, use the new **Secure access** setting to specify either `http` (default) or `https` (secure access) communication. In addition, use the new **Hop limit** setting to specify a value between `1` (default) and `64`. Both of these settings apply only when the metadata service is enabled. For more information, see [Configure metadata settings by using the UI](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#metadata-config-ui).

Hyper Protect Secure Build
:    You can now use [Hyper Protect Secure Build](/docs/vpc?topic=vpc-about-hpsb) to securely build an Open Container Initiative (OCI) image in [Hyper Protect Virtual Servers for VPC](/docs/vpc?topic=vpc-about-se). You can push the image to DockerHub or IBM Cloud Container Registry (ICR). Later, you can pull the image from the registry to provision it in another {{site.data.keyword.hpvs}} for VPC instance. You can also pull SLES BaseContainerImages (BCI) from the SUSE registry, and use the images to provision {{site.data.keyword.hpvs}} for VPC instances.

### 09 February 2023
{: #vpc-feb0923}
{: release-note}

Export custom images (beta release)
:   For accounts that are authorized to preview this feature, you can now export custom images to {{site.data.keyword.cos_full_notm}}. For more information, see [Exporting a custom image to IBM Cloud Object Storage (beta release)](/docs/vpc?topic=vpc-managing-custom-images&interface=ui#custom-image-export-to-cos).

### 07 February 2023
{: #vpc-feb0723}
{: release-note}

Block Storage fast restore snapshots
:    You can now restore a fully provisioned volume with all its data from a snapshot by using a fast restore snapshot clone. You can use fast restore to restore a volume more quickly than restoring from a regular snapshot. To create the clone, you specify a zone or zones in the same region as the source snapshot. The clone is used to automatically restore a volume with all of its data in the zone where the clone exists. For more information, see [Restoring a volume by using fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-use-fast-restore).

Extra security for VPC snapshots (closed beta)
:    For customers with special access to this security beta feature, data isolation is provided to store snapshots created from your dedicated hosts. With data isolation extra security, your data is encrypted at rest with a unique key and access to your data is protected by a private firewall.

### 03 February 2023
{: #vpc-feb0323}
{: release-note}

Images for VPC UI Updates
:    Previously, the path to custom images was **VPC Infrastructure > Compute > Custom Images**. The new path is **VPC Infrastructure > Compute > Images**. The new page is **Images for VPC** and now has a tab for each type of image:
    - Custom images
    - Stock images
    - Catalog images

## January 2023
{: #vpc-jan23}

### 31 January 2023
{: #vpc-jan3123}
{: release-note}

Secure boot with Trusted Platform Module (TPM) (select availability)
:   Secure boot makes sure that your server starts with trusted software by verifying the signatures for all code in the boot process. Trusted Platform Module (TPM) provides hardware-based security functions. With supporting software, TPM helps maintain platform integrity and generates cryptographic keys. For more information, see [Secure boot with Trusted Platform Module (TPM)](/docs/vpc?topic=vpc-secure-boot-tpm).

### 30 January 2023
{: #vpc-jan3023}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-8` updates
:  For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-8`, new certificates are available.
   - Attestation certificate
   - Encryption certificate
   - Intermediate certificate

   Using {{site.data.keyword.hpvs}} for VPC in a private network
   - You can use your {{site.data.keyword.hpvs}} for VPC instance in **private-only** network configurations, in which the VPC doesn't have a public gateway, and the virtual server instance doesn't have a floating IP. You can connect to private endpoints of other services, including container registry and [IBM Log Analysis](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc). The prerequisite is that you need a DNS server that is attached to your virtual server instance. You don't need to do any extra configurations.

   Security enhancement to disk encryption verification
   - To address Denial-of-Service attacks, the requests to [verify disk encryption status](/docs/vpc?topic=vpc-hpvs-disks-encryption-validate) are throttled at three per five minutes.

### 27 January 2023
{: #vpc-jan2723}
{: release-note}

Context-based restrictions
:   Context-based restrictions are now available for {{site.data.keyword.vpc_short}} resources. With context-based restrictions, account owners and administrators can define and enforce network access policies. For more information, see [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr).

### 17 January 2023
{: #vpc-jan1723}
{: release-note}

Application Load Balancer and VPN for VPC
:    As a reminder, end of support for IBM Cloud Certificate Manager was 31 December 2022. Remaining instances of Certificate Manager were deleted. If any user-provided Ingress secrets are stored in Certificate Manager, they are no longer valid.

End of support (EOS) for deprecated VPN for VPC IKE and IPsec ciphers
:    On 20 September 2022, the following VPN IKE and IPsec ciphers were deprecated:
   - Authentication algorithms `md5` and `sha1`
   - Encryption algorithm `triple_des`
   - Diffie–Hellman groups `2` and `5`

   Effective today, these ciphers are no longer supported in the console and EOS for use with the CLI and API is forthcoming. If you didn't upgrade to more secure ciphers, do so now.


## December 2022
{: #vpc-dec22}

### 20 December 2022
{: #vpc-dec2022}
{: release-note}

Instance provision by volume (beta release)
:   By default, when a virtual server instance is deleted attached boot volumes are deleted.
You can disable this behavior, causing the boot volume to instead be detached when the virtual server instance is deleted. You can then attach the boot volume to a new virtual server instance.
For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui), [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli), and [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui).

Backup for VPC
:   The backup policy jobs feature, previously released as a Beta, is now generally available. You can now view all backup policy jobs or details of a single job from the UI, CLI, or API.  A backup policy job is triggered when a scheduled backup snapshot is being created or deleted. If the create or delete action is successful, the job contains information about the backup snapshot that was created or deleted. If the job ran unsuccessfully, the job contains the reason for the failure. For more information, see [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

### 17 December 2022
{: #vpc-dec1722}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-7` updates
:  For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-7`, new certificates are available.
   - Attestation certificate
   - Encryption certificate
   - Intermediate certificate

   Certificate revocation list
   - Starting from `ibm-hyper-protect-container-runtime-1-0-s390x-7-encrypt.crt` and `ibm-hyper-protect-container-runtime-1-0-s390x-7-attestation.crt`, the certificates contain **Certificate Revocation List (CRL) Distribution Points**. You can use the CRL to verify that your certificates are valid (not revoked). For more information, see the [Certificate revocation list](/docs/vpc?topic=vpc-cert_validate#certificate-revocation-list).

### 15 December 2022
{: #vpc-dec1522}
{: release-note}

Bare metal servers now support custom images
:   You can now create custom images to use on your bare metal servers. For more information, see [Custom image considerations](/docs/vpc?topic=vpc-bare-metal-image&interface=ui#bare-metal-custom-images-requirements) and [Bare metal server considerations](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#bare-metal-server-custom-images-considerations).

### 13 December 2022
{: #vpc-dec1322}
{: release-note}

Volume creation from a Block Storage snapshot
:   You can now use the UI and CLI, in addition to the VPC API, to create a stand-alone Block Storage volume from a snapshot. Stand-alone data volumes can be attached to a virtual server instance at any time. You can select a snapshot of a boot volume and use it to boot a new virtual server instance. For more information, see
[Restore a stand-alone data volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

Block Storage volume health states
:   You can now view the health state of a Block Storage volume from the UI, CLI, and API. Health indicated whether a volume is performing as expected or degraded. You can view health status and reasons from the list of volumes and volume details, and when you create and updating volumes. For more information, see [Block Storage volume health states](/docs/vpc?topic=vpc-block-storage-vpc-monitoring&interface=ui#block-storage-vpc-health-states).
