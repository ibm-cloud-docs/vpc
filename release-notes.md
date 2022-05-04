---

copyright:
  years: 2019, 2022

lastupdated: "2022-04-29"

keywords:

subcollection: vpc

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}
{:release-note: data-hd-content-type='release-note'}

# Release notes for {{site.data.keyword.vpc_short}}
{: #release-notes}

Use the release notes to learn the latest updates to {{site.data.keyword.vpc_full}} that are grouped by date.
{: shortdesc}

For information about changes to the {{site.data.keyword.vpc_short}} API , see [{{site.data.keyword.vpc_short}} API change log](/docs/vpc?topic=vpc-api-change-log)

## April 2022
{: #vpc-apr22}

### 29 April 2022
{: #vpc-april2922}
{: release-note}

Backup for VPC (LA)
:    Accounts authorized to preview this service can create backup policies and plans to automatically back up block storage volumes. Backup policies control which source volumes are selected for backup by matching user tags in the volume with tags defined in the backup policy. Policies can contain up to four backup plans, which specify how often backup snapshots are taken (daily, weekly, monthy, or using a `cron-spec`) and retained (by date or by count). You can also view backup jobs, which shows status of backup snapshots being created or deleted. This release also provides new functionality for restoring volumes from backup snapshots. For information about this service, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about) concepts.

### 28 April 2022
{: #vpc-april2822}
{: release-note}

UI Updates
:   The following UI updates were made:
    - The custom image details information was moved from the side panel to a separate custom image details page.
    - Previously, when you changed the region or zone while you provision a virtual server instance, you had to click either Save or Cancel to continue. This behavior was changed and you don't need to make that selection when you change the region or zone.
    - You can now delete the virtual server instance without manually unbinding the floating IP address. When you delete the virtual server instance, the floating IP address is now automatically unbound before the virtual server instance is deleted. This UI change now matches how the virtual server instance is deleted when you use the CLI or API.
    -
### 11 April 2022
{: #vpc-april1122}
{: release-note}

2022-03-29 release of the VPC API introduced incompatible changes
:    The `2022-03-29` release of the VPC API necessitated incompatible changes in support of the reserved IPs feature:
     - IP addresses are now modeled as objects (resources), rather than strings.
     - Security groups must now be associated with targets rather than network interfaces.

:    Even if you are not planning to make use of reserved IPs, to avoid regressions in client functionality, be sure to reference [2022-03-29 API migration](/docs/vpc?topic=vpc-2022-03-29-migration) and follow the Action needed recommendations before specifying version `2022-03-29` or later. For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#version-2022-03-29) and [Reserved IP known issues](/docs/vpc?topic=vpc-known-issues#ip-known-issues).

## March 2022
{: #vpc-mar22}

### 31 March 2022
{: #vpc-march3122}
{: release-note}

Add "Other" device types as Application Load Balancer (ALB) pool members
:   You can now add "Other" device types as ALB back-end pool members, such as servers contained within a Power Systems Virtual Server instance connected over IBM Cloud Direct Link (2.0). In the past, you were only able to select virtual server instances or Bare Metal servers within a VPC. When creating a new member, select the **Other** tab from the Attach server screen and enter the device IP address. For more information, see [Creating an IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancer&interface=ui).

### 30 March 2022
{: #vpc-march3022}
{: release-note}

New stock image for bare metal servers
:   RHEL 8.4 is now supported as an image when you provision a bare metal server.

### 29 March 2022
{: #vpc-march2922}
{: release-note}

UDP support for Network Load Balancers (NLB)
:   You can now set User Datagram Protocol (UDP) as the communication protocol for the {{site.data.keyword.nlb_full}} (NLB) listener and pool. UDP is a communications protocol for establishing low-latency and loss-tolerating connections between applications on the internet. It speeds up transmissions by enabling the transfer of data before an agreement is provided by the receiving party. For more information, see [Configuring the UDP protocol for network load balancers](/docs/vpc?topic=vpc-nlb-udp&interface=ui).

:   When configuring UDP and attaching a pool to your listener, you must configure the pool with the same protocol as the listener.
{: important}

Reserved IPs
:   You can now assign an IP address to your virtual server instance by specifying an already reserved IP for private IPs and bind them to network interfaces. For more information, see [Managing IP addresses](/docs/vpc?topic=vpc-managing-ip-addresses).


## March 25 2022
{: #vpc-march2522}
{: release-note}

LinuxONE (s390x processor architecture)
:   You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud in the Japan (Tokyo), Brazil (São Paulo), Canada (Toronto), and United Kingdom (London) regions. For more information about available LinuxONE (s390x processor architecture) profiles, see [Instance Profiles](/docs/vpc?topic=vpc-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).


## 24 March 2022
{: #vpc-march2422}
{: release-note}

New stock image for virtual servers
:   Windows&reg; 2022 is now supported as a stock image when you provision {{site.data.keyword.vpc_short}} virtual servers.

## 10 March 2022
{: #vpc-march1022}
{: release-note}

New stock image for virtual servers
:   Debian 11 is now supported as a stock image when you provision {{site.data.keyword.vpc_short}} virtual servers.

## February 2022
{: #vpc-feb22}

### 24 February 2022
{: #vpc-feb2422}
{: release-note}

VPC Instance Metadata Service (GA)
:   The VPC Instance Metadata service is now generally available in all regions. This free service provides a REST API you invoke within an instance to get information about that instance. Access to the API is unavailable from outside the instance. The service is disabled by default. Before you can access the metadata, the service lets you generate an instance identity access token for accessing the metadata service. You can optionally get an IAM token from this token to access all IAM-enabled services. For more information, see [About VPC Instance Metadata](/docs/vpc?topic=vpc-imd-about).

:   Virtual server instances created on LinuxONE (s390x processor architecture) are not enabled for the VPC Instance Metadata service. The metadata service is currently supported only on x86 systems.
{: note}

UI Update
:   There is a new Location cascading selector. For example, when you provision a virtual server, the Location section now has 3 new cascading selector boxes. You can now choose the geographic location, the metro (region) location, and the zone. The options available in these boxes are updated based on your selections. If you select “Americas” for the Geography, then the Metro box displays only the Metros available for the Americas.

### 22 February 2022
{: #vpc-feb2222}
{: release-note}

Host failure recovery policies
:    Host failure recovery policies for VPC is generally available (GA). For more information about host failure recovery policies, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=ui).

### 15 February 2022
{: #vpc-feb1522}
{: release-note}

LinuxONE (s390x processor architecture)
:   You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud in the Japan (Tokyo), United Kingdom (London), and São Paulo regions. For more information about available LinuxONE (s390x processor architecture) profiles, see [Instance Profiles](/docs/vpc?topic=vpc-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

Resizable boot volumes
:    You can now increase the capacity of a boot volume, up to 250 gigabytes (GB), when creating an instance from an image or instance template. You can also directly update an existing boot volume to increase its capacity. For more information, see [Increasing boot volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes).

File Storage for VPC
:    The file service is now integrated with the Security and Compliance Center to help you manage security and compliance for your organization. For information about this feature, see [Managing security and compliance](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-vpc-manage-security).

### 14 February 2022
{: #vpc-feb1422}
{: release-note}

Snapshots for VPC
:    The snapshots service is now integrated with the Security and Compliance Center to help you manage security and compliance for your organization. For information about this feature, see [Managing security and compliance](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-manage-security).

### 11 February 2022
{: #vpc-feb1122}
{: release-note}

**Maximum bandwidth for each vNIC is increased**
:   The maximum bandwidth for a vNIC is now 25 Gbps instead of the previous 16 Gbps to 25 Gbps. For more information, see [Optimizing network bandwidth allocation for profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles#network-perf-notes-for-profiles).   

### 10 February 2022
{: #vpc-feb1022}
{: release-note}

Block storage attachments
:    Limits on the number of block storage data volumes you can attach to an instance were amended. Previously, instances created from smaller vCPU profiles could only attach up to four data volumes. These limits are removed. For all instances, you can attach up to 12 block storage data volumes.

### 08 February 2022
{: #vpc-feb0822}
{: release-note}

Port ranges for public network load balancers
:    When [creating a public network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer&interface=api), you can now specify a range of listener ports.

Bare Metal server support for application load balancers
:    Bare Metal server members are now supported on application load balancers. See [Load balancers for VPC overview](/docs/vpc?topic=vpc-nlb-vs-elb&interface=ui) for more information.

## January 2022
{: #vpc-jan22}

### 31 January 2022
{: #vpc-jan3122}
{: release-note}

Bare Metal Servers for VPC
:    Bare Metal Servers for VPC is generally available (GA). For more information about bare metal servers, see [About Bare Metal Servers for VPC](/docs/vpc?topic=vpc-about-bare-metal-servers).

### 25 January 2022
{: #vpc-jan2522}
{: release-note}

Security group support for Virtual Private Endpoint (VPE) gateways
:   You can now attach security groups to your endpoint gateways and manage them for your application needs. Attach security groups when creating an endpoint gateway, or modify security groups bound to an endpoint gateway after provisioning. For more information, see [Configuring ACLs and security groups for use with endpoint gateways](/docs/vpc?topic=vpc-configure-acls-sgs-endpoint-gateways).

Update to Snapshots for VPC
:   When you view details of a snapshot from the [UI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-snapshot-ui), [CLI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-details-cli), or [API](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=api#snapshots-vpc-view-api), a new field shows when the snapshot was captured from the volume. This feature applies to snapshots created after January 2022.

### 21 January 2022
{: #vpc-jan2122}
{: release-note}

**New stock images for virtual servers**
:   The following stock images are now available for virtual servers:
    - Redhat 8.4 for SAP
    - SLES 12 SP5 for SAP
    - SLES 15 SP3 for SAP

### 14 January 2022
{: #vpc-jan1422}
{: release-note}

Application Load Balancer (ALB) for VPC
:   Application load balancers now support HTTP/HTTPS compression, which allows you to compress data being transmitted to your users. For more information, see [Compression (HTTP/HTTPS only)](/docs/vpc?topic=vpc-advanced-traffic-management#compression).

High Availability (HA) Virtual Network Function (VNF) support
:   Support for a highly available, highly resilient virtual network functions can be achieved through the use of the "routing mode" feature of the IBM Cloud Network Load Balancer (NLB) for VPC. For more information, see [About virtual network functions over VPC](/docs/vpc?topic=vpc-about-vnf) and [About HA VNF deployments](/docs/vpc?topic=vpc-about-vnf-ha).

Updates to Getting started with IBM Cloud VPC button
:   The "Getting started with IBM Cloud VPC" button now includes access to tours that are specific to what you are doing on the IBM console. If a tour is not available, the button takes you to the VPC List view.

### 06 January 2022
{: #vpc-jan0622}
{: release-note}

UI update when creating a virtual server
:   When you create a virtual server, the UI is updated to include a link in the Operating system section that opens a panel containing information about the image lifecycle.

## December 2021
{: #vpc-dec21}

### 16 December 2021
{: #vpc-dec1621}
{: release-note}

File Storage for VPC (LA)
:   {{site.data.keyword.filestorage_vpc_full}} is now available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, London, Sydney, and Tokyo regions.

:   For more information about this service, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about).

### 09 December 2021
{: #vpc-dec0921}
{: release-note}

Security updates
:   The following stock images were refreshed with the most recent fixes and security updates.
    - Debian version 10
    - CentOS version 7
    - RHEL versions 7, 8.2, and 8.4
    - Ubuntu version 20.04

Windows stock image updates
:   For all Windows stock images, TLS 1.2 was enabled and all earlier versions were disabled.

:   The following cloudbase-init configuration files were updated to add the value "ntp_use_dhcp_config=true":
    - C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init.conf
    - C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init-unattend.conf

New stock image for virtual servers
:   Rocky Linux version 8.5 is now supported as a stock image when you provision {{site.data.keyword.vpc_short}} virtual servers.

## November 2021
{: #vpc-nov21}

### 30 November 2021
{: #vpc-nov3021}
{: release-note}

VPN client-to-site servers update (open Beta)
:   You can now update a VPN server after the VPN is provisioned. For example, you can upgrade a stand-alone VPN server (one subnet) to a High Availability (HA) VPN server (two subnets in different zones). You can also detach a subnet to downgrade an HA VPN server to a stand-alone deployment, or change an attached subnet after your VPN server is provisioned. For more information, see [Upgrading to an HA VPN server](/docs/vpc?topic=vpc-vpn-client-to-site-change-server-types).

### 16 November 2021
{: #vpc-nov1621}
{: release-note}

New Very High Memory and Ultra High Memory instance profile family for dedicated host (LA)
:   Very High Memory with instance storage and Ultra High Memory with instance storage profiles are now available for dedicated host.

:   Very High Memory profiles offer a core to RAM ratio of 1 vCPU to 14 GiB of RAM. This family is hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and is best for OLAP workloads and SAP-related services, such as SAP NetWeaver. Very High Memory profiles are available in the US South (Dallas), US East (Washington DC), Canada (Toronto), United Kingdom (London), EU Germany (Frankfurt), Japan (Tokyo), Japan (Osaka), and Australia (Sydney) regions. For more information, see [Very High Memory with instance storage profiles](/docs/vpc?topic=vpc-dh-profiles#vhm-is-dh-pr).

:   Ultra High Memory profiles offer a core to RAM ratio of 1 vCPU to 28 GiB of RAM. This family is hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and are optimized for running memory intensive applications and in-memory database such as SAP HANA, Memcached, or Redis. Ultra High Memory profiles are available in the US South (Dallas) and EU Germany (Frankfurt) regions. For more information, see [Ultra HIgh Memory with instance storage profiles](/docs/vpc?topic=vpc-dh-profiles#uhm-is-dh-pr).

### 12 November 2021
{: #vpc-nov1221}
{: release-note}

Fedora Core OS
:   Fedora Core OS is now available in all regions as a stock image. The user login ‘root’ is disabled by default in Fedora Core OS. The user login ‘core’ can be used to log in to Fedora Core OS instances. For more information, see [Virtual server images](/docs/vpc?topic=vpc-about-images#x86-supported-os) and [User data examples for Fedora Core OS](/docs/vpc?topic=vpc-user-data#user-data-examples-for-fed-core).

### 02 November 2021
{: #vpc-nov0221}
{: release-note}

Instance Metadata Service for VPC (LA)
:   The metadata service is now available in all regions to customer accounts authorized to access this service. This service is enabled by default when you create new instances using the UI, CLI, or API. You can also disable the service from these interfaces. For more information, see [Enable or disable the metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-metadata-service-enable).

:   The metadata service also provides a new API call to generate an IAM token from the instance identity access token using trusted profile information. For more information, see [Generate an IAM token from an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange).

## October 2021
{: #vpc-oct21}

### 21 October 2021
{: #vpc-oct2121}
{: release-note}

**UI Updates**
:    When you create a virtual server instance by using the UI, the image selection on the UI was updated. The image selection is changed from a tile selection to a drop-down menu. For both custom and catalog images, there is now a side panel that enables you to filter, sort, and search for images.

### 15 October 2021
{: #vpc-oct1521}
{: release-note}

New GPU Instance Profile family
:   GPU profiles are available for all images. The profiles include attached NVIDIA 16 GB PCIe v100 GPUs. Drivers must be installed separately. You can use these instances to accelerate certain processing functions for workloads such as inferencing and machine learning. For more information about this service, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

### 05 October 2021
{: #vpc-oct0521}
{: release-note}

New regions for Very High Memory instance profile family (LA)
:   Very High Memory profiles now are available in the United Kingdom (London), Japan (Osaka), and Australia (Sydney) regions. For more information, see [Very High Memory profiles](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory).

### 01 October 2021
{: #vpc-oct0121}
{: release-note}

File Storage for VPC (LA)
:   {{site.data.keyword.filestorage_vpc_full}} is now available in the United Kingdom (London) region. For more information about this service, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about).

## September 2021
{: #vpc-sep21}

### 23 September 2021
{: #vpc-sep2321}
{: release-note}

VNF routing mode for network load balancers
:   With {{site.data.keyword.vpc_short}}, you can provision Virtual Network Function (VNF) devices to gain better and more affordable scalability than you would by purchasing physical network devices. {{site.data.keyword.vpc_short}} network load balancers with routing mode allow you to send traffic only to VNF devices as back-end targets. In addition, their health checks ensure that workloads only travel through healthy VNF devices. For more information about this service, see [Creating a network load balancer with routing mode for VPC](/docs/vpc?topic=vpc-nlb-vnf).

### 14 September 2021
{: #vpc-sep1421}
{: release-note}

Terraform available for Placement Groups
:   You can now create and manage placement groups in your {{site.data.keyword.vpc_short}} by using Terraform. See [Managing placement groups](/docs/vpc?topic=vpc-managing-placement-group&interface=terraform).  

### 07 September 2021
{: #vpc-sep0721}
{: release-note}

New Very High Memory instance profile family (LA)
:   Very High Memory profiles are available in the US South (Dallas), US East (Washington DC), Canada (Toronto), EU Germany (Frankfurt), and Japan (Tokyo) regions. Very High Memory profiles offer a core to RAM ratio of 1 vCPU to 14 GiB of RAM. This family is optimized for running high-compute-intensity in-memory workloads like SAP BW/4 HANA. For more information, see [Very High Memory profiles](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory).  

Instance Bandwidth (LA)
:   Instance bandwidth is now available in the US South (Dallas), US East (Washington DC), Canada (Toronto), EU Germany (Frankfurt),  Japan (Osaka), Brazil (Sao Paulo) regions. When you provision a virtual server instance, you can now allocate bandwidth between attached volumes and networking by using the API and CLI. You can adjust bandwidth after provisioning a virtual server instance by using the UI, API, and CLI. The maximum bandwidth capacity is determined by the instance profile that you select during instance provisioning. For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

### 01 September 2021
{: #vpc-sep0121}
{: release-note}

New GPU Instance Profile family (Beta)
:   GPU profiles are available in a limited, allow-listed beta for all images. The profiles include attached NVIDIA 16 GB PCIe v100 GPUs. Drivers must be installed separately. You can use these instances to accelerate certain processing functions for workloads such as inferencing and machine learning. For more information about this service, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

## August 2021
{: #vpc-aug21}

### 26 August 2021
{: #vpc-aug2621}
{: release-note}

RHEL on SAP image support update
:   When you provision a virtual server instance and select {{site.data.keyword.redhat_full}} Enterprise Linux, be aware that version locking must be done manually. When you log into your operating system, a message is displayed with the command needed to manually lock the operating system version. Performing a “yum update” without the version being locked will result in the operating system being upgraded to the latest RHEL release, which is currently 8.4.

Block storage for VPC
:   For Block Storage for VPC volumes attached to a virtual server instance, you can increase or decrease IOPS for a volume by specifying a different IOPS tier profile or different IOPS value withing a custom IOPS band. For more information, see [Adjusting IOPS for block storage volumes](/docs/vpc?topic=vpc-adjusting-volume-iops).

Client-to-site VPN servers (Beta)
:   Until now, the {{site.data.keyword.cloud_notm}} VPN for VPC service supported only site-to-site connectivity, which connects your on-premises network to the {{site.data.keyword.cloud_notm}} VPC network. This Beta adds client-to-site connectivity, which allows users to connect to their {{site.data.keyword.cloud_notm}} VPC infrastructure through a secure/encrypted connection over the internet. This service is especially useful for individuals working at home, traveling, or at locations where site-to-site VPN might not be available. For more information, see [About client-to-site VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-overview).

### 24 August 2021
{: #vpc-aug2421}
{: release-note}

HTTPS redirect listeners
:   The [HTTPS redirect feature](/docs/vpc?topic=vpc-load-balancers#https-redirect-listener) is now available on Application Load Balancer for VPC to redirect traffic from an HTTP load balancer listener to an HTTPS listener. You can configure an HTTPS redirect on either [load balancer listeners](/docs/vpc?topic=vpc-load-balancers), [load balancer policies](/docs/vpc?topic=vpc-layer-7-load-balancing), or both.

### 19 August 2021
{: #vpc-aug1921}
{: release-note}

Larger size boot volumes for custom images
:   You can import custom images with a boot disk size from 10 GB to 250 GB, which will become the image's minimum provisioned size after importing. When you specify the image as part of [creating an instance](/docs/vpc?topic=vpc-creating-block-storage), the boot volume capacity is set to this size. For more information, see [Planning custom images](/docs/vpc?topic=vpc-planning-custom-images).

### 18 August 2021
{: #vpc-aug1821}
{: release-note}

File Storage for VPC
:   Custom IOPS profiles are available with capacity up to 16,000 GB. For information, see [File storage profiles](/docs/vpc?topic=vpc-file-storage-profiles).

:   You can also increase the file share size from its original capacity in GB increments up to 32,000 GB capacity, depending on your share profile. For more information, see [Expanding file share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity).

:   {{site.data.keyword.filestorage_vpc_full}} is available to customers with special approval to preview this service in the Washington, Dallas, and Frankfurt regions.

### 17 August 2021
{: #vpc-aug1721}
{: release-note}

Instance Identifier Update
:   The instance identifier (ID) now includes the SMBIOS system-uuid as a portion of the ID. The ID, including the SMBIOS system-uuid portion, is static and persists for the lifecycle of the virtual server instance until that virtual server instance is deleted. For more information, including how to retrieve this information from within your virtual server, see [Retrieving the virtual server instance identifier](/docs/vpc?topic=vpc-managing-virtual-server-instances#retrieve-VSI-instance-identifer) section in Managing virtual server instances.

Placement groups (GA)
:   Placement groups for {{site.data.keyword.vpc_full}} are used to create placement group strategies for managing high availability workloads. A placement group contains virtual server instances that share a common placement strategy. Placement strategies influence the physical placement of select VPC resources to meet certain workload demands. For more information about placement groups, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc).

### 11 August 2021
{: #vpc-aug1121}
{: release-note}

Auto scale supports data volumes and private catalog
:   Instance groups now support using an instance template that includes one ore more data volumes. In addition, instance groups are now supported in private catalog. For more information about instance groups, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

### 9 August 2021
{: #vpc-aug0921}
{: release-note}

LinuxONE (s390x processor architecture)
:   You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud™ in the Tokyo region. For more information about available LinuxONE (s390x processor architecture) profiles, see [Instance Profiles](/docs/vpc?topic=vpc-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).
For more information about some of the limitations of LinuxONE (s390x processor architecture), see [Service limitations](/docs/vpc?topic=vpc-limitations).

## July 2021
{: #vpc-jul21}

### 30 July 2021
{: #vpc-jul3021}
{: release-note}

Instance Metadata for VPC (closed Beta)
:   If your account is granted special approval to preview this feature, you can access metadata about your VPC compute resources. The metadata service is a REST API that you invoke using a well-known URI to retrieve instance-specific information from the metadata server. For more information, see [About Instance Metadata for VPC (Beta)](/docs/vpc?topic=vpc-imd-about).

### 26 July 2021
{: #vpc-jul2621}
{: release-note}

New São Paulo region
:   The São Paulo region endpoint is now in service at `https://br-sao.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

### 22 July 2021
{: #vpc-jul2221}
{: release-note}

Block Storage for VPC
:   IOPS tier and custom profiles are available with volume capacity up to 16,000 GB. For information, see [Block storage profiles](/docs/vpc?topic=vpc-block-storage-profiles).

:   For secondary volumes attached to a virtual server instance, you can increase capacity in GB increments up to 16,000 GB, depending on the volume's profile. The volume capacity is immediately increased. For more information, see [Expanding block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

### 21 July 2021
{: #vpc-jul2121}
{: release-note}

Bare Metal Servers for VPC (closed Beta)
:   If your account has been granted special approval to preview this feature, you can now create bare metal servers to host VMware clusters in VPC. You can set up VMware management applications and create VMware virtual machines (VM) on the bare metal servers. As bare metal servers are integrated with the VPC platform, you can take advantage of the network, storage, and security capabilities provided by the VPC. For more information, see [About Bare Metal Servers for VPC (beta)](/docs/vpc?topic=vpc-about-bare-metal-servers).

:   The bare metal console feature is temporarily unavailable. An "unauthorized" error will be returned if you try to connect to the console.

### 13 July 2021
{: #vpc-jul1321}
{: release-note}

Instance Identifier Update
:   The instance identifier (ID) now includes the SMBIOS system-uuid as a portion of the ID. The ID, including the SMBIOS system-uuid portion, is static and persists for the lifecycle of the virtual server instance until that virtual server instance is deleted. For more information, including how to retrieve this information from within your virtual server, see [Retrieving the virtual server instance identifier](/docs/vpc?topic=vpc-managing-virtual-server-instances#retrieve-VSI-instance-identifer) section in Managing virtual server instances.

### 6 July 2021
{: #vpc-jul0621}
{: release-note}

File storage for VPC
:   The file storage general purpose 3 IOPS/GB profile is expanded to allow you to create file shares up to 32 TB with potential max IOPS performance of 96,000 IOPS. For more information, see [File storage profiles](/docs/vpc?topic=vpc-file-storage-profiles). File Storage for VPC is available to customers with special approval to preview this service in the Washington, Dallas, and Frankfurt regions.

## June 2021
{: #vpc-jun21}

### 30 June 2021
{: #vpc-jun3021}
{: release-note}

Placement groups
:   Placement groups for {{site.data.keyword.vpc_full}} are used to create placement group strategies for managing high availability workloads. A placement group contains virtual server instances that share a common placement strategy. Placement strategies influence the physical placement of select VPC resources to meet certain workload demands. For more information about placement groups, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups).

### 17 June 2021
{: #vpc-jun1721}
{: release-note}

UI enhancement
:   When provisioning a VPC, you can now create multiple subnets from the default address prefix. When you create a VPC, one subnet is created in each zone of the VPC region. You can add up to a maximum of 9 subnets.”

Application Load Balancer (ALB) support for cookie-based session persistence
:   ALBs now supports HTTP-cookie persistence and application-cookie persistence. For more information, see [Advanced traffic management](/docs/vpc?topic=vpc-advanced-traffic-management#session-persistence).

### 03 June 2021
{: #vpc-jun0321}
{: release-note}

UI enhancement
:   When deleting a virtual server, you must now confirm that you want to delete an instance by typing *delete*.

## May 2021
{: #vpc-may21}

### 25 May 2021
{: #vpc-may2521}
{: release-note}

Snapshots for VPC
:   Snapshots for VPC is a regional offering that lets you create a point-in-time copy of your block storage boot or data volume. Select a snapshot during instance provisioning and restore a new, fully-provisioned boot volume to start the instance. You can also create and attach a data volume from a snapshot within a running virtual server instance. Learn more [about creating and using snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) and explore the new [snapshots API methods](/apidocs/vpc#delete-snapshots).

## 21 May 2021
{: #vpc-may2121}
{: release-note}

Image from volume
:   You can create a custom image from a boot volume on a virtual server instance. The image is a full copy of the source volume, including the operating system and any user data. You can create new virtual server instances from the image created from the volume. For more information, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc).

### 13 May 2021
{: #vpc-may1321}
{: release-note}

Scheduled Scaling for VPC (GA)
:   Scheduled Scaling for VPC lets you set scheduled actions to automatically scale instance group capacity up or down based on daily, intermittent, or seasonal demand. You can set multiple, recurring scheduled action frequencies that can scale monthly, weekly, daily, hourly, or even every set number of minutes. For more information, see [Scheduled scaling](/docs/vpc?topic=vpc-scheduled-scaling-vpc).

### 06 May 2021
{: #vpc-may0621}
{: release-note}

New Ultra High Memory instance profile family (LA)
:   Ultra High Memory profiles are hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and are optimized for running memory intensive applications and in-memory database such as SAP HANA, Memcached, or Redis. This profile family offers our highest vCPU to memory ratio with 28 GiB of memory for every 1 vCPU of compute and up to 5.7 TiB of available RAM. For more information, see [Instance Profiles](/docs/vpc?topic=vpc-profiles).

:   The Ultra High Memory family of profiles is currently available in the Dallas and Frankfurt multizone regions(MZRs). For more information, see the Multizone regions section in [Locations for resource deployment](/docs/overview?topic=overview-locations#mzr-table). Contact your IBM Sales representative if you need Ultra High Memory profiles in a MZR other than Dallas or Frankfurt.

## April 2021
{: #vpc-apr21}

### 30 April 2021
{: #vpc-apr3021}
{: release-note}

File Storage for VPC
:   File Storage for VPC is available to customers with special approval to preview this service in the Washington, Dallas, and Frankfurt regions. With this feature, you can create NFS-based file shares in a single zone in a region. You can share file storage over multiple virtual service instances within the same zone across multiple VPCs. For more information about this service, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about). Contact your IBM Sales representative if you are interested in getting access.

### 07 April 2021
{: #vpc-apr0721}
{: release-note}

File Storage for VPC (Beta)
:   NFS-based file shares for a zone within a region are available in a limited, allow-listed beta. You can share file storage over multiple virtual service instances within the same zone across multiple VPCs. For more information about this service, see [About File Storage for VPC (beta)](/docs/vpc?topic=vpc-file-storage-vpc-about).

UI enhancement
:   When provisioning a new VPN gateway in a default VPC, the UI now populates the subnet table with the default subnet information.

### 06 April 2021
{: #vpc-apr0621}
{: release-note}

New Toronto region
:   The Toronto region endpoint is now in service at `https://ca-tor.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

### 01 April 2021
{: #vpc-apr0121}
{: release-note}

Instance storage
:   You can now provision a virtual server instance with instance storage, a set of one or more solid state drives directly attached to your instance. An instance storage disk provides fast, affordable, temporary storage to improve the performance of cloud-native workloads with scratch space, caching buffers, or a place for replicated data. To provision a virtual server instance with instance storage, you must select an instance storage [profile](/docs/vpc?topic=vpc-profiles). For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

Dedicated hosts support instance storage
:   You can now provision a dedicated host with an instance storage profile, providing the capability to host virtual server instances that include instance storage. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles).

Instance resize (GA)
:   The ability to resize a virtual server instance by selecting a different profile to assign to the instance is now generally available. Profiles are a combination of instance attributes, such as the number of vCPUs, amount of RAM, network bandwidth, and more that define the size and capabilities of the virtual server instance. For more information, see [Resizing an instance](/docs/vpc?topic=vpc-resizing-an-instance).

## March 2021
{: #vpc-mar21}

### 31 March 2021
{: #vpc-mar3121}
{: release-note}

Virtual server instance console
:   The virtual server instance console feature is now generally available in all regions. For more information, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console).

### 25 March 2021
{: #vpc-mar2521}
{: release-note}

New parameter-based rule types for application load balancers
:   When creating policy rules for {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB), the `field` property may now be set to `query` or `body` to perform additional forms of layer 7 load balancing.
   * `query` - Write layer 7 rules that use the query string to route traffic to a specific target.
   * `body` - If the body of the `POST` request uses form encoding (UTF-8), then you can create layer 7 rules to route traffic based on the parameter name and value in the body. For more information, refer to [Layer 7 load balancing](/docs/vpc?topic=vpc-layer-7-load-balancing).

TCP keep alive support for application load balancers
:   {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) now supports `TCP keep alive`. With this setting, the ALB sends TCP-keep-alive packets to both client and back-end servers every five seconds. For more information, refer to [Advanced traffic management](/docs/vpc?topic=vpc-advanced-traffic-management#tcp-keep-alive).

New monitoring metrics
:   {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) now has additional monitoring metrics to help track the traffic and usage patterns for your load balancers. These new metrics can provide insight about peak traffic hours, usage dropoffs, and overall usage patterns. The new metrics are:

   * Total number of HTTP/HTTPS requests received by the back-end
   * Average response time for HTTP/HTTPS requests
   * Count of responses with the HTTP 2xx back-end response code
   * Count of responses with the HTTP 3xx back-end response code
   * Count of responses with the HTTP 4xx back-end response code
   * Count of responses with the HTTP 5xx back-end response code

   For more information, refer to [Monitoring Application Load Balancer for VPC metrics](/docs/vpc?topic=vpc-monitoring-metrics-alb).

Private network load balancers
:   {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) now supports the creation of private network load balancers. A private NLB is a load balancer only accessible from within the VPC network and/or where the client has reachability to the VPC network (for example, through Direct Link or a transit gateway). To create private load balancers, you must have a dedicated subnet with no custom routes configured for the subnet. For information on creating a private NLB, refer to [Creating a network load balancer for VPC](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer).

### 19 March 2021
{: #vpc-mar1921}
{: release-note}

Virtual server instance console
:   The virtual server instance console feature is now generally available in the following regions: Dallas, Frankfurt, London, Osaka, and Washington DC. For more information, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console).

Bring you own license (GA)
:   You can bring your own license (BYOL) when you import a RedHat Enterprise Linux (RHEL) or Windows custom image to IBM Cloud VPC. Because these images are registered and licensed by you, you maintain control over your licenses and with no additional cost. Acquisition and activation of the license is between you and and the OS vendor. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about).

Dedicated hosts (GA)
:   Dedicated hosts are available in all regions. Use a dedicated host to carve out a single-tenant compute node for {{site.data.keyword.vpc_short}}. For more information, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)

### 12 March 2021
{: #vpc-mar1221}
{: release-note}

New profiles
:   The Balanced, Compute, and Memory profile families now include three new profiles with 64, 96, and 128 vCPUs. Profiles with 64 or more vCPUs are deployed exclusively on 2nd generation Intel Xeon Platinum 8260 (Cascade Lake) running at a base speed of 2.4 GHz and an all-core turbo frequency of 3.1 GHz.

VPN for VPC
:   For high-security standards, VPN now supports SHA-512 authentication and Diffie-Hellman Group 19.

:   Gateway private IP addresses are now visible for gateway members.

### 05 March 2021
{: #vpc-mar0521}
{: release-note}

Instance resize (Beta)
:   Beta users can now resize a virtual server instance by selecting a different profile to assign to the instance. Profiles are a combination of instance attributes, such as the number of vCPUs, amount of RAM, network bandwidth, and more that define the size and capablilities of the virtual server instance. For more information, see [Resizing an instance (Beta)](/docs/vpc?topic=vpc-resizing-an-instance).

Snapshots for VPC (Beta)
:   Select Beta users can now create snapshots of their block storage boot and data volumes attached to a running virtual server instance. These snapshots can be used to create new volumes, which function like the original volume. For more information, see [About Snapshots for VPC (Beta)](/docs/vpc?topic=vpc-snapshots-vpc-about).

UI enhancement
:   When creating or editing a VPN policy (IKE or IPsec), a new authentication option is available, SHA512. Additionally, a new Diffie-Hellman key exchange group is supported: group 19.

## February 2021
{: #vpc-feb21}

### 25 February 2021
{: #vpc-feb2521}
{: release-note}

Dedicated hosts
:   You can now use dedicated hosts to carve out a single-tenant compute node for {{site.data.keyword.vpc_short}} in the following regions: Dallas, London, Tokyo, and Osaka. For more information, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances).

Application load balancer security group integration
:   For enhanced security, application load balancers can now be associated with security groups. You can specify one or more security groups when you create the application load balancer, and associate security groups with your existing application load balancers. For more information, see [Integrating an IBM Cloud Application Load Balancer for VPC with security groups](/docs/vpc?topic=vpc-alb-integration-with-security-groups).

UI enhancement
:   On the New virtual server for VPC page, selected SSH keys persist if you leave the page and then return to it during a single logged in session. If you log out or switch your account, the selected SSH keys change.

VPC address prefixes are no longer restricted to RFC-1918 addresses
:   You must now configure VPCs that use both non-RFC-1918 addresses and have public connectivity (floating IPs or public gateways) using a custom route that contains the new `Delegate-VPC` action. You must specify this action for destination CIDRs that are non-RFC-1918 compliant and also outside of the VPC, such as for destinations that are reachable through Direct Link (2.0), Transit Gateway, or VPC classic access. For more information about when to use the `Delegate-VPC` action, see [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana).

### 18 February 2021
{: #vpc-feb1821}
{: release-note}

UI enhancement
:   You can now provision a VPN gateway or load balancer without first creating a VPC. If you don't have an existing VPC you can provision the VPN gateway or load balancer in the default VPC. The new VPN gateway or load balancer is created along with a default VPC that includes a default security group, access control list, and subnet.

### 07 February 2021
{: #vpc-feb0721}
{: release-note}

Virtual server instance console (Beta)
:   You can now access your virtual server by connecting to a VNC or serial console. This feature provides a quick-and-easy way for you to interact with the virtual server without using a Secure Shell. This is a Beta feature that requires special approval. Contact your IBM Sales representative if you are interested in getting access. For more information, see [Accessing virtual server instances by using VNC or serial consoles (Beta)](/docs/vpc?topic=vpc-vsi_is_connecting_console)

## January 2021
{: #vpc-jan21}

### 26 January 2021
{: #vpc-jan2621}
{: release-note}

New region
:   The Osaka region endpoint is now in service at `https://jp-osa.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region). See also [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API.

### 11 January 2021
{: #vpc-jan1121}
{: release-note}

**Customer-managed encryption (GA)**
:   Customer-managed encryption, which supports deleting or disabling customer root keys and provides new statuses, is now available in the Washington DC (WDC) region. All VPC service regions now support this feature.

## December 2020
{: #vpc-dec20}

### 22 December 2020
{: #vpc-dec2220}
{: release-note}

Customer-managed encryption (GA)
:   For block storage volumes and encrypted custom images, deleting or disabling a customer root key (CRK) is now managed by these VPC services. When you delete a root key, the resources become unusable for normal operations. A new `unusable` status and reason code `encryption_key_deleted` or `encryption_key_disabled` has been added to the API for `GET / volumes` and `GET / image` methods. These statuses also appear in the CLI and UI. For more information, see [Disabling root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys) and [Deleting root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys). For information on key states and resource statuses, see [User actions that impact root key states and resource status](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-root-key-states).

:   The Washington D.C. multi-zone region will be enabled in January 2021. This feature is available in all other multi-zone regions.


### 18 December 2020
{: #vpc-dec1820}
{: release-note}

Bring you own license (Beta)
:   You can now bring your own license (BYOL) when you import a custom image to {{site.data.keyword.vpc_short}}. This is a Beta feature that is available for evaluation and testing purposes. Contact your IBM Sales representative if you are interested in getting access. For more information, see [Bring your own license (Beta)](/docs/vpc?topic=vpc-byol-vpc-about).

### 17 December 2020
{: #vpc-dec1720}
{: release-note}

Checksum (SHA256) for imported images (Beta)
:   Now when you import a custom image, you can view the checksum that's generated for the image when it is imported to {{site.data.keyword.vpc_short}}. If you generate a checksum locally for your image before importing it, you can compare the two checksums to ensure that they are identical. For more information, see [Validating a custom image after importing (Beta)](/docs/vpc?topic=vpc-managing-images#validate-custom).
- **UI enhancement** Default boot volume names are now appended with a millisecond timestamp.

### 11 December 2020
{: #vpc-dec1120}
{: release-note}

VPN logging and auditing
:   The ability to monitor and audit your VPNs has been added to  {{site.data.keyword.vpn_vpc_short}}. For more information, see [Using IBM Log Analysis to view VPN logs](/docs/vpc?topic=vpc-using-log-analysis-to-view-vpn-logs).

### 01 December 2020
{: #vpc-dec0120}
{: release-note}

New SDK
:   The [Node SDK](https://{DomainName}/apidocs/vpc?code=node) is now generally available.

## November 2020
{: #vpc-nov20}

### 20 November 2020
{: #vpc-nov2020}
{: release-note}

Support for ingress routing
:   Ingress routing is included as part of custom routing tables, released on 30 October 2020. Ingress routing enables you to customize routes on incoming traffic to a VPC from traffic sources external to the VPC's availability zone (IBM Cloud Direct Link 2.0, IBM Cloud Transit Gateway, or another availability zone in the same VPC). For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

Support for datapath log forwarding with Log Analysis
:   Datapath log forwarding with {{site.data.keyword.la_full_notm}} is now available for IBM Cloud Application Load Balancer for VPC. Data and health check logs are valuable for debugging, analysis, and maintenance purposes. With the datapath logging feature enabled, your load balancer forwards these logs to your account's {{site.data.keyword.la_full_notm}} dashboard. For more information, see [Datapath log forwarding with Log Analysis](/docs/vpc?topic=vpc-datapath-logging#datapath-logging).

UI enhancement
:   You can now provision a VPC from the within the New virtual server for VPC page.

### 12 November 2020
{: #vpc-nov1220}
{: release-note}

Route-based virtual private network (VPN) gateways for VPC
:   Use route-based {{site.data.keyword.vpn_vpc_short}} gateways to manage large, on-premises or cloud-based networks that require the ability to statically route network and connectivity over a secure, encrypted connection. VPN for VPC provides UI, CLI, and API support in all MZRs. For more information, see [About VPN gateways](/docs/vpc?topic=vpc-using-vpn).

UI enhancement
:   You can now provision virtual server instances without first creating a VPC. Your virtual server instance is created in a default VPC.
:   The refresh data option is removed from all details pages. Instead, users can refresh by using the browser refresh.

### 06 November 2020
{: #vpc-nov0620}
{: release-note}

Proxy protocol support for IBM Cloud Application Load Balancer for VPC (ALB)
:   Proxy protocol provides a way to carry client information across proxies by appending it to the traffic your load balancer sends to the back-end servers. It also allows you to use listeners if incoming traffic contains proxy protocol information set by an intermediate proxy in between the client and your load balancer. See [Enabling proxy protocol](/docs/vpc?topic=vpc-advanced-traffic-management#proxy-protocol-enablement) for details.

### 02 November 2020
{: #vpc-nov0220}
{: release-note}

Custom routing tables and routes (GA)
:   {{site.data.keyword.cloud_notm}} Custom Routes for VPC enables you to configure and control the routing behavior of traffic within your VPC. This functionality provides the ability to explicitly manage traffic between and among subnets within a VPC; thereby, supporting network topologies that incorporate the use of virtual appliances, such as firewalls. This service also allows {{site.data.keyword.cloud_notm}} to continue to develop and deliver advanced features and capabilities for our {{site.data.keyword.cloud_notm}} services. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

IP spoofing (GA)
:   IP spoofing allows users to enable or disable the IP spoofing check on virtual server instances. This facilitates the traffic flow configured with custom routes functionality. For more information, see [About IP spoofing](/docs/vpc?topic=vpc-ip-spoofing-about).

## October 2020
{: #vpc-oct20}

### 30 October 2020
{: #vpc-oct3020}
{: release-note}

Virtual private endpoint gateways (GA)
:   {{site.data.keyword.cloud_notm}} Virtual Private Endpoints (VPE) for VPC is now generally available. This service enables you to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, allocated from a subnet within your VPC. For more information, see [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

### 1 October 2020
{: #vpc-oct0120}
{: release-note}

Encrypted images (GA)
:   Support for encrypting and importing a custom image is now generally available. For more information, see [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image).

UI enhancements
:   Improved name validation when an existing resource name is specified for a new resource

:   New way to view and select instance profiles in a side window when you provision an instance

:   Enhanced security group details page to include attached network interfaces, with a link to manage the interfaces

## September 2020
{: #vpc-sep20}

### 24 September 2020
{: #vpc-sep2420}
{: release-note}

New SDK
:   The [Java SDK](https://{DomainName}/apidocs/vpc?code=java) is now generally available.

### 04 September 2020
{: #vpc-sep0420}
{: release-note}

Network load balancer (GA)
:   The {{site.data.keyword.cloud_notm}} Network load balancer (NLB) for VPC is now generally available. Use the network load balancer to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [About IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-network-load-balancers).

### 01 September 2020
{: #vpc-sep0120}
{: release-note}

New region
:   The Sydney region endpoint is now in service at `https://au-syd.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region). See also [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API.

## August 2020
{: #vpc-aug20}

### 27 August 2020
{: #vpc-aug2720}
{: release-note}

Large profiles (GA)
:   The following two 48-core profiles are now generally available:

    * cx2-48x96
    * mx2-48x384

   For more information, see [Profiles](/docs/vpc?topic=vpc-profiles).

UI enhancements
:   New design for the VPC Overview page and generation switcher
:   Updated load balancer page to include auto scale enablement

### 24 August 2020
{: #vpc-aug2420}
{: release-note}

Auto scale (GA)
:   Auto scale is now generally available. Use auto scale to improve performance and costs by dynamically creating virtual server instances to meet the demands of your environment. For more information, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

### 12 August 2020
{: #vpc-aug1220}
{: release-note}

New image support
:   {{site.data.keyword.redhat_full}} Enterprise Linux 8 is now available in all regions.

### 07 August 2020
{: #vpc-aug0720}
{: release-note}

Virtual private endpoints (Beta)
:   Endpoint gateways for {{site.data.keyword.cloud_notm}} VPC are now available in a limited, allow-listed beta. Use virtual private endpoints (VPE) to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, which is allocated from a subnet within your VPC. For more information, see [About Virtual Private Endpoints for VPC (Beta)](/docs/vpc?topic=vpc-about-vpe).

UI enhancements
:   Improved name validation on provisioning pages
:   Eliminated polling on data tables
:   Enhanced data tables

## July 2020
{: #vpc-jul20}

### 30 July 2020
{: #vpc-jul3020}
{: release-note}

Larger volume capacity for block storage for VPC (Beta)
:   Provision volumes with capacities greater than 2 TB and up to 16 TB, depending on the profile you selected. See [Expanded Capacity IOPS Tiers (Beta)](/docs/vpc?topic=vpc-block-storage-profiles#tiers).

Expanding block storage for VPC volume capacity
:   Expand the size of new block storage volumes that you create and attach to a virtual server instance on Gen 2 Compute resources. Indicate capacity in GB increments up to 16 TB, depending on your volume profile limits. For more information, see [Expanding block storage volume capacity (Beta)](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

Encrypted images (Beta)
:   Import an encrypted custom image with beta support for the feature. For more information, see [Creating an encrypted custom image (Beta)](/docs/vpc?topic=vpc-create-encrypted-custom-image).

New SDK
:   The [Python SDK](https://{DomainName}/apidocs/vpc?code=python) is now generally available.

### 23 July 2020
{: #vpc-jul2320}
{: release-note}

Customer-managed encryption for block storage for VPC (GA)
:   Customer-managed encryption is now available for protecting boot and data volumes on Gen 2 compute resources. Import your own root keys to the cloud or create a key using Key Protect or Hyper Protect Crypto Services. Then, use your key to secure your data. For more information, see [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption).

Flow logs for VPC (GA)
:   FLow logs for {{site.data.keyword.cloud_notm}} VPC are now generally available. This service enables the collection, storage, and presentation of information about IP traffic flowing to and from network interfaces within a VPC. For more information, see [About IBM Cloud Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs).

### 16 July 2020
{: #vpc-jul1620}
{: release-note}

New region
:   The Tokyo region endpoint is now in service at `https://jp-tok.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

"Add to estimate" capability updates
:   Added support for itemized virtual server pricing in the Add to estimate capability.   

### 03 July 2020
{: #vpc-jul0320}
{: release-note}

Network load balancer (Beta)
:   Network load balancers are available in a limited beta. Use network load balancers to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [Load balancers overview](/docs/vpc?topic=vpc-nlb-vs-elb) and [About network load balancers](/docs/vpc?topic=vpc-network-load-balancers).

** {{site.data.keyword.vpn_vpc_short}} update**
:   Access VPN monitoring metrics by using [Monitoring VPN for VPC metrics](/docs/vpc?topic=vpc-vpn-monitoring-metrics).

## June 2020
{: #vpc-jun20}

### 26 June 2020
{: #vpc-jun2620}
{: release-note}

Dedicated hosts (Beta)
:   In a limited beta, use dedicated hosts to carve out a single-tenant compute node. For more information, see [Creating dedicated hosts and groups (Beta)](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances).

### 19 June 2020
{: #vpc-jun1920}
{: release-note}

Updated custom images information
:   A new [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images) checklist is available. The procedure for migrating a virtual server instance from classic infrastructure to {{site.data.keyword.cloud_notm}} VPC is enhanced. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc).

UI enhancements
:   Added pagination for the security groups list table

:   On the VPN Provision page, select a subnet from a table or create subnets

:   New "upgrade pending" button if a user account is pending upgrade

New SDK
:   The [Go SDK](https://{DomainName}/apidocs/vpc?code=go) is now generally available.

### 02 June 2020
{: #vpc-jun0220}
{: release-note}

IBM Cloud Virtual Servers for VPC on POWER service is deprecated
:   As of 02 June 2020, you cannot provision new instances. Any instance that is still provisioned as of 22 August 2020 will be deleted. For more information, see the [End of Service Announcement for Virtual Servers for VPC on POWER](https://www.ibm.com/cloud/blog/announcements/end-of-service-announcement-for-virtual-servers-for-vpc-on-power).

## May 2020
{: #vpc-may20}

### 21 May 2020
{: #vpc-may2120}
{: release-note}

Auto scale for VPC (Beta)
:   In this limited beta, use auto scale to improve performance and costs by dynamically creating virtual server instances to meet the demands of your environment. For more information, see [Creating an instance group for auto scaling (Beta)](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

### 15 May 2020
{: #vpc-may1520}
{: release-note}

**Load balancer for VPC update**
:   Back-end encryption (HTTPS for back-end pool) is now supported. See [Using load balancers for VPC](/docs/vpc?topic=vpc-load-balancers) for details.

### 1 May 2020
{: #vpc-may0120}
{: release-note}

Customer-managed encryption (Beta)
:   Customer-managed encryption (BYOK) is available in beta. Bring your own encryption keys and store the keys in Key Protect or Hyper Protect Crypto Services within VPC. Use BYOK to encrypt your VPC-enabled block storage at the volume level by using your own keys. For more information, see [Creating virtual server instances with customer-managed encryption (Beta)](/docs/vpc?topic=vpc-creating-instances-byok).

Flow logs (Beta)
:   Flow logs are now available in beta. Use flow logs to collect, store, and present the Internet Protocol (IP) traffic flowing to and from networks within your VPC. For more information, see [About flow logs (Beta)](/docs/vpc?topic=vpc-flow-logs).

UI enhancement
:   Create a new subnet directly from the page where you create a new instance.

## April 2020
{: #vpc-apr20}

### 22 April 2020
{: #vpc-apr2220}
{: release-note}

New Frankfurt region
:   The Frankfurt region endpoint (eu-de) is now in service at `http://eu-de.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

### 17 April 2020
{: #vpc-apr1720}
{: release-note}

Documentation correction
:   {{site.data.keyword.cloud_notm}} interprets volume capacity units in gibibytes, but the API documentation used gigabytes. This issue is now resolved in the documentation.

### 03 April 2020
{: #vpc-apr0320}
{: release-note}

Load balancer for VPC update
:   Access load balancer monitoring metrics (throughput, active connections, connection rate) using [{{site.data.keyword.mon_full_notm}}](/docs/vpc?topic=vpc-monitoring-metrics).

   The following cipher suites are supported for load balancer HTTPS listeners:

   - `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
   - `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
   - `TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256`
   - `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
   - `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
   - `TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256`

## March 2020
{: #vpc-mar20}

### 27 March 2020
{: #vpc-mar2720}
{: release-note}

{{site.data.keyword.mon_full_notm}} monitoring
:   Monitor virtual server instances using {{site.data.keyword.mon_full_notm}}. Use the new **Add monitoring** button on the instance's **Monitoring** page to provision an instance of the monitoring service. If a monitoring instance is already provisioned for the region, use the **Launch monitoring** button to view metrics associated with the instance. For more information, see [Monitoring metrics](/docs/vpc?topic=vpc-sysdig-monitoring-metrics).

Updated styling
:   VPC pages in {{site.data.keyword.IBM_notm}} console now use [Carbon 10](https://www.carbondesignsystem.com/){: external}, the IBM open source design system, which improves consistency and quality.

### 09 March 2020
{: #vpc-mar0920}
{: release-note}

IBM virtual servers for VPC on POWER
:   POWER-based instances are now generally available. For more information, see the following resources:

   - [IBM Cloud Virtual Private Cloud on POWER](https://developer.ibm.com/linuxonpower/power-virtual-private-cloud/){: external}  
   - [Profiles](/docs/vpc?topic=vpc-profiles)

### 02 March 2020
{: #vpc-mar0220}
{: release-note}

New region
:   The London region endpoint (eu-gb) is now in service at `http://eu-gb.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

UI enhancement
:   A new public gateway details page is available. For more information about public gateways, see [external connectivity](/docs/vpc?topic=vpc-about-networking-for-vpc#external-connectivity).

## February 2020
{: #vpc-feb20}

### 14 February 2020
{: #vpc-feb1420}
{: release-note}

New VPC network services (GA)
:   The following VPC network services are now generally available:

   - **Virtual private network (VPN):** Use VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premises private network or another VPC. For details, see [Using VPN](/docs/vpc?topic=vpc-using-vpn).
   - **Load balancers:** Create public and private load balancers to distribute traffic among multiple server instances within the same region of your VPC. For details, see [Using load balancers](/docs/vpc?topic=vpc-load-balancers).

### 13 February 2020
{: #vpc-feb1320}
{: release-note}

Red Hat Enterprise Linux (RHEL) and Windows stock images are now available
:   Provision an instance that uses an RHEL image or a Windows image in the Dallas region. For more information, see [Images](/docs/vpc?topic=vpc-about-images).

### 10 February 2020
{: #vpc-feb1020}
{: release-note}

API change notification
:   We have temporarily suspended API support for creating new instances from an existing boot volume. For more information, see t[API change log](/docs/vpc?topic=vpc-api-change-log).

### 03 February 2020
{: #vpc-feb0320}
{: release-note}

IBM virtual servers for VPC on POWER (Beta)
:   The beta is now open to anyone interested, no signup necessary. For more information, see the following resources:

   * [IBM Cloud Virtual Private Cloud on POWER](https://developer.ibm.com/linuxonpower/power-virtual-private-cloud/){: external}  
   * [Profiles](/docs/vpc?topic=vpc-profiles)

Red Hat Enterprise Linux (RHEL) and Windows stock images are now available
:   Provision an instance that uses an RHEL image or a Windows image in the Washington DC region. For more information, see [Images](/docs/vpc?topic=vpc-about-images).

**UI enhancement**
:   The modals for provisioning and attaching a public gateway and for creating an SSH key are now replaced with a redesigned side pane.

## January 2020
{: #vpc-jan20}

### 10 January 2020
{: #vpc-jan1020}
{: release-note}

New region
:   The Washington DC region endpoint (us-east) is now in service at `http://us-east.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

CLI plug-in release 0.5.10
:   For more information, see [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
    * You now have an Example section in command help for creating and updating commands. Example: `ibmcloud is help instance-create`, `ibmcloud is help instance-update`, or `ibmcloud is help volume-create`.
    * Use the _resource group filter_ in list commands. Example: `ibmcloud is vpcs --resource-group-name Littleton`.
    * JSON output format support for the `ibmcloud is target --json` command.
    * `ipv4` and `primary-network-interface` options for `instance-create`. Options help specify the primary private IP for the primary network interface when you create an instance.

## December 2019
{: #vpc-dec19}

### 09 December 2019
{: #vpc-dec0919}
{: release-note}

IBM virtual servers for VPC on POWER (Beta)
:   Create POWER-based instances for all virtual server families, including the addition of the GPU family (for POWER only). For more information, see [Profiles](/docs/vpc?topic=vpc-profiles).

UI enhancement
:   On the VPC details page, a new section is available to view source IP addresses for any cloud service endpoints you enabled. For more information, see [Service endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc).

CLI plug-in release 0.5.9
:   Target a specific resource group by using the '[-g YOUR_GROUP]' command option. If you specify the target resource group by using the `ibmcloud target [-g YOUR_GROUP]`command, the output displays only VPC resources inside of the specified resource group. This update also introduces enhancements to some of the commands for `get` and `list`, showing more detail for your VPC, instances, and instance profiles. For more information, see [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

### 02 December 2019
{: #vpc-dec0219}
{: release-note}

Access control lists
:   Use an access control list (ACL) to control all incoming and outgoing traffic in {{site.data.keyword.vpc_full}}. For more information, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).

## November 2019
{: #vpc-nov19}

### 14 November 2019
{: #vpc-nov1419}
{: release-note}

**VPC layout**
:   View resources that are associated with a VPC in the {{site.data.keyword.cloud_notm}} console. For more information, see [Viewing resources associated with a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#vpc-layout).

### 07 November 2019
{: #vpc-nov0719}
{: release-note}

Load balancer (Beta)
:   Use the {{site.data.keyword.cloud}} Load balancer for VPC service to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [Using load balancers (Beta)](/docs/vpc?topic=vpc-load-balancers).

VPN (Beta)
:   Use the {{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_short}} service to securely connect your VPC to another private network. For more information, see [Using VPN (Beta)](/docs/vpc?topic=vpc-using-vpn).

## October 2019
{: #vpc-oct19}

### 31 October 2019
{: #vpc-oct3119}
{: release-note}

Classic access
:   Set up access from a VPC to your {{site.data.keyword.cloud}} classic infrastructure, including Direct Link connectivity. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

Advanced routing
:   Control the flow of network traffic in your VPC by configuring VPC routes. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

## June 2019
{: #vpc-oct19}

### 04 June 2019
{: #vpc-jun-0419}
{: release-note}

Introducing {{site.data.keyword.vpc_short}}
:   {{site.data.keyword.vpc_full}} is now generally available in the Dallas, Frankfurt, and Tokyo regions. For more information, see [{{site.data.keyword.vpc_short}}](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-virtual-private-cloud).
