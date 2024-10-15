---

copyright:
  years: 2019, 2024
lastupdated: "2024-10-15"

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

For more information about changes to the {{site.data.keyword.vpc_short}} command line interface (CLI), see [{{site.data.keyword.vpc_short}} CLI release notes](/docs/vpc?topic=vpc-vpc-cli-rn).
  
## October 2024
{: #vpc-oct24}

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
{: #vpc-sept24}

### 30 September 2024
{: #vpc-sept3024}
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
:   The Red Hat Enterprise AI (RHEL AI) operating system can be imported as a bring your own license (BYOL). A RHEL AI qcow2 file is available directly from Red Hat. For more information, see [Red Hat Enteprise Linux AI BYOL custom images](/docs/vpc?topic=vpc-planning-custom-images#rhel-ai-byol-custom-images).

### 03 September 2024
{: #vpc-sep0324}
{: release-note}

GPU H100 profile available in select regions (select availability)
:   The GPU H100 profile is now available on the latest generation GPU-enabled infrastructure for running machine learning (ML) and deep learning (DL) frameworks in support of AI initiatives. The GPU H100 profile is available in the following regions: London (`eu-gb`), Sydney (`au-syd`), Toronto (`ca-tor`), Madrid (`eu-es`), and Washington DC (`us-east`). When you use the H100 virtual server profile, it runs on an [NVIDIA Hopper-based HGX](https://www.nvidia.com/en-us/data-center/hgx/){: external} server and is the sole tenant running on the host. For more information about the `gx3d-160x1792x8h100` profile, see [GPU](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) profiles.

## August 2024
{: #vpc-aug24}

### 28 August 2024
{: #vpc-aug2824}

Reservations for Bare Metal Servers for VPC (beta)
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
:   {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) is now available in the Frankfurt (`eu-de`) region in {{site.data.keyword.cloud_notm}}. For more information, see [{{site.data.keyword.waziaas_full_notm}} product page](https://www.ibm.com/cloud/wazi-as-a-service){: external}.

### 7 August 2024
{: #vpc-aug0724}
{: release-note}

UI enhancement: Filter instance profiles by business scenario
:   When provisioning a virtual server, you can now use the **By scenario** tab on the Select an instance profile page to narrow the results to include only applicable instance profiles. For example, you can filter profiles by the following business scenarios: SAP; Web Development and Test; HPC; Confidential computing; AI, Deep learning & Machine learning; Visualizations, VDI; and Storage optimized. When a specific filter is selected, the profile results display only the profiles related to the defined business scenario.

### 5 August 2024
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
:  The new Update firmware action on Bare Metal Servers for VPC is now generally available. You can see if a firmware update is available for your bare metal server and also initiate the update. You can use the UI, CLI, and API to update the firmware. In the UI, this action is only visible if the server is stopped and there is a firmware update available. It is recommended to back up your bare metal server before any firmware update. For more information, see [Managing Bare Metal Servers for VPC](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=ui).

### 20 June 2024
{: #vpc-jun2024}
{: release-note}

UI Enhancements to Images for VPC
:   The Images for VPC UI includes multiple enhancements. When you click any image name, a side panel is displayed for that specific image. From this Details page, you can review both the Details and IDs for the selected image. You can also click **Continue to provisioning** which takes you to **Virtual server for VPC**, where you can create a virtual server instance with the selected image.

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

Update firmware on Bare Metal Servers for VPC (beta)
:   With the new Update firmware action on Bare Metal Servers for VPC, you can see if a firmware update is available for your bare metal server and also initiate the update. You can use the UI, CLI, and API to update the firmware. In the UI, this action is only visible if the server is stopped and there is a firmware update available. It is recommended to back up your bare metal server before any firmware update. For more information, see [Managing Bare Metal Servers for VPC](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=ui).

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
:   You can now attach both primary and secondary IP addresses to a security group to refine the binding of security groups rules to a particular port IP instead of all IPs belonging to the port. Also, security group rules now support both source and destination on ingress and egress rules. This allows customers with multiple, secondary private IP addresses associated with a single vNIC to have the ability to apply security group rules to source and destination IP addresses, thus enabling finer granularity in security rules. This enhancement provides the capability to secure the primary IP different from the secondary IPs, and also applies to VIP prefixes (custom routes) used with a vNIC with IP spoofing disabled. For more information, see [Applying security group rules to source and destination IP addresses](/docs/vpc?topic=vpc-security-groups-rules&interface=ui).

### 29 May 2024
{: #vpc-may2924}
{: release-note}

Confidential computing with Intel Software Guard Extensions (SGX) for Virtual Servers for VPC (select availability)
:   Confidential computing with Intel&reg; Software Guard Extensions (SGX) protects your data through hardware-based server security by using isolated memory regions that are known as encrypted enclaves. This hardware-based computation helps protect your data from disclosure or modification. Which means that your sensitive data is encrypted while it is in virtual server instance memory by allowing applications to run in private memory space. To use SGX, you must install the SGX drivers and platform software on SGX-capable worker nodes. Then, design your app to run in an SGX environment. Confidential computing with Intel SGX for VPC is available only in US-South (Dallas) region. For more information, see [Confidential computing with Intel Software Guard Extensions (SGX) for Virtual Servers for VPC](/docs/vpc?topic=vpc-about-sgx-vpc).

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

Third-party image billing and metering (beta)
:   When you select a catalog image, you now have associated billing plans to choose from. Catalog images are billed in one of the following ways:

   * Free trial
   * Usage-based billing
   * BYOL

### 09 April 2024
{: #vpc-apr0924}
{: release-note}

Generic operating system custom images with Virtual Server Instances and Bare Metal Servers for VPC (beta)
:   When you create a server on {{site.data.keyword.vpc_full}} (VPC) using an x86 profile, you can use an operating system that is not listed in IBM Cloud by specifying a generic operating system custom image. You can create this custom image by specifying one of the new operating systems with properties that indicate it is generic. When you provision a server by using a generic operating system custom image, most operating system-specific provisioning steps aren't performed, such as console setup and automatic registration. You must provide the appropriate user data if you want your generic operating system custom image to perform these steps. For more information, see [Generic operating system custom images](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#generic-os-custom-images) and [Creating a generic operating system custom image](/docs/vpc?topic=vpc-create-generic-os-custom-image&interface=ui).

Network boot of operating systems with Bare Metal Servers for VPC (beta)
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
:   VMware ESXi image on Bare Metal Servers for VPC will no longer be available when you provision a bare metal for VPC servers. If looking to deploy a VMware solution in VPC, consider provisioning VMware Cloud Foundation (VCF) through IBM Cloud VMware Solutions. For more information about this solution, see [VMware Cloud Foundation overview](/docs/vmwaresolutions?topic=vmwaresolutions-vpc-vcf-ovw). For more information on the updated packaging and pricing for VMware® portfolio, see [Packaging and pricing for VMware by Broadcom](/docs/vmwaresolutions?topic=vmwaresolutions-vmwaresol_packaging-pricing).

Next generation instance profiles available in Washington DC region (select availability)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are now available as a select availability offering in the Washington DC (`us-east`) region, in addition to the Dallas (`us-south`), London (`eu-gb`), and Frankfurt (`eu-de`) regions. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) family, see the *bx3d* profiles tab. In the [Compute](/docs/vpc?topic=vpc-profiles&interface=ui#compute) family, see the *cx3d* profiles tab. In the [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) family, see the *mx3d* profiles tab. 3rd generation dedicated host profiles are also available. For more information, see *bx3d*, *cx3d*, and *mx3d* profiles in [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 26 March 2024
{: #vpc-mar2624}
{: release-note}

Security group support for secondary IP addresses (select availability)
:   Accounts that are granted special approval to preview this feature can now attach both primary and secondary IP addresses to a security group to refine the binding of security groups rules to a particular port IP instead of all IPs belonging to the port. Also, security group rules now support both source and destination on ingress and egress rules. This allows customers with multiple, secondary private IP addresses associated with a single vNIC to have the ability to apply security group rules to source and destination IP addresses, thus enabling finer granularity in security rules. This enhancement provides the capability to secure the primary IP different from the secondary IPs, and also applies to VIP prefixes (custom routes) used with a vNIC with IP spoofing disabled. For more information, see [Applying security group rules to source and destination IP addresses](/docs/vpc?topic=vpc-security-groups-rules&interface=ui).

### 25 March 2024
{: #vpc-mar2524}

Reservations for VPC (GA)
:   Reservations for VPC are now GA (generally available). A reservation is a great option if you want guaranteed resources for future deployments and cost savings. You can choose between either a 1 or 3-year contract term for your reservation. Reservations are available the Dallas (`us-south`), Washington DC (`us-east`), São Paulo (`br-sao`), Toronto (`ca-tor`), London (`eu-gb`), Frankfurt (`eu-de`), Madrid (`eu-es`), Osaka (`jp-osa`), Tokyo (`jp-tok`), and Sydney (`au-syd`) multizone regions (MZRs). For more information, see [About reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc).

### 22 March 2024
{: #vpc-mar2224}
{: release-note}

Private Path Services for VPC (Beta)
:   Accounts that are granted special approval to preview this feature can now create a [Private Path service](/docs/vpc?topic=vpc-private-path-service-intro&interface=ui) and [Private Path network load balancer](/docs/vpc?topic=vpc-ppnlb-ui-creating-private-path-network-load-balancer&interface=ui).

   Private Path services provide private connectivity for IBM Cloud and third-party services. A Private Path service requires a Private Path network load balancer to deploy a service on IBM Cloud and a Virtual Private Endpoint (VPE) gateway for consumers to connect to the service. Traffic stays on the IBM Cloud backbone without traversing the public internet.

   For more information, see the [Private Path solution guide](/docs/private-path?topic=private-path-overview). If you are interested in getting early access to this beta offering, contact your IBM Support representative.
   {: note}

### 21 March 2024
{: #vpc-mar2124}
{: release-note}

UI Enhancement to SSH Keys
:   The UI process for uploading SSH keys is improved where you **Select SSH key input method**. Improvements were made to the ability to copy and paste, drag and drop, and the upload buttons. Previously, these three actions were all individually located. These actions are now combined to use the same space within the UI.

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
:   New `l40S` GPU profiles that include NVIDIA's L40S 48GB GPU are now available in the Frankfurt (`eu-de`), Madrid (`eu-es`), Sydney (`au-syd`), and Tokyo (`jp-tok`) regions. For more information, see [GPU x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

GPU l4 profiles now available
:   New `l4` GPU profiles that include NVIDIA's L4 24GB GPU are now available in the Toronto (`ca-tor`), London (`eu-gb`), Frankfurt (`eu-de`), Madrid (`eu-es`),  Sydney (`au-syd`), and Tokyo (`jp-tok`) regions. For more information, see [GPU x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 23 February 2024
{: #vpc-feb2324}
{: release-note}

London 1 AZ for bare metal servers
:   The London 1 availability zone (AZ) is now available for Bare Metal Servers for VPC.

### 20 February 2024
{: #vpc-feb2024}
{: release-note}

Virtual Network Interfaces for VPC (Select availability)
:   Accounts that have been granted special approval can preview a new feature in the Virtual Private Cloud (VPC) service that expands the support for [virtual network interfaces](/docs/vpc?topic=vpc-vni-about). The following features are available.
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
:   The following table shows activity tracker events that have been corrected.

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

Reserved Capacity for VPC (Beta)
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
:   The GPU `a100` profile is now availableon the Intel&reg;'s quad processor Xeon® Gold 6342 Ice Lake with 96 cores that are running at a base speed of 2.8 GHz and an all-core turbo frequency of 3.5 GHz. The Ice Lake processor is available only in the Washington DC (`us-east`) region. For more information, see the [GPU profile family](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) documentation.

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
:   {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) is now available in the Dallas (`us-south`) region in {{site.data.keyword.cloud_notm}}. For more information, see [{{site.data.keyword.waziaas_full_notm}} product page](https://www.ibm.com/cloud/wazi-as-a-service){: external}.

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
:   {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) is now available in the Madrid (`eu-es`) region in IBM Cloud. For more information, see [{{site.data.keyword.waziaas_full_notm}} product page](https://www.ibm.com/cloud/wazi-as-a-service){: external}.

### 21 September 2023
{: #vpc-sep2123}
{: release-note}

IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}}
:   You can now create IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instances on LinuxONE (s390x processor architecture) in the Madrid (`eu-es`) region, in addition to São Paulo (`br-sao`), Toronto (`ca-tor`), Tokyo (`jp-tok`), and Washington DC (`us-east`) regions. To create IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers), and [IBM Hyper Protect Container Runtime image](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime). A valid contract is required for creating an instance. For more information, see [About the contract](/docs/vpc?topic=vpc-about-contract_se&interface=ui).

LinuxONE (s390x processor architecture)
:   You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud in the Madrid (`eu-es`) region, in addition to Tokyo (`jp-tok`), São Paulo (`br-sao`), Toronto (`ca-tor`), and London (`eu-gb`) regions. For more information about available LinuxONE (s390x processor architecture) profiles, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

## 14 September 2023
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
:   The Ultra High Memory profile family (`ux2d`) is now available in the United Kingdome (London) region. For more information about this profile family, see [Ultra High Memory](/docs/vpc?topic=vpc-profiles&interface=ui#uhmemory). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).


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

Next generation instance profiles (Beta)
:   The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are available as a beta offering to select customers. This new generation features virtual server profile families hosted exclusively on 4th Generation Intel&reg; Xeon&reg; Scalable processors to provide the most powerful and performant general-purpose profiles available. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles) and the *bx3d* and *mx3d* profiles in the [Balanced](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) and [Memory](/docs/vpc?topic=vpc-profiles&interface=ui#memory) profile families.

### 22 August 2023
{: #vpc-august2223}
{: release-note}

VPC Status History
:   You can now customize notification settings for your VPC dashboard. When status history is enabled, your notification history is retained to help you find old error messages or track down when the creation or deletion of a resource occurred. To enable Status History, make sure that your browser alllows local storage access. For more information, see [Enabling local storage in your browser](/docs/vpc?topic=vpc-enable-local-browser-storage).

### 15 August 2023
{: #vpc-august1523}
{: release-note}

Metadata Instance identity certificates
:   You can now use the instance identity access token and a Certificate Signing Request (CSR) to [create](/apidocs/vpc-metadata-beta/initial#create-certificate) an instance identity certificate with the Metadata API. For more information, see [Generating an instance identity certificate by using an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-acquire-certificate). Instance identity certificates can be used when the traffic between an authorized client and the mounted file share is [encrypted in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit).

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
:  The Ed25519 SSH key type is a new, supported SSH key type and can be used as an alternative to the RSA SSH key type. The Ed25519 SSH key can be used with Linux operating systems, but is not supported for Windows or VMware images. For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys) and [Managing SSH Keys](https://cloud.ibm.com/docs/vpc?topic=vpc-managing-ssh-keys).

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
   - In the `workload` section of the contract, you can define the workload via Pod descriptors. Each pod can contain one or more container definitions. Previously, only one container described by docker compose was supported. For more information about using Pod descriptors, see the [`play` subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_play). Container images described by Pod descriptors can be [validated by RedHat Simple Signing](/docs/vpc?topic=vpc-about-contract_se#container-image-by-pod-descriptors).

   Changes to the attestation document
   - In the attestation document **se-checksums.txt**, `user-data.decrypted` is removed, and `Machine Type/Plant/Serial` (the information required to identify the host machine) is added. For more information, see [Attestation](/docs/vpc?topic=vpc-about-attestation).

Instance group integration with network load balancers (GA)
:  Network Load Balancer for VPC is now integrated with instance groups to improve pool member scaling. When you create or update an instance group for auto scaling, you can now specify the Network Load Balancer pool for the instance group to manage. For more information see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

Access control modes and granular authorization for File Storage for VPC file shares (beta)
:   For users with accounts that have access to file shares, you can now specify an access control mode to either restrict mounting a file share to a specific virtual server instance in the VPC or allow VPC-wide file share mounting. File share mount targets that were created before `20-June-2023` have a default of VPC-wide file share mounting. File shares that are created after that date can specify security group access control mode to restrict access to a specific instance. For this option, file shares must be based on the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#dp2-profile). From the UI, CLI, or API, you set the access control mode when you create or update file shares, and can see the setting when you list file shares and in the file share details. When you create a mount target for a file share with security group access mode, you can specify a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to be created and attached to the mount target with a [security group](/docs/vpc?topic=vpc-using-security-groups). For the virtual network interface, you can specify an existing reserved IP, or specify a subnet and allow the system to assign an IP address. When the mount target is attached and the share is mounted, the virtual network interface performs security group policy check to ensure only authorized virtual server instances can communicate with the share. For more information, see [Mount targets for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets).

Data encryption in transit for file shares (beta)
:  For users with accounts that have access to file shares, you can enable secure end-to-end encryption of your data when using security group based access control on file shares and mount targets with virtual network interfaces. The traffic between the authorized virtual server and the file share can optionally be IPsec encapsulated by the client. By using an Internet Security Protocol (IPsec), you can establish an encrypted mount connection between the virtual server instance and a file share with the `dp2` profile. The {{site.data.keyword.cloud}} file service provides a [mount helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) to automate the complex tasks of configuring and maintaining the connection. For more information, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).

Virtual network interface (beta)
:  Virtual network interfaces are now available in a beta release for use with file share mount targets. For more information see [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about).

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

{{site.data.keyword.filestorage_vpc_short}} file share activity tracker event name changes (beta)
:    For users with accounts that have access to file shares, when making API requests using a `version` query parameter of `2023-05-30` or later, the shares `targets` property was changed to `mount_targets`. This change affects file share Activity Tracker events. Events generated when [creating](/apidocs/vpc-beta/initial#create-share-mount-target), [listing](/apidocs/vpc-beta/initial#list-share-mount-targets), [retrieving](/apidocs/vpc-beta/initial#get-share-mount-target), [deleting](/apidocs/vpc-beta/initial#delete-share-mount-target), and [updating](/apidocs/vpc-beta/initial#update-share-mount-target) mount targets for a file share are now `is.share.mount-target.create`, `is.share.mount-target.list`,`is.share.mount-target.read`, `is.share.mount-target.delete`, and `is.share.mount-target.update`. Events for `is.share.target.create`, `is.share.target.list`, `is.share.target.read`, `is.share.target.delete`, and `is.share.target.update` are deprecated and will be removed in a future API release per the VPC beta API [versioning policy](/apidocs/vpc-beta/initial#api-versioning-beta).

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

### 9 May 2023
{: #vpc-may0923}
{: release-note}

New `-a100` GPU profile is available (select availability)
:   There is a new profile available for customers with special approval to preview this service that is for provisioning instances based on NVIDIA's A100 Amperere GPU attached to a single virtual server instance. The `gx2-80x1280x8a100` profile supports artificial intelligence and machine language frameworks and includes instance storage. Only Redhat and Ubuntu are supported for this profile. This profile is currently only available in the Washington DC (`us-east`) region. For more information, see [GPU profiles](/docs/vpc?topic=vpc-profiles&interface=ui#gpu). To request access to this profile, you must open a [support case](https://cloud.ibm.com/unifiedsupport/supportcenter).

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

### 6 April 2023
{: #vpc-apr0623}
{: release-note}

Image lifecycle management for custom images (beta)
:   For customers with special access to this feature, you can use the UI, CLI, and API to manage the lifecycle of your custom images with the following three statuses. You can move the image back and forth through all the statuses. You can also schedule status changes to manage the entire lifecycle of the image. For more information, see [Custom image lifecycle](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#custom-image-lifecycle) in [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images&interface=ui).
    * `available`: The image can be used to create an instance.
    * `deprecated`: The image can be used to create an instance. Using the `deprecated` status can discourage use of the image before the status changes to `obsolete`.
    * `obsolete`: The image can't be used to create an instance.

### 4 April 2023
{: #vpc-apr0423}
{: release-note}

{{site.data.keyword.filestorage_vpc_short}} high-performance profile (beta)
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

VPC instance metadata communication protocol and hop limit
:    You can now control the communication protocol and hop limit for IP response packets that are used by the [VPC Instance Metadata service](/docs/vpc?topic=vpc-imd-about). When you provision or update an instance, use the new **Secure access** setting to specify either `http` (default) or `https` (secure access) communication. In addition, use the new **Hop limit** setting to specify a value between `1` (default) and `64`. Both of these settings apply only when the metadata service is enabled. For more information, see [Configure metadata settings by using the UI](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#metadata-config-ui).

Hyper Protect Secure Build
:    You can now use [Hyper Protect Secure Build](/docs/vpc?topic=vpc-about-hpsb) to securely build an Open Container Initiative (OCI) image in [Hyper Protect Virtual Servers for VPC](/docs/vpc?topic=vpc-about-se). You can push the image to DockerHub or IBM Cloud Container Registry (ICR). Later, you can pull the image from the registry to provision it in another {{site.data.keyword.hpvs}} for VPC instance. You can also pull SLES BaseContainerImages (BCI) from the SUSE registry, and use the images to provision {{site.data.keyword.hpvs}} for VPC instances.

### 9 February 2023
{: #vpc-feb0923}
{: release-note}

Export custom images (Beta)
:   For accounts that are authorized to preview this feature, you can now export custom images to {{site.data.keyword.cos_full_notm}}. For more information, see [Exporting a custom image to IBM Cloud Object Storage (Beta)](/docs/vpc?topic=vpc-managing-custom-images&interface=ui#custom-image-export-to-cos).

### 7 February 2023
{: #vpc-feb0723}
{: release-note}

Block Storage fast restore snapshots
:    You can now restore a fully provisioned volume with all its data from a snapshot by using a fast restore snapshot clone. You can use fast restor to restore a volume more quickly than restoring from a regular snapshot. To create the clone, you specify a zone or zones in the same region as the source snapshot. The clone is used to automatically restore a volume with all of its data in the zone where the clone exists. For more information, see [Restoring a volume by using fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-use-fast-restore).

Extra security for VPC snapshots (closed beta)
:    For customers with special access to this security beta feature, data isolation is provided to store snapshots created from your dedicated hosts. With data isolation extra security, your data is encrypted at rest with a unique key and access to your data is protected by a private firewall.

### 3 February 2023
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

Secure boot with Trusted Plaform Module (TPM) (select availability)
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

   Effective today, these ciphers are no longer supported in the UI and EOS for use with the CLI and API is forthcoming. If you didn't upgrade to more secure ciphers, do so now.


## December 2022
{: #vpc-dec22}

### 20 December 2022
{: #vpc-dec2022}
{: release-note}

Instance provision by volume (Beta)
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
   - Starting from `ibm-hyper-protect-container-runtime-1-0-s390x-7-encrypt.crt` and `ibm-hyper-protect-container-runtime-1-0-s390x-7-attestation.crt`, the certificates contain **Certificate Revocation List (CRL) Distribution Points**. You can use the CRL to verify that your certificates are valid (not revoked). For more information, see [Certificate revocation list](/docs/vpc?topic=vpc-cert_validate#certificate-revocation-list).

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

## November 2022
{: #vpc-nov22}

### 14 November 2022
{: #vpc-nov1422}
{: release-note}

IBM Hyper Protect Container Runtime image `ibm-hyper-protect-container-runtime-1-0-s390x-6` updates
:  For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-6`, new certificates are available.
   - Attestation certificate
   - Encryption certificate
   - Intermediate certificate

   Logging for {{site.data.keyword.hpvs}} for VPC
   - Apart from IBM Log Analysis, you can now configure logging with a generic syslog backend. For more information, see [Logging for {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc#syslog).

   Security claims on disks encryption
   - Both the root disk and data disks in the {{site.data.keyword.hpvs}} for VPC instance are configured with Linux Unified Key Setup (LUKS) Encryption. You can verify the encryption status by checking related messages in the log. For more information, see [Verifying disk encryption status](/docs/vpc?topic=vpc-hpvs-disks-encryption-validate).

### 11 November 2022
{: #vpc-nov1122}
{: release-note}

Access management tags to manage VPC resources
:   You can now use access management tags to control access to VPC resources, such as virtual server instances and Block Storage volumes. See the [Access management tags](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-access-management-tags) section in [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started). For more information about using access management tags, see the following IAM resources:
    * [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial) UI tutorial
    * [Working with tags](/docs/account?topic=account-tag&interface=ui)
    * [Granting users access to tag resources and service IDs](/docs/account?topic=account-access&interface=ui)

### 8 November 2022
{: #vpc-nov0822}
{: release-note}

Backup policy jobs
:    You can now list all jobs for a backup policy and retrieve backup policy job details from the UI, CLI, or API. Backup policy job information includes the backup plan used to create the backup snapshot, the backup job type, job status, source volume, target snapshot, and additional information. For more information, see [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## October 2022
{: #vpc-oct22}

### 31 October 2022
{: #vpc-oct3122}
{: release-note}

Terraform is now available for sharing images across an enterprise account
:    You can now share or publish custom images by using Terraform to other accounts within your enterprise by using a private catalog. If you select a catalog image that belongs to a different account, review [Using cross-account image references in a private catalog in Terraform](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=terraform#private-catalog-image-reference-vpc-terraform) for additional considerations and limitations. To create a private catalog, see the tutorial [Onboarding a virtual server image with Terraform](/docs/account?topic=account-catalog-vsi-tutorial&interface=ui). To create an instance from a catalog image using Terraform, see [Creating virtual server instances by using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-terraform).

### 21 October 2022
{: #vpc-oct2122}
{: release-note}

Context-based restrictions (limited availability)
:   For accounts authorized to preview this functionality, account owners and administrators are now able to define and enforce network access policies for IBM Cloud VPC resources. For more information and for specific VPC Infrastructure services that are supported for context-based restrictions, see [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions (limited availability)](/docs/vpc?topic=vpc-cbr).

### 19 October 2022
{: #vpc-oct1922}
{: release-note}

Windows BYOL for multi-tenant hosts
:   You can now bring your own license for Windows operating systems with a custom image to provision virtual server instances on multi-tenant hosts. Previously Windows BYOL was limited to dedicated hosts. For more information, see [Bring your Own License](/docs/vpc?topic=vpc-byol-vpc-about) and [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images).


## 3 October 2022
{: #vpc-oct0322}

VPC Public Ingress Routing
:   You can now route public internet ingress traffic (destined to a floating IP) to a VPC next-hop IP. For more information, see [Creating a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table) and limitations and guidelines for [Ingress routes](/docs/vpc?topic=vpc-about-custom-routes&interface=ui#routes-ingress).

   Currently, public ingress routing (`public internet` traffic choice) is available in the UI and API only. CLI is forthcoming.
   {: note}

Flow Logs for VPC
:   Flow log collectors now support the following cross-account service virtual interfaces at VPC and subnet levels:

   - IBM Cloud Kubernetes Service (IKS) workers
   - RedHat OpenShift Kubernetes Service (ROKS)
   - Load Balancer as a Service (LBaaS)

   For more information, see [About IBM Cloud Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs).

## September 2022
{: #vpc-september22}

### 23 September 2022
{: #vpc-september2322}

IBM Wazi as a Service
:   {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) is now generally available in IBM Cloud in Tokyo (`jp-tok`), São Paulo (`br-sao`), Toronto (`ca-tor`), London (`eu-gb`), and Washington DC (`us-east`) regions. For more information, see [{{site.data.keyword.waziaas_full_notm}} product page](https://www.ibm.com/cloud/wazi-as-a-service){: external}.
* For the latest updates of z/OS dev and test stock images, see [Change log for z/OS stock images](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=vpc-change-log-zos-stock-images){: external}.
* For instructions on creating z/OS virtual server instances, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).


Network interfaces for virtual servers
:   You can now add up to 15 network interfaces to virtual server instances. The number of interfaces that a virtual server supports depends on the vCPU count that is in the [instance profile](/docs/vpc?topic=vpc-profiles). Profiles that include 17 - 48 vCPUs now support up to 10 network interfaces. Profiles that include 49 or more vCPUs now support up to 15 network interfaces. For existing virtual servers with 17 or more vCPUs to take advantage of the new network interface limits, a running virtual server instance must be stopped and restarted. For more information about multiple network interfaces, see [Managing network interfaces](/docs/vpc?topic=vpc-using-instance-vnics).

### 22 September 2022
{: #vpc-september2222}
{: release-note}

Sharing images across an enterprise account
:    You can now share or publish custom images to other accounts within your enterprise by using a private catalog. A private catalog provides a way for you to manage access to products for multiple accounts. You can use any existing x86 virtual server custom image with a private catalog, with the exception of an encrypted image. For more information, see [Getting started with Catalog Images on VPC](/docs/vpc?topic=vpc-getting-started-images-on-vpc-catalog&interface=terraform) and the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial). Custom images can't be deleted while being managed from a catalog and can only be managed from one catalog product offering version at a time. Deleting the catalog does not free its managed resources for a 7-day reclamation period. For more information, see [Deleting a custom image in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui#deleting-private-catalog-custom-image-vpc) and [Using resource reclamations](/docs/account?topic=account-resource-reclamation). If you plan to share images with other accounts, users in those accounts should be aware of considerations related to cross-account references to those images. For more information, see [Using cross-account image references in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui#private-catalog-image-reference-vpc-ui). Custom images can also be published to the IBM Cloud catalog and to other (non-enterprise) accounts. This process requires onboarding to the [IBM Cloud Partner Center](https://cloud.ibm.com/partner-center/sell).

Deprecated VPN for VPC ciphers
:    The following VPN for VPC IKE and IPsec ciphers are now deprecated:
   - Authentication algorithms `md5` and `sha1`
   - Encryption algorithm `triple_des`
   - Diffie–Hellman groups `2` and `5`

   You have until 13 December 2022 to upgrade to more secure ciphers. After this date, VPN connections using deprecated ciphers show a `status` of `down` (and no longer transfer data) until you upgrade from the weak cipher.

Additional VPN for VPC ciphers
:    VPN gateways now provide new algorithms to help meet your security and compliance requirements.

   * IKE policy now supports the `sha384` value for authentication, `aes192` for encryption, and `15`, `16`, `17`, `18`, `20`, `21`, `22`, `23`, `24`, and `31` values for Diffie–Hellman groups.

   * IPsec policy now supports `sha384` and `disabled` values for authentication, `aes192`, `aes128gcm16`, `aes192gcm16`, and `aes256gcm16` values for encryption, and `group_15`, `group_16`, `group_17`, `group_18`, `group_20`, `group_21`, `group_22`, `group_23` , `group_24`, and `group_31` Diffie–Hellman groups.

   Specifying IKE and IPsec policies when configuring a VPN connection is optional. If a policy is not selected, one is chosen through *auto-negotiation*. For more information, see [About policy negotiation](/docs/vpc?topic=vpc-using-vpn#policy-negotiation).
   {: note}

### 20 September 2022
{: #vpc-september2022}
{: release-note}

Updating subnets for existing application load balancers
:   You can now add or remove subnets for existing ALBs by using the UI, API, or CLI. ETag support was added for load balancer resources, as it is required for any resource that allows arrays to be updated. For more information, see to [Updating subnets for existing application load balancers](/docs/vpc?topic=vpc-alb-updating-subnets&interface=ui).

:   For more information about using ETags, see [Concurrent update protection](/apidocs/vpc/latest#concurrent-update-protection).

### 12 September 2022
{: #vpc-september1222}
{: release-note}

New stock images for VPC
:   The following stock images are now available for x86-64 virtual server instances:
    - The Windows Server 2019 Standard Edition with SQL Server 2019 Web Edition bundle is now supported as an image when you provision a   {{site.data.keyword.vpc_short}} server. If you select this stock image when you provision a virtual server instance, the software that is part of that bundle is also included in your instance.
    - SUSE Linux Enterprise Server is now supported as an operating system stock image when you provision an x86-64 virtual server. For more information, see [x86-64 virtual server images](/docs/vpc?topic=vpc-about-images).

## August 2022
{: #vpc-august22}

### 31 August 2022
{: #vpc-august3122}
{: release-note}

Configuration governance
:   New VPC config rules for the Image service and Virtual Servers are now available with the Security and Compliance Center. For more information, see the [Security and regulation compliance](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance) section of [Understanding your responsibilities when using Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc).

### 30 August 2022
{: #vpc-august3022}
{: release-note}

IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}}
:   You can now create IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instances on LinuxONE (s390x processor architecture) in the London (`eu-gb`) region, in addition to São Paulo (`br-sao`), Toronto (`ca-tor`), Tokyo (`jp-tok`), and Washington DC (`us-east`) regions. To create IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers), and [IBM Hyper Protect Container Runtime image](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime). A valid contract is required for creating an instance. For more information, see [About the contract](/docs/vpc?topic=vpc-about-contract_se&interface=ui).

### 23 August 2022
{: #vpc-august2322}
{: release-note}

Additional user tag support for boot and data volumes
:  You can now add user tags to boot and data volumes when provisioning a virtual server instance or creating an instance template. You can add tags to the boot volume by editing it during instance provisioning. You can also add user tags for any data volumes you create and attach. If you import from a snapshot, any tags defined for the snapshot will be applied to the new volume. For more information, see [Create and attach a Block Storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage&interface=api). For more information about user tags, see [Working with tags](/docs/account?topic=account-tag&interface=api).

### 17 August 2022
{: #vpc-august1722}
{: release-note}

New stock image for bare metal servers
:   Debian 11 is now supported as an image when you provision a bare metal server.

### 15 August 2022
{: #vpc-august1522}
{: release-note}

Health check metrics for application load balancer pools
:   Two new health check metrics have now been added to IBM Cloud Monitoring for application load balancers, specifically for pools:

* A metric for reporting the total number of members in a pool.
* A second metric for reporting the total number of healthy (or active) members in a pool.

:   For more information about these new metrics or the IBM Cloud Monitoring service, refer to [Monitoring Application Load Balancer for VPC metrics](/docs/vpc?topic=vpc-monitoring-metrics-alb).

### 09 August 2022
{: #vpc-august0922}
{: release-note}

VPC Instance Metadata service
:   A restriction was removed from the [instance metadata service](/docs/vpc?topic=vpc-imd-about&interface=ui) in which you had to stop and restart the virtual server instance to fully enable the metadata service. You can now create instances with the metadata service disabled and then enable the service for the instance to start using it immediately.

### 08 August 2022
{: #vpc-august0822}
{: release-note}

Virtual servers for VPC
:   You can now use the UI, CLI, and API to specify user tags for volume resources when you create an instance. You can add user tags to the instance's boot volume by editing the boot volume. You can also add user tags when attaching a data volume to the instance. For more information, see [Adding user tags to Block Storage volumes](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-user-tags).

Snapshot and Backup for VPC
:   The [quota of snapshots and backup snapshots](/docs/vpc?topic=vpc-quotas#block-storage-quotas) you can create per volume has been increased to 750.

### 01 August 2022
{: #vpc-august0122}
{: release-note}

Sharing images across an enterprise account (beta)
:    You can now share or publish custom images to other accounts within your enterprise by using a private catalog. A private catalog provides a way for you to manage access to products for multiple accounts. You can use any existing x86 virtual server custom image with a private catalog, with the exception of an encrypted image. For more information, see [Getting started with Catalog Images on VPC](/docs/vpc?topic=vpc-getting-started-images-on-vpc-catalog).

## June 2022
{: #vpc-jun22}

### 30 June 2022
{: #vpc-jun0630}
{: release-note}

IBM Wazi as a Service (s390x processor architecture)
:   You can now create virtual server instances of IBM z/OS with IBM Wazi as a Service (Wazi aaS) image on IBM Z (s390x processor architecture) in IBM Cloud in the Tokyo (`jp-tok`), São Paulo (`br-sao`), Toronto (`ca-tor`), and London (`eu-gb`) regions. The option to select the Wazi aaS z/OS dev and test image is offered as an IBM Cloud allow-listed service. For more information, see [IBM Wazi as a Service product page](https://www.ibm.com/cloud/wazi-as-a-service){: external}.

File Storage for VPC
:   You can now access a customer root key (CRK) from one account, and then use that key to encrypt file shares you create in another account. When you create the file share, you specify the CRN of a root key from the account that contains the key. For more information, see [Cross-account encryption for multitenant file storage resources](/docs/vpc?topic=vpc-vpc-byok-cross-acct-key-file).

### 24 June 2022
{: #vpc-june2422}
{: release-note}

New stock image for bare metal servers
:   Ubuntu (20.04 and 18.04) is now supported as an image when you provision a bare metal server.

### 21 June 2022
{: #vpc-june2122}
{: release-note}

Backup for VPC (GA)
:    You can now create automated backup snapshots of your Block Storage volumes. If your original volume is compromised, you can restore it from a backup snapshot. You create a backup policy to control which source volumes are selected for backup by matching user tags in the volume with tags that are defined in the policy. Each policy contains up to four backup plans, which define how often backup snapshots are taken (daily, weekly, monthly, or more frequently by using a cron-spec) and retained (by date or by count). You can also view backup jobs, which show status of backup snapshots that are being created or deleted. For more information about this service, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

### 10 June 2022
{: #vpc-june0610}
{: release-note}

Block Storage for VPC
:    You can use the `volumes` API to restore an unattached data volume from a snapshot. Restoring from a snapshot creates a new, fully provisioned volume. The data volume that was created from the snapshot is fully hydrated (data is restored) when you later attach it to an instance. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

### 01 June 2022
{: #vpc-june0122}
{: release-note}

File Storage for VPC
:    For accounts authorized to preview this service, you can increase or decrease file share IOPS to meet your performance needs. Adjust IOPS within an IOPS tier profile or a custom profile. Or, adjust IOPS between profiles, for example, from a 3 IOPS/GB tiered profile to a custom profile. Adjusting IOPS within a profile or between profiles depends on the file share size. For more information, see [Adjusting file share IOPS](/docs/vpc?topic=vpc-adjusting-share-iops).

## May 2022
{: #vpc-may22}

### 27 May 2022
{: #vpc-may2722}
{: release-note}

Secrets Manager for application load balancers
:    Application load balancers now support [IBM Secrets Manager](/docs/secrets-manager?topic=secrets-manager-getting-started). With Secrets Manager, you can create, lease, and centrally manage secrets that are used in {{site.data.keyword.cloud}} services or your custom-built applications.

:    As a reminder, end of support for IBM Cloud Certificate Manager was 31 December 2022. Remaining instances of Certificate Manager have been deleted. If you have any user-provided Ingress secrets stored in Certificate Manager, they are no longer valid.

### 17 May 2022
{: #vpc-may1722}
{: release-note}

File Storage for VPC
:    For accounts authorized to preview this service, you can configure replication for new and existing file shares. Replication creates a read-only copy of your file share data in a different zone. You can fail over to the replica share if the source share becomes damaged or compromised. For more information, see [About file share replication](/docs/vpc?topic=vpc-file-storage-replication).

:    You can now add user tags from the UI, CLI, or API when you create a new file share or update file shares. For more information, see [Adding user tags](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-tags-shares-ui).


## April 2022
{: #vpc-apr22}

### 29 April 2022
{: #vpc-apr2922}
{: release-note}

Backup for VPC (select availability)
:    Accounts authorized to preview this service can create backup policies and plans to automatically back up Block Storage volumes. Backup policies control which source volumes are selected for backup by matching user tags in the volume with tags that are defined in the backup policy. Policies can contain up to four backup plans, which specify how often backup snapshots are taken (daily, weekly, monthly, or by using a `cron-spec`) and retained (by date or by count). You can also view backup jobs, which show status of backup snapshots that are being created or deleted. This release also provides new functionality for restoring volumes from backup snapshots. For more information about this service, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about) concepts.

### 28 April 2022
{: #vpc-apr2822}
{: release-note}

UI Updates
:   The following UI updates were made:
    - The custom image details information was moved from the side panel to a separate custom image details page.
    - Previously, when you changed the region or zone while you provision a virtual server instance, you had to click either Save or Cancel to continue. This behavior was changed and you don't need to make that selection when you change the region or zone.
    - You can now delete the virtual server instance without manually unbinding the floating IP address. When you delete the virtual server instance, the floating IP address is now automatically unbound before the virtual server instance is deleted. This UI change now matches how the virtual server instance is deleted when you use the CLI or API.

### 11 April 2022
{: #vpc-apr1122}
{: release-note}

2022-03-29 release of the VPC API introduced incompatible changes
:    The `2022-03-29` release of the VPC API necessitated incompatible changes in support of the reserved IPs feature:
     - IP addresses are now modeled as objects (resources), rather than strings.
     - Security groups must now be associated with targets rather than network interfaces.

:    Even if you are not planning to use reserved IPs, to avoid regressions in client functionality, be sure to reference [2022-03-29 API migration](/docs/vpc?topic=vpc-2022-03-29-migration) and follow the Action needed recommendations before you specify version `2022-03-29` or later. For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#version-2022-03-29) and [Reserved IP known issues](/docs/vpc?topic=vpc-known-issues).

### 7 April 2022
{: #vpc-apr0722}
{: release-note}

Backup for VPC (limited availability)
:    For accounts authorized to preview this service, you can create backup policies that automatically back up your Block Storage volumes. Backups are triggered when a user tag is applied to a volume matches a backup policy tag. Each backup policy has a backup plan, which defines how often backups are taken and how long they're retained. With this release, you can create and update up to four backup plans per policy. You can also track backup job status when a backup is created. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

## March 2022
{: #vpc-mar22}

### 31 March 2022
{: #vpc-mar3122}
{: release-note}

Add "Other" device types as Application Load Balancer (ALB) pool members
:   You can now add "Other" device types as ALB back-end pool members, such as servers contained within a Power Systems Virtual Server instance connected over IBM Cloud Direct Link. In the past, you were only able to select virtual server instances or Bare Metal servers within a VPC. When you create a new member, select the **Other** tab from the Attach server screen and enter the device IP address. For more information, see [Creating an IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancers&interface=ui).

### 30 March 2022
{: #vpc-mar3022}
{: release-note}

New stock image for bare metal servers
:   RHEL 8.4 is now supported as an image when you provision a bare metal server.

### 29 March 2022
{: #vpc-mar2922}
{: release-note}

UDP support for Network Load Balancers (NLB)
:   You can now set User Datagram Protocol (UDP) as the communication protocol for the {{site.data.keyword.nlb_full}} (NLB) listener and pool. UDP is a communications protocol for establishing low-latency and loss-tolerating connections between applications on the internet. It speeds up transmissions by enabling the transfer of data before an agreement is provided by the receiving party. For more information, see [Configuring the UDP protocol for network load balancers](/docs/vpc?topic=vpc-nlb-udp&interface=ui).

:   When you configure UDP and attaching a pool to your listener, you must configure the pool with the same protocol as the listener.
{: important}

Reserved IPs
:   You can now assign an IP address to your virtual server instance by specifying an already reserved IP for private IPs and bind them to network interfaces. For more information, see [Managing IP addresses](/docs/vpc?topic=vpc-managing-ip-addresses).

## 25 March 2022
{: #vpc-mar2522}
{: release-note}

LinuxONE (s390x processor architecture)
:   You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud in the Tokyo (`jp-tok`), São Paulo (`br-sao`), Toronto (`ca-tor`), and London (`eu-gb`) regions. For more information about available LinuxONE (s390x processor architecture) profiles, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

## 24 March 2022
{: #vpc-mar2422}
{: release-note}

New stock image for virtual servers
:   Windows&reg; 2022 is now supported as a stock image when you provision {{site.data.keyword.vpc_short}} virtual servers.

## 10 March 2022
{: #vpc-mar1022}
{: release-note}

New stock image for virtual servers
:   Debian 11 is now supported as a stock image when you provision {{site.data.keyword.vpc_short}} virtual servers.


## February 2022
{: #vpc-feb22}

### 24 February 2022
{: #vpc-feb2422}
{: release-note}

VPC Instance Metadata Service (GA)
:   The VPC Instance Metadata service is now generally available in all regions. This free service provides a REST API that you invoke within an instance to get information about that instance. Access to the API is unavailable from outside the instance. The service is disabled by default. Before you can access the metadata, you need to use the service to generate an instance identity access token for accessing the metadata service. You can optionally get an IAM token from this token to access all IAM-enabled services. For more information, see [About VPC Instance Metadata](/docs/vpc?topic=vpc-imd-about).

:   Virtual server instances created on LinuxONE (s390x processor architecture) are not enabled for the VPC Instance Metadata service. The metadata service is supported only on x86 systems.
{: note}

UI Update
:   A new Location cascading selector is available. For example, when you provision a virtual server, the Location section now has three new cascading selector boxes. You can now choose the geographic location, the metro (region) location, and the zone. The options available in these boxes are updated that are based on your selections. If you select “Americas” for the Geography, then the Metro box displays only the Metros available for the Americas.

### 22 February 2022
{: #vpc-feb2222}
{: release-note}

Host failure recovery policies
:    Host failure recovery policies for VPC are generally available (GA). For more information about host failure recovery policies, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=ui).

### 15 February 2022
{: #vpc-feb1522}
{: release-note}

LinuxONE (s390x processor architecture)
:   You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud in the Tokyo (`jp-tok`), London (`eu-gb`), and São Paulo (`br-sao`) regions. For more information about available LinuxONE (s390x processor architecture) profiles, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

Resizable boot volumes
:    You can now increase the capacity of a boot volume, up to 250 gigabytes (GB), when you create an instance from an image or instance template. You can also directly update an existing boot volume to increase its capacity. For more information, see [Increasing boot volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes).

Backup for VPC (closed beta)
:    For accounts authorized to preview this service, you can create backup policies that automatically backup your Block Storage volumes. Backups are triggered when a user tag is applied to a volume matches a backup policy tag. Each backup policy has a backup plan, which defines how often backups are taken and how long they're retained. For more information, see [About Backup for VPC (Beta)](/docs/vpc?topic=vpc-backup-service-about).

File Storage for VPC
:    The file service is now integrated with the Security and Compliance Center to help you manage security and compliance for your organization. For more information about this feature, see [Managing security and compliance](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-vpc-manage-security).

### 14 February 2022
{: #vpc-feb1422}
{: release-note}

Snapshots for VPC
:    The snapshots service is now integrated with the Security and Compliance Center to help you manage security and compliance for your organization. For more information about this feature, see [Managing security and compliance](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-manage-security).

### 11 February 2022
{: #vpc-feb1122}
{: release-note}

Maximum bandwidth for each vNIC is increased
:   The maximum bandwidth for a vNIC is now 25 Gbps instead of the previous 16 Gbps to 25 Gbps. For more information, see [Optimizing network bandwidth allocation for profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles#network-perf-notes-for-profiles).

### 10 February 2022
{: #vpc-feb1022}
{: release-note}

Block Storage attachments
:    Limits on the number of Block Storage data volumes you can attach to an instance are amended. Previously, instances that were created from smaller vCPU profiles attached only up to four data volumes. These limits are removed. For all instances, you can attach up to 12 Block Storage data volumes.

### 08 February 2022
{: #vpc-feb0822}
{: release-note}

Port ranges for public network load balancers
:    When [creating a public network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer&interface=api), you can now specify a range of listener ports.

Bare Metal server support for application load balancers
:    Bare Metal server members are now supported on application load balancers. For more information, see [Load balancers for VPC overview](/docs/vpc?topic=vpc-nlb-vs-elb&interface=ui).

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
:   You can now attach security groups to your endpoint gateways and manage them for your application needs. Attach security groups when you create an endpoint gateway, or modify security groups that are bound to an endpoint gateway after provisioning. For more information, see [Configuring ACLs and security groups for use with endpoint gateways](/docs/vpc?topic=vpc-configure-acls-sgs-endpoint-gateways).

Update to Snapshots for VPC
:   When you view details of a snapshot from the [UI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-snapshot-ui), [CLI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-details-cli), or [API](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=api#snapshots-vpc-view-api), a new field shows when the snapshot was captured from the volume. This feature applies to snapshots created after January 2022.

### 21 January 2022
{: #vpc-jan2122}
{: release-note}

New stock images for virtual servers
:   The following stock images are now available for virtual servers:
    - Red Hat 8.4 for SAP
    - SLES 12 SP5 for SAP
    - SLES 15 SP3 for SAP

### 14 January 2022
{: #vpc-jan1422}
{: release-note}

Application Load Balancer (ALB) for VPC
:   Application load balancers now support HTTP/HTTPS compression, which you use to compress data that is transmitted to your users. For more information, see [Compression (HTTP/HTTPS only)](/docs/vpc?topic=vpc-advanced-traffic-management#compression).

High Availability (HA) Virtual Network Function (VNF) support
:   Support for a highly available, highly resilient virtual network functions can be achieved by using the "routing mode" feature of the IBM Cloud Network Load Balancer (NLB) for VPC. For more information, see [About virtual network functions over VPC](/docs/vpc?topic=vpc-about-vnf) and [About HA VNF deployments](/docs/vpc?topic=vpc-about-vnf-ha).

Updates to Getting started with IBM Cloud VPC button
:   The "Getting started with IBM Cloud VPC" button now includes access to tours that are specific to what you are doing on the IBM console. If a tour is not available, the button takes you to the VPC List view.

### 06 January 2022
{: #vpc-jan0622}
{: release-note}

UI update when you create a virtual server
:   When you create a virtual server, the UI is updated to include a link in the Operating system section that opens a panel that contains information about the image lifecycle.

## December 2021
{: #vpc-dec21}

### 16 December 2021
{: #vpc-dec1621}
{: release-note}

File Storage for VPC (select availability)
:   {{site.data.keyword.filestorage_vpc_full}} is now available for customers with special approval to preview this service in the Washington DC (`us-east`), Dallas (`us-south`), Frankfurt (`eu-de`), London (`eu-gb`), Sydney (`au-syd`), and Tokyo (`jp-tok`) regions.

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
:   For all Windows&reg; stock images, TLS 1.2 was enabled and all earlier versions were disabled.

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

VPN client-to-site servers update (open beta)
:   You can now update a VPN server after the VPN is provisioned. For example, you can upgrade a stand-alone VPN server (one subnet) to a High Availability (HA) VPN server (two subnets in different zones). You can also detach a subnet to downgrade an HA VPN server to a stand-alone deployment, or change an attached subnet after your VPN server is provisioned. For more information, see [Upgrading to an HA VPN server](/docs/vpc?topic=vpc-vpn-client-to-site-change-server-types).

### 16 November 2021
{: #vpc-nov1621}
{: release-note}

New Very High Memory and Ultra High Memory instance profile family for dedicated host (select availability)
:   Very High Memory with instance storage and Ultra High Memory with instance storage profiles are now available for dedicated host.

:   Very High Memory profiles offer a core to RAM ratio of 1 vCPU to 14 GiB of RAM. This family is hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and is best for OLAP workloads and SAP-related services, such as SAP NetWeaver. Very High Memory profiles are available in the Dallas (`us-south`), Washington DC (`us-east`), Toronto (`ca-tor`), London (`eu-gb`), Frankfurt (`eu-de`), Tokyo (`jp-tok`), Osaka (`jp-osa`), and Sydney (`au-syd`) regions. For more information, see [Very High Memory with instance storage profiles](/docs/vpc?topic=vpc-dh-profiles#vhm-is-dh-pr). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

:   Ultra High Memory profiles offer a core to RAM ratio of 1 vCPU to 28 GiB of RAM. This family is hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and is optimized for running memory intensive applications and in-memory database such as SAP HANA, Memcached, or Redis. Ultra High Memory profiles are available in the Dallas (`us-south`) and Frankfurt (`eu-de`) regions. For more information, see [Ultra HIgh Memory with instance storage profiles](/docs/vpc?topic=vpc-dh-profiles#uhm-is-dh-pr). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 12 November 2021
{: #vpc-nov1221}
{: release-note}

Fedora Core OS
:   Fedora Core OS is now available in all regions as a stock image. The user login ‘root’ is disabled by default in Fedora Core OS. The user login ‘core’ can be used to log in to Fedora Core OS instances. For more information, see [Virtual server images](/docs/vpc?topic=vpc-about-images#x86-supported-os) and [User data examples for Fedora Core OS](/docs/vpc?topic=vpc-user-data#user-data-examples-for-fed-core).

### 02 November 2021
{: #vpc-nov0221}
{: release-note}

Instance Metadata Service for VPC (select availability)
:   The metadata service is now available in all regions to customer accounts authorized to access this service. This service is enabled by default when you create new instances by using the UI, CLI, or API. You can also disable the service from these interfaces. For more information, see [Enable or disable the metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-metadata-service-enable).

:   The metadata service also provides a new API call to generate an IAM token from the instance identity access token by using trusted profile information. For more information, see [Generate an IAM token from an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange).

Snapshots for VPC (GA)
:   You can now delete any or all snapshots not actively restoring a volume. Snapshots can be anywhere in the snapshot chain and must be in a `stable` state. For more information, see [Deleting snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-delete-snapshot-ui).

## October 2021
{: #vpc-oct21}

### 21 October 2021
{: #vpc-oct2121}
{: release-note}

UI Updates
:    When you create a virtual server instance by using the UI, the image selection on the UI was updated. The image selection is changed from a tile selection to a drop-down menu. For both custom and catalog images, a new side panel is available that you can use to filter, sort, and search for images.

### 15 October 2021
{: #vpc-oct1521}
{: release-note}

New GPU Instance Profile family
:   GPU profiles are available for all images. The profiles include attached NVIDIA 16 GB PCIe v100 GPUs. Drivers must be installed separately. You can use these instances to accelerate certain processing functions for workloads such as inferencing and machine learning. For more information about this service, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

### 05 October 2021
{: #vpc-oct0521}
{: release-note}

New regions for Very High Memory instance profile family (select availability)
:   Very High Memory profiles now are available in the London (`eu-gb`), Osaka (`jp-osa`), and Sydney (`au-syd`) regions. For more information, see [Very High Memory profiles](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

### 01 October 2021
{: #vpc-oct0121}
{: release-note}

File Storage for VPC (select availability)
:   {{site.data.keyword.filestorage_vpc_full}} is now available in the London (`eu-gb`) region. For more information about this service, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about).

## September 2021
{: #vpc-sep21}

### 23 September 2021
{: #vpc-sep2321}
{: release-note}

VNF routing mode for network load balancers
:   With {{site.data.keyword.vpc_short}}, you can provision Virtual Network Function (VNF) devices to gain better and more affordable scalability than you would by purchasing physical network devices. {{site.data.keyword.vpc_short}} network load balancers with routing mode that you use to send traffic to only VNF devices as back-end targets. In addition, their health checks ensure that workloads only travel through healthy VNF devices. For more information about this service, see [Creating a network load balancer with routing mode for VPC](/docs/vpc?topic=vpc-nlb-vnf).

### 14 September 2021
{: #vpc-sep1421}
{: release-note}

Terraform available for Placement Groups
:   You can now create and manage placement groups in your {{site.data.keyword.vpc_short}} by using Terraform. See [Managing placement groups](/docs/vpc?topic=vpc-managing-placement-group&interface=terraform).

### 07 September 2021
{: #vpc-sep0721}
{: release-note}

New Very High Memory instance profile family (select availability)
:   Very High Memory profiles are available in the Dallas (`us-south`), Washington DC (`us-east`), Toronto (`ca-tor`), Frankfurt (`eu-de`), and Tokyo (`jp-tok`) regions. Very High Memory profiles offer a core to RAM ratio of 1 vCPU to 14 GiB of RAM. This family is optimized for running high-compute-intensity in-memory workloads like SAP BW/4 HANA. For more information, see [Very High Memory profiles](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

Instance Bandwidth (select availability)
:   Instance bandwidth is now available in the Dallas (`us-south`), Washington DC (`us-east`), Toronto (`ca-tor`), Frankfurt (`eu-de`), Osaka (`jp-osa`), Brazil (Sao Paulo) regions. When you provision a virtual server instance, you can now allocate bandwidth between attached volumes and networking by using the API and CLI. You can adjust bandwidth after you provision a virtual server instance by using the UI, API, and CLI. The maximum bandwidth capacity is determined by the instance profile that you select during instance provisioning. For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

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
:   When you provision a virtual server instance and select {{site.data.keyword.redhat_full}} Enterprise Linux, be aware that locking a version must be done manually. When you log in to your operating system, a message is displayed with the command that you need to manually lock the operating system version. Performing a “yum update” without the version locking results in the operating system that is upgrading to the latest RHEL release, which is 8.4.

Block Storage for VPC
:   For Block Storage for VPC volumes that are attached to a virtual server instance, you can increase or decrease IOPS for a volume by specifying a different IOPS tier profile or different IOPS value withing a custom IOPS band. For more information, see [Adjusting IOPS for Block Storage volumes](/docs/vpc?topic=vpc-adjusting-volume-iops).

Client-to-site VPN servers (Beta)
:   Until now, the {{site.data.keyword.cloud_notm}} VPN for VPC service that is supported only site-to-site connectivity, which connects your on-premises network to the {{site.data.keyword.cloud_notm}} VPC network. This beta adds client-to-site connectivity, which allows users to connect to their {{site.data.keyword.cloud_notm}} VPC infrastructure through a secure or encrypted connection over the internet. This service is especially useful for individuals that are working at home, traveling, or at locations where site-to-site VPN might not be available. For more information, see [About client-to-site VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-overview).

### 24 August 2021
{: #vpc-aug2421}
{: release-note}

HTTPS redirect listeners
:   The [HTTPS redirect feature](/docs/vpc?topic=vpc-load-balancers-about&interface=ui#https-redirect-listener) is now available on Application Load Balancer for VPC to redirect traffic from an HTTP load balancer listener to an HTTPS listener. You can configure an HTTPS redirect on either [load balancer listeners](/docs/vpc?topic=vpc-load-balancers&interface=ui), [load balancer policies](/docs/vpc?topic=vpc-layer-7-load-balancing&interface=ui), or both.

Virtual server instances for VPC
:   When you provision an instance, you can now allocate I/O bandwidth between storage and network bandwidth from the total instance bandwidth. The maximum bandwidth capacity is determined by the instance profile that you selected during instance provisioning. For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

### 19 August 2021
{: #vpc-aug1921}
{: release-note}

Larger size boot volumes for custom images
:   You can import custom images with a boot disk size from 10 - 250 GB, which becomes the image's minimum provisioned size after you import. When you specify the image as part of [creating an instance](/docs/vpc?topic=vpc-creating-block-storage), the boot volume capacity is set to this size. For more information, see [Planning custom images](/docs/vpc?topic=vpc-planning-custom-images).

### 18 August 2021
{: #vpc-aug1821}
{: release-note}

File Storage for VPC
:   Custom IOPS profiles are available with capacity up to 16,000 GB. For more information, see [File Storage profiles](/docs/vpc?topic=vpc-file-storage-profiles).

:   You can also increase the file share size from its original capacity in GB increments up to 32,000 GB capacity, depending on your share profile. For more information, see [Expanding file share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity).

:   {{site.data.keyword.filestorage_vpc_full}} is available to customers with special approval to preview this service in the Washington DC (`us-east`), Dallas (`us-south`), and Frankfurt (`eu-de`) regions.

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
:   Instance groups now support by using an instance template that includes one or more data volumes. In addition, instance groups are now supported in private catalog. For more information about instance groups, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

### 9 August 2021
{: #vpc-aug0921}
{: release-note}

LinuxONE (s390x processor architecture)
:   You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud in the Tokyo (`jp-tok`) region. For more information about available LinuxONE (s390x processor architecture) profiles, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers). For more information about some of the limitations of LinuxONE (s390x processor architecture), see [Service limitations](/docs/vpc?topic=vpc-limitations).

## July 2021
{: #vpc-jul21}

### 26 July 2021
{: #vpc-jul2621}
{: release-note}

New São Paulo (`br-sao`) region
:   The São Paulo (`br-sao`) region endpoint is now in service at `https://br-sao.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

### 22 July 2021
{: #vpc-jul2221}
{: release-note}

Block Storage for VPC
:   IOPS tier and custom profiles are available with volume capacity up to 16,000 GB. For more information, see [Block Storage profiles](/docs/vpc?topic=vpc-block-storage-profiles).

:   For secondary volumes that are attached to a virtual server instance, you can increase capacity in GB increments up to 16,000 GB, depending on the volume's profile. The volume capacity is immediately increased. For more information, see [Expanding Block Storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

### 21 July 2021
{: #vpc-jul2121}
{: release-note}

Bare Metal Servers for VPC (closed beta)
:   If your account is granted special approval to preview this feature, you can now create bare metal servers to host VMware clusters in VPC. You can set up VMware management applications and create VMware virtual machines (VM) on the bare metal servers. As bare metal servers are integrated with the VPC platform, you can take advantage of the network, storage, and security capabilities provided by the VPC. For more information, see [About Bare Metal Servers for VPC (beta)](/docs/vpc?topic=vpc-about-bare-metal-servers).

:   The bare metal console feature is temporarily unavailable. An "unauthorized" error is returned if you try to connect to the console.

### 13 July 2021
{: #vpc-jul1321}
{: release-note}

Instance Identifier Update
:   The instance identifier (ID) now includes the SMBIOS system-uuid as a portion of the ID. The ID, including the SMBIOS system-uuid portion, is static and persists for the lifecycle of the virtual server instance until that virtual server instance is deleted. For more information, including how to retrieve this information from within your virtual server, see [Retrieving the virtual server instance identifier](/docs/vpc?topic=vpc-managing-virtual-server-instances#retrieve-VSI-instance-identifer) section in Managing virtual server instances.

### 6 July 2021
{: #vpc-jul0621}
{: release-note}

File Storage for VPC
:   The file storage general purpose 3 IOPS/GB profile is expanded so you can create file shares up to 32 TB with potential max IOPS performance of 96,000 IOPS. For more information, see [File Storage profiles](/docs/vpc?topic=vpc-file-storage-profiles). File Storage for VPC is available to customers with special approval to preview this service in the Washington DC (`us-east`), Dallas (`us-south`), and Frankfurt (`eu-de`) regions.

## June 2021
{: #vpc-jun21}

### 30 June 2021
{: #vpc-jun3021}
{: release-note}

Placement groups (Beta)
:   Placement groups for {{site.data.keyword.vpc_full}} are used to create placement group strategies for managing high availability workloads. A placement group contains virtual server instances that share a common placement strategy. Placement strategies influence the physical placement of select VPC resources to meet certain workload demands. For more information about placement groups, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc).

### 17 June 2021
{: #vpc-jun1721}
{: release-note}

UI enhancement
:   When you provision a VPC, you can now create multiple subnets from the default address prefix. When you create a VPC, one subnet is created in each zone of the VPC region. You can add up to a maximum of nine subnets.”

Application Load Balancer (ALB) support for cookie-based session persistence
:   ALBs now supports HTTP-cookie persistence and application-cookie persistence. For more information, see [Advanced traffic management](/docs/vpc?topic=vpc-advanced-traffic-management#session-persistence).

### 03 June 2021
{: #vpc-jun0321}
{: release-note}

UI enhancement
:   When you delete a virtual server, you must now confirm that you want to delete an instance by typing *delete*.

## May 2021
{: #vpc-may21}

### 25 May 2021
{: #vpc-may2521}
{: release-note}

Snapshots for VPC
:   Snapshots for VPC is a regional offering that lets you create a point-in-time copy of your Block Storage boot or data volume. Select a snapshot during instance provisioning and restore a new, fully-provisioned boot volume to start the instance. You can also create and attach a data volume from a snapshot within a running virtual server instance. Learn more [about creating and using snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) and explore the new [snapshots API methods](/apidocs/vpc#delete-snapshots).

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

### 15 May 2021
{: #vpc-may1521}
{: release-note}

Virtual Private Cloud (VPC) Gen 1 end of service
:   Virtual Private Cloud (VPC) Gen 1 reached end of service on 26 February 2021. The VPC on Classic API reference has been removed from the library page on 15 May, 2021. VPC on Classic users are being redirected to the [Virtual Private Cloud API](/apidocs/vpc).

### 06 May 2021
{: #vpc-may0621}
{: release-note}

New Ultra High Memory instance profile family (select availability)
:   Ultra High Memory profiles are hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and are optimized for running memory intensive applications and in-memory database such as SAP HANA, Memcached, or Redis. This profile family offers our highest vCPU to memory ratio with 28 GiB of memory for every 1 vCPU of compute and up to 5.7 TiB of available RAM. For more information, see [x86 Instance Profiles](/docs/vpc?topic=vpc-profiles).

:   The Ultra High Memory family of profiles is currently available in the Dallas (`us-south`) and Frankfurt (`eu-de`) multizone regions (MZRs). For more information about the Multizone regions, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations). Contact your IBM Sales representative if you need Ultra High Memory profiles in an MZR other than Dallas or Frankfurt.

## April 2021
{: #vpc-apr21}

### 30 April 2021
{: #vpc-apr3021}
{: release-note}

File Storage for VPC
:   File Storage for VPC is available to customers with special approval to preview this service in the Washington DC (`us-east`), Dallas (`us-south`), and Washington DC (`us-east`) regions. With this feature, you can create NFS-based file shares in a single zone in a region. You can share file storage over multiple virtual service instances within the same zone across multiple VPCs. For more information about this service, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about). Contact your IBM Sales representative if you are interested in getting access.

### 07 April 2021
{: #vpc-apr0721}
{: release-note}

File Storage for VPC (beta)
:   NFS-based file shares for a zone within a region are available in a limited, allow-listed beta. You can share file storage over multiple virtual service instances within the same zone across multiple VPCs. For more information about this service, see [About File Storage for VPC (beta)](/docs/vpc?topic=vpc-file-storage-vpc-about).

UI enhancement
:   When provisioning a new VPN gateway in a default VPC, the UI now populates the subnet table with the default subnet information.

### 06 April 2021
{: #vpc-apr0621}
{: release-note}

New Toronto (`ca-tor`) region
:   The Toronto (`ca-tor`) region endpoint is now in service at `https://ca-tor.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

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
:   {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) now supports the creation of private network load balancers. A private NLB is a load balancer only accessible from within the VPC network and/or where the client has reachability to the VPC network (for example, through Direct Link or a transit gateway). To create private load balancers, you must have a dedicated subnet with no custom routes configured for the subnet. For more information about creating a private NLB, refer to [Creating a network load balancer for VPC](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer).

### 19 March 2021
{: #vpc-mar1921}
{: release-note}

Virtual server instance console
:   The virtual server instance console feature is now generally available in the following regions: Dallas (`us-south`),Frankfurt (`eu-de`), London (`eu-gb`), Osaka (`jp-osa`), and Washington DC (`us-east`). For more information, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console).

Bring you own license (GA)
:   You can bring your own license (BYOL) when you import a RedHat Enterprise Linux (RHEL) or Windows&reg; custom image to IBM Cloud VPC. Because these images are registered and licensed by you, you maintain control over your licenses and with no additional cost. Acquisition and activation of the license is between you and and the OS vendor. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about).

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
:   Select beta users can now create snapshots of their boot and data volumes that are attached to a running virtual server instance. These snapshots can be used to create new volumes, which function like the original volume. For more information, see [About Snapshots for VPC (Beta)](/docs/vpc?topic=vpc-snapshots-vpc-about).

UI enhancement
:   When creating or editing a VPN policy (IKE or IPsec), a new authentication option is available, SHA512. Additionally, a new Diffie-Hellman key exchange group is supported: group 19.

## February 2021
{: #vpc-feb21}

### 25 February 2021
{: #vpc-feb2521}
{: release-note}

Dedicated hosts
:   You can now use dedicated hosts to carve out a single-tenant compute node for {{site.data.keyword.vpc_short}} in the following regions: Dallas (`us-south`), Washington DC (`us-east`), London (`eu-gb`), Tokyo (`jp-tok`), and Osaka (`jp-osa`). For more information, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances).

Application load balancer security group integration
:   For enhanced security, application load balancers can now be associated with security groups. You can specify one or more security groups when you create the application load balancer, and associate security groups with your existing application load balancers. For more information, see [Integrating an IBM Cloud Application Load Balancer for VPC with security groups](/docs/vpc?topic=vpc-alb-integration-with-security-groups).

UI enhancement
:   On the New virtual server for VPC page, selected SSH keys persist if you leave the page and then return to it during a single logged in session. If you log out or switch your account, the selected SSH keys change.

VPC address prefixes are no longer restricted to RFC-1918 addresses
:   You must now configure VPCs that use both non-RFC-1918 addresses and have public connectivity (floating IPs or public gateways) using a custom route that contains the new `Delegate-VPC` action. You must specify this action for destination CIDRs that are non-RFC-1918 compliant and also outside of the VPC, such as for destinations that are reachable through Direct Link, Transit Gateway, or VPC classic access. For more information about when to use the `Delegate-VPC` action, see [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana).

### 18 February 2021
{: #vpc-feb1821}
{: release-note}

UI enhancement
:   You can now provision a VPN gateway or load balancer without first creating a VPC. If you don't have an existing VPC you can provision the VPN gateway or load balancer in the default VPC. The new VPN gateway or load balancer is created along with a default VPC that includes a default security group, access control list, and subnet.

### 07 February 2021
{: #vpc-feb0721}
{: release-note}

Virtual server instance console (Beta)
:   You can now access your virtual server by connecting to a VNC or serial console. This feature provides a quick-and-easy way for you to interact with the virtual server without using a Secure Shell. This is a beta feature that requires special approval. Contact your IBM Sales representative if you are interested in getting access. For more information, see [Accessing virtual server instances by using VNC or serial consoles (Beta)](/docs/vpc?topic=vpc-vsi_is_connecting_console)

## January 2021
{: #vpc-jan21}

### 26 January 2021
{: #vpc-jan2621}
{: release-note}

New region
:   The Osaka (`jp-osa`) endpoint is now in service at `https://jp-osa.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region). See also [Endpoint URLs](/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API.

### 11 January 2021
{: #vpc-jan1121}
{: release-note}

Customer-managed encryption (GA)
:   Customer-managed encryption, which supports deleting or disabling customer root keys and provides new statuses, is now available in the Washington DC (WDC) region. All VPC service regions now support this feature.

## December 2020
{: #vpc-dec20}

### 22 December 2020
{: #vpc-dec2220}
{: release-note}

Customer-managed encryption (GA)
:   For Block Storage volumes and encrypted custom images, deleting or disabling a customer root key (CRK) is now managed by these VPC services. When you delete a root key, the resources become unusable for normal operations. A new `unusable` status and reason code `encryption_key_deleted` or `encryption_key_disabled` has been added to the API for `GET / volumes` and `GET / image` methods. These statuses also appear in the CLI and UI. For more information, see [Disabling root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys) and [Deleting root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys). For more information about key states and resource statuses, see [User actions that impact root key states and resource status](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-root-key-states).

:   The Washington DC (`us-east`) multi-zone region will be enabled in January 2021. This feature is available in all other multi-zone regions.


### 18 December 2020
{: #vpc-dec1820}
{: release-note}

Bring you own license (Beta)
:   You can now bring your own license (BYOL) when you import a custom image to {{site.data.keyword.vpc_short}}. This is a beta feature that is available for evaluation and testing purposes. Contact your IBM Sales representative if you are interested in getting access. For more information, see [Bring your own license (Beta)](/docs/vpc?topic=vpc-byol-vpc-about).

### 17 December 2020
{: #vpc-dec1720}
{: release-note}

Checksum (SHA256) for imported images (Beta)
:   Now when you import a custom image, you can view the checksum that's generated for the image when it is imported to {{site.data.keyword.vpc_short}}. If you generate a checksum locally for your image before importing it, you can compare the two checksums to ensure that they are identical. For more information, see [Validating a custom image after importing (Beta)](/docs/vpc?topic=vpc-importing-custom-images-vpc&interface=ui#validate-custom-images-cloud-object-storage-ui).

UI enhancements
:   Default boot volume names are now appended with a millisecond timestamp.

### 11 December 2020
{: #vpc-dec1120}
{: release-note}

VPN logging and auditing
:   The ability to monitor and audit your VPNs has been added to VPN for VPC. For more information, see [Using IBM Log Analysis to view VPN logs](/docs/vpc?topic=vpc-using-log-analysis-to-view-vpn-logs).

### 01 December 2020
{: #vpc-dec0120}
{: release-note}

New SDK
:   The [Node SDK](/apidocs/vpc?code=node) is now generally available.

## November 2020
{: #vpc-nov20}

### 20 November 2020
{: #vpc-nov2020}
{: release-note}

Support for ingress routing
:   Ingress routing is included as part of custom routing tables, released on 30 October 2020. Ingress routing enables you to customize routes on incoming traffic to a VPC from traffic sources external to the VPC's availability zone (IBM Cloud Direct Link, IBM Cloud Transit Gateway, or another availability zone in the same VPC). For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

Support for datapath log forwarding with Log Analysis
:   Datapath log forwarding with {{site.data.keyword.la_full_notm}} is now available for IBM Cloud Application Load Balancer for VPC. Data and health check logs are valuable for debugging, analysis, and maintenance purposes. With the datapath logging feature enabled, your load balancer forwards these logs to your account's {{site.data.keyword.la_full_notm}} dashboard. For more information, see [Datapath log forwarding with Log Analysis](/docs/vpc?topic=vpc-datapath-logging#datapath-logging).

UI enhancements
:   You can now provision a VPC from within the New virtual server for VPC page.

### 12 November 2020
{: #vpc-nov1220}
{: release-note}

Route-based virtual private network (VPN) gateways for VPC
:   Use route-based VPN for VPC gateways to manage large, on-premises or cloud-based networks that require the ability to statically route network and connectivity over a secure, encrypted connection. VPN for VPC provides UI, CLI, and API support in all MZRs. For more information, see [About VPN gateways](/docs/vpc?topic=vpc-using-vpn).

UI enhancements:
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
:   The [Java SDK](/apidocs/vpc?code=java) is now generally available.

### 04 September 2020
{: #vpc-sep0420}
{: release-note}

Network load balancer (GA)
:   The {{site.data.keyword.cloud_notm}} Network load balancer (NLB) for VPC is now generally available. Use the network load balancer to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [About IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-network-load-balancers).

### 01 September 2020
{: #vpc-sep0120}
{: release-note}

New region
:   The Sydney (`au-syd`) region endpoint is now in service at `https://au-syd.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region). See also [Endpoint URLs](/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API.

## August 2020
{: #vpc-aug20}

### 27 August 2020
{: #vpc-aug2720}
{: release-note}

Large profiles (GA):
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

Larger volume capacity for Block Storage for VPC (Beta)
:   Provision volumes with capacities greater than 2 TB and up to 16 TB, depending on the profile you selected. See [Expanded Capacity IOPS Tiers (Beta)](/docs/vpc?topic=vpc-block-storage-profiles#tiers).

Expanding Block Storage for VPC volume capacity
:   Expand the size of new Block Storage volumes that you create and attach to a virtual server instance on Gen 2 Compute resources. Indicate capacity in GB increments up to 16 TB, depending on your volume profile limits. For more information, see [Expanding Block Storage volume capacity (Beta)](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

Encrypted images (Beta)
:   Import an encrypted custom image with beta support for the feature. For more information, see [Creating an encrypted custom image (Beta)](/docs/vpc?topic=vpc-create-encrypted-custom-image).

New SDK
:   The [Python SDK](/apidocs/vpc?code=python) is now generally available.

### 23 July 2020
{: #vpc-jul2320}
{: release-note}

Customer-managed encryption for Block Storage for VPC (GA)
:   Customer-managed encryption is now available for protecting boot and data volumes on Gen 2 compute resources. Import your own root keys to the cloud or create a key using Key Protect or Hyper Protect Crypto Services. Then, use your key to secure your data. For more information, see [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption).

Flow logs for VPC (GA)
:   FLow logs for {{site.data.keyword.cloud_notm}} VPC are now generally available. This service enables the collection, storage, and presentation of information about IP traffic flowing to and from network interfaces within a VPC. For more information, see [About IBM Cloud Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs).

### 16 July 2020
{: #vpc-jul1620}
{: release-note}

New region
:   The Tokyo (`jp-tok`) region endpoint is now in service at `https://jp-tok.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

"Add to estimate" capability updates
:   Added support for itemized virtual server pricing in the Add to estimate capability.

### 03 July 2020
{: #vpc-jul0320}
{: release-note}

Network load balancer (Beta)
:   Network load balancers are available in a limited beta. Use network load balancers to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [Load balancers overview](/docs/vpc?topic=vpc-nlb-vs-elb) and [About network load balancers](/docs/vpc?topic=vpc-network-load-balancers).

VPN for VPC update
:   Access VPN monitoring metrics by using [Monitoring VPN for VPC metrics](/docs/vpc?topic=vpc-vpn-monitoring-metrics&interface=ui).

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

UI Enhancements
:   Added pagination for the security groups list table

:   On the VPN Provision page, select a subnet from a table or create subnets

:   New "upgrade pending" button if a user account is pending upgrade

New SDK
:   The [Go SDK](/apidocs/vpc?code=go) is now generally available.

### 02 June 2020
{: #vpc-jun0220}
{: release-note}

IBM Cloud Virtual Servers for VPC on POWER service is deprecated
:   As of 02 June 2020, you cannot provision new instances. Any instance that is still provisioned as of 22 August 2020 will be deleted. For more information, see the [End of Service Announcement for Virtual Servers for VPC on POWER](https://www.ibm.com/blog/announcement/end-of-service-announcement-for-virtual-servers-for-vpc-on-power/).

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

Load balancer for VPC update
:   Back-end encryption (HTTPS for back-end pool) is now supported. See [Using load balancers for VPC](/docs/vpc?topic=vpc-load-balancers) for details.

### 1 May 2020
{: #vpc-may0120}
{: release-note}

Customer-managed encryption (Beta)
:   Customer-managed encryption (BYOK) is available in beta. Bring your own encryption keys and store the keys in Key Protect or Hyper Protect Crypto Services within VPC. Use BYOK to encrypt your VPC-enabled Block Storage at the volume level by using your own keys. For more information, see [Creating virtual server instances with customer-managed encryption (Beta)](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

Flow logs (Beta)
:   Flow logs are now available in beta. Use flow logs to collect, store, and present the Internet Protocol (IP) traffic flowing to and from networks within your VPC. For more information, see [About flow logs (Beta)](/docs/vpc?topic=vpc-flow-logs).

UI enhancement
:   Create a new subnet directly from the page where you create a new instance.

## April 2020
{: #vpc-apr20}

### 22 April 2020
{: #vpc-apr2220}
{: release-note}

New Frankfurt (`eu-de`) region
:   The Frankfurt (`eu-de`) region endpoint (eu-de) is now in service at `http://eu-de.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

### 17 April 2020
{: #vpc-apr1720}
{: release-note}

Documentation correction
:   {{site.data.keyword.cloud_notm}} interprets volume capacity units in gibibytes, but the API documentation used gigabytes. This issue is now resolved in the documentation.

### 03 April 2020
{: #vpc-apr0320}
{: release-note}

Load balancer for VPC update
:   Access load balancer monitoring metrics (throughput, active connections, connection rate) using [IBM Cloud Monitoring](/docs/cloud-infrastructure?topic=cloud-infrastructure-monitoring-iaas).

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
:   Monitor virtual server instances using {{site.data.keyword.mon_full_notm}}. Use the new **Add monitoring** button on the instance's **Monitoring** page to provision an instance of the monitoring service. If a monitoring instance is already provisioned for the region, use the **Launch monitoring** button to view metrics associated with the instance. For more information, see [Monitoring metrics](/docs/vpc?topic=vpc-vpn-monitoring-metrics&interface=ui).

Updated styling
:   VPC pages in {{site.data.keyword.IBM_notm}} console now use [Carbon 10](https://carbondesignsystem.com/){: external}, the IBM open source design system, which improves consistency and quality.

### 09 March 2020
{: #vpc-mar0920}
{: release-note}

IBM virtual servers for VPC on POWER
:   POWER-based instances are now generally available. For more information, see the following resource:

   - [Profiles](/docs/vpc?topic=vpc-profiles)

### 02 March 2020
{: #vpc-mar0220}
{: release-note}

New London (`eu-gb`) region
:   The London (`eu-gb`) region endpoint (eu-gb) is now in service at `http://eu-gb.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

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
:   Provision an instance that uses an RHEL image or a Windows&reg; image in the Dallas (`us-south`) region. For more information, see [Images](/docs/vpc?topic=vpc-about-images).

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

   * [Profiles](/docs/vpc?topic=vpc-profiles)

Red Hat Enterprise Linux (RHEL) and Windows stock images are now available
:   Provision an instance that uses an RHEL image or a Windows&reg; image in the Washington DC (`us-east`) region. For more information, see [Images](/docs/vpc?topic=vpc-about-images).

UI enhancements
:   The modals for provisioning and attaching a public gateway and for creating an SSH key are now replaced with a redesigned side pane.

## January 2020
{: #vpc-jan20}

### 10 January 2020
{: #vpc-jan1020}
{: release-note}

New Washington DC (`us-east`) region
:   The Washington DC (`us-east`) region endpoint (us-east) is now in service at `http://us-east.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

CLI plug-in release 0.5.10
:   For more information, see [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli).
    * You now have an Example section in command help for creating and updating commands. Example: `ibmcloud is help instance-create`, `ibmcloud is help instance-update`, or `ibmcloud is help volume-create`.
    * Use the *resource group filter* in list commands. Example: `ibmcloud is vpcs --resource-group-name Littleton`.
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
:   Target a specific resource group by using the '[-g YOUR_GROUP]' command option. If you specify the target resource group by using the `ibmcloud target [-g YOUR_GROUP]`command, the output displays only VPC resources inside of the specified resource group. This update also introduces enhancements to some of the commands for `get` and `list`, showing more detail for your VPC, instances, and instance profiles. For more information, see [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli).

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

VPC layout
:   View resources that are associated with a VPC in the {{site.data.keyword.cloud_notm}} console. For more information, see [Viewing resources associated with a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#vpc-layout).

### 07 November 2019
{: #vpc-nov0719}
{: release-note}

Load balancer (Beta)
:   Use the {{site.data.keyword.cloud}} Load balancer for VPC service to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [Using load balancers (Beta)](/docs/vpc?topic=vpc-load-balancers).

VPN (Beta)
:   Use the {{site.data.keyword.cloud}} VPN for VPC service to securely connect your VPC to another private network. For more information, see [Using VPN (Beta)](/docs/vpc?topic=vpc-using-vpn).

## October 2019
{: #vpc-oct19}

### 31 October 2019
{: #vpc-oct3119}
{: release-note}

Classic access
:   Set up access from a VPC to your {{site.data.keyword.cloud}} Classic infrastructure, including Direct Link connectivity. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

Advanced routing
:   Control the flow of network traffic in your VPC by configuring VPC routes. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

## June 2019
{: #vpc-june19}

### 04 June 2019
{: #vpc-jun0419}
{: release-note}

Introducing {{site.data.keyword.vpc_short}}
:   {{site.data.keyword.vpc_full}} is now generally available in the Dallas (`us-south`), Frankfurt (`eu-de`), and Tokyo (`jp-tok`) regions. For more information, see [{{site.data.keyword.vpc_short}}](https://www.ibm.com/blog/introducing-ibm-cloud-virtual-private-cloud/).{: external}
