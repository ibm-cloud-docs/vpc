---

copyright:
  years: 2019, 2021

lastupdated: "2021-08-17"

keywords: release notes, changes, updates

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:external: target="_blank" .external}

# Release notes
{: #release-notes}

Use the release notes to learn about new and changed {{site.data.keyword.vpc_full}} features.
{:shortdesc}

## 17 August 2021
{: #august-17-2021}

**Instance Identifier Update:** The instance identifier (ID) now includes the SMBIOS system-uuid as a portion of the ID. The ID, including the SMBIOS system-uuid portion, is static and persists for the lifecycle of the virtual server instance until that virtual server instance is deleted. For more information, including how to retrieve this information from within your virtual server, see [Retrieving the virtual server instance identifier](/docs/vpc?topic=vpc-managing-virtual-server-instances#retrieve-VSI-instance-identifer) section in Managing virtual server instances.

**Placement groups (GA):** Placement groups for {{site.data.keyword.vpc_full}} are used to create placement group strategies for managing high availability workloads. A placement group contains virtual server instances that share a common placement strategy. Placement strategies influence the physical placement of select VPC resources to meet certain workload demands. For more information about placement groups, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc).

## 11 August 2021
 {: #august-11-2021}

 **Auto scale supports data volumes and private catalog:** Instance groups now support using an instance template that includes one ore more data volumes. In addition, instance groups are now supported in private catalog. For more information about instance groups, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

## 9 August 2021

{: #august-9-2021}

**LinuxONE (s390x processor architecture):** You can now create virtual server instances on LinuxONE (s390x processor architecture) in IBM Cloud™. For more information about available LinuxONE (s390x processor architecture) profiles, see [Instance Profiles](/docs/vpc?topic=vpc-profiles). To create instances on LinuxONE (s390x processor architecture), see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).
For more information about some of the limitations of LinuxONE (s390x processor architecture), see [Service limitations](/docs/vpc?topic=vpc-limitations).

## 30 July 2021

{: #july-30-2021}

**Instance Metadata for VPC (closed Beta):** If your account is granted special approval to preview this feature, you can access metadata about your VPC compute resources. The metadata service is a REST API that you invoke using a well-known URI to retrieve instance-specific information from the metadata server. For more information, see [About Instance Metadata for VPC (Beta)](/docs/vpc?topic=vpc-imd-about).

## 26 July 2021
{: #july-26-2021}

**New region:** The São Paulo region endpoint is now in service at `https://br-sao.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 22 July 2021
{: #july-22-2021}

**Block Storage for VPC:** IOPS tier and custom profiles are available with volume capacity up to 16,000 GB. For information, see [Block storage profiles](/docs/vpc?topic=vpc-block-storage-profiles).

For secondary volumes attached to a virtual server instance, you can increase capacity in GB increments up to 16,000 GB, depending on the volume's profile. The volume capacity is immediately increased. For more information, see  [Expanding block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

## 21 July 2021
{: #july-21-2021}

**Bare Metal Servers for VPC (closed beta):** If your account has been granted special approval to preview this feature, you can now create bare metal servers to host VMware clusters in VPC. You can set up VMware management applications and create VMware virtual machines (VM) on the bare metal servers. As bare metal servers are integrated with the VPC platform, you can take advantage of the network, storage, and security capabilities provided by the VPC. For more information, see [About Bare Metal Servers for VPC (beta)](/docs/vpc?topic=vpc-about-bare-metal-servers).

The bare metal console feature is temporarily unavailable. An "unauthorized" error will be returned if you try to connect to the console.
{:note}

## 6 July 2021
{: #july-6-2021}

**File storage for VPC**: The file storage general purpose 3 IOPS/GB profile is expanded to allow you to create file shares up to 32 TB with potential max IOPS performance of 96,000 IOPS. For more information, see [File storage profiles](/docs/vpc?topic=vpc-file-storage-profiles). File Storage for VPC is available to customers with special approval to preview this service in the Washington, Dallas, and Frankfurt regions.

## 30 June 2021
{: #june-30-2021}

**Placement groups:** Placement groups for {{site.data.keyword.vpc_full}} are used to create placement group strategies for managing high availability workloads. A placement group contains virtual server instances that share a common placement strategy. Placement strategies influence the physical placement of select VPC resources to meet certain workload demands. For more information about placement groups, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc).

## 17 June 2021
{: #june-17-2021}

**UI enhancement:** When provisioning a VPC, you can now create multiple subnets from the default address prefix. When you create a VPC, one subnet is created in each zone of the VPC region. You can add up to a maximum of 9 subnets.”

**Application Load Balancer (ALB) support for cookie-based session persistence:** ALBs now supports HTTP-cookie persistence and application-cookie persistence. For more information, see [Advanced traffic management](/docs/vpc?topic=vpc-advanced-traffic-management#session-persistence).

## 03 June 2021
{: #june-03-2021}

**UI enhancement:** When deleting a virtual server, you must now confirm that you want to delete an instance by typing *delete*.

## 25 May 2021
{: #may-25-2021}

**Snapshots for VPC:** - Snapshots for VPC is a regional offering that lets you create a point-in-time copy of your block storage boot or data volume. Select a snapshot during instance provisioning and restore a new, fully-provisioned boot volume to start the instance. You can also create and attach a data volume from a snapshot within a running virtual server instance.

Learn more [about creating and using snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) and explore the new [snapshots API methods](/apidocs/vpc#delete-snapshots).

## 21 May 2021
{: #may-21-2021}

**Image from volume:** You can create a custom image from a boot volume on a virtual server instance. The image is a full copy of the source volume, including the operating system and any user data. You can create new virtual server instances from the image created from the volume. For more information, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc).

## 13 May 2021
{: #may-13-2021}

**Scheduled Scaling for VPC (GA):** - Scheduled Scaling for VPC lets you set scheduled actions to automatically scale instance group capacity up or down based on daily, intermittent, or seasonal demand. You can set multiple, recurring scheduled action frequencies that can scale monthly, weekly, daily, hourly, or even every set number of minutes. For more information, see [Scheduled scaling](/docs/vpc?topic=vpc-scheduled-scaling-vpc).

## 06 May 2021
{: #may-06-2021}

**New Ultra High Memory instance profile family (LA):** Ultra High Memory profiles are hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and are optimized for running memory intensive applications and in-memory database such as SAP HANA, Memcached, or Redis. This profile family offers our highest vCPU to memory ratio with 28 GiB of memory for every 1 vCPU of compute and up to 5.7 TiB of available RAM. For more information, see [Instance Profiles](/docs/vpc?topic=vpc-profiles).

The Ultra High Memory family of profiles is currently available in the Dallas and Frankfurt multizone regions(MZRs). For more information, see the Multizone regions section in [Locations for resource deployment](https://cloud.ibm.com/docs/overview?topic=overview-locations#mzr-table). Contact your IBM Sales representative if you need Ultra High Memory profiles in a MZR other than Dallas or Frankfurt.
{: note}

## 30 April 2021
{: #april-30-2021}

**File Storage for VPC:** File Storage for VPC is available to customers with special approval to preview this service in the Washington, Dallas, and Frankfurt regions. With this feature, you can create NFS-based file shares in a single zone in a region. You can share file storage over multiple virtual service instances within the same zone across multiple VPCs. For more information about this service, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about). Contact your IBM Sales representative if you are interested in getting access.

## 07 April 2021
{: #april-07-2021}

- **File Storage for VPC (beta):** NFS-based file shares for a zone within a region are available in a limited, allow-listed beta. You can share file storage over multiple virtual service instances within the same zone across multiple VPCs. For more information about this service, see [About File Storage for VPC (beta)](/docs/vpc?topic=vpc-file-storage-vpc-about).
- **UI enhancement:** When provisioning a new VPN gateway in a default VPC, the UI now populates the subnet table with the default subnet information.

## 06 April 2021
{: #april-06-2021}

**New region:** The Toronto region endpoint is now in service at `https://ca-tor.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 01 April 2021
{: #april-01-2021}

- **Instance storage:** You can now provision a virtual server instance with instance storage, a set of one or more solid state drives directly attached to your instance. An instance storage disk provides fast, affordable, temporary storage to improve the performance of cloud native workloads with scratch space, caching buffers, or a place for replicated data. To provision a virtual server instance with instance storage, you must select an instance storage [profile](/docs/vpc?topic=vpc-profiles). For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

- **Dedicated hosts support instance storage:** You can now provision a dedicated host with an instance storage profile, providing the capability to host virtual server instances that include instance storage. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles).

- **Instance resize (GA):** The ability to resize a virtual server instance by selecting a different profile to assign to the instance is now generally available. Profiles are a combination of instance attributes, such as the number of vCPUs, amount of RAM, network bandwidth, and more that define the size and capabilities of the virtual server instance. For more information, see [Resizing an instance](/docs/vpc?topic=vpc-resizing-an-instance).

## 31 March 2021
{: #march-31-2021}

**Virtual server instance console**: The virtual server instance console feature is now generally available in all regions. For more information, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console).

## 25 March 2021
{: #march-25-2021}

- **New parameter-based rule types for application load balancers:** When creating policy rules for {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB), the `field` property may now be set to `query` or `body` to perform additional forms of layer 7 load balancing.

	* `query` - Write layer 7 rules that use the query string to route traffic to a specific target.

	* `body` - If the body of the `POST` request uses form encoding (UTF-8), then you can create layer 7 rules to route traffic based on the parameter name and value in the body.

  For more information, refer to [Layer 7 load balancing](/docs/vpc?topic=vpc-layer-7-load-balancing).
- **TCP keep alive support for application load balancers:** {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) now supports `TCP keep alive`. With this setting, the ALB sends TCP-keep-alive packets to both client and back-end servers every five seconds.

  For more information, refer to [Advanced traffic management](/docs/vpc?topic=vpc-advanced-traffic-management#tcp-keep-alive).
- **New monitoring metrics:** {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) now has additional monitoring metrics to help track the traffic and usage patterns for your load balancers. These new metrics can provide insight about peak traffic hours, usage dropoffs, and overall usage patterns. The new metrics are:

	* Total number of HTTP/HTTPS requests received by the back-end
	* Average response time for HTTP/HTTPS requests
	* Count of responses with the HTTP 2xx back-end response code
	* Count of responses with the HTTP 3xx back-end response code
	* Count of responses with the HTTP 4xx back-end response code
	* Count of responses with the HTTP 5xx back-end response code

  For more information, refer to [Monitoring IBM Cloud Application Load Balancer metrics](/docs/vpc?topic=vpc-monitoring-metrics-alb).
- **Private network load balancers:** {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) now supports the creation of private network load balancers. A private NLB is a load balancer only accessible from within the VPC network and/or where the client has reachability to the VPC network (for example, through Direct Link or a transit gateway).

  To create private load balancers, you must have a dedicated subnet with no custom routes configured for the subnet.

  For information on creating a private NLB, refer to [Creating a Network Load Balancer for VPC](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer).

## 19 March 2021
{: #march-19-2021}

- **Virtual server instance console**: The virtual server instance console feature is now generally available in the following regions: Dallas, Frankfurt, London, Osaka, and Washington DC. For more information, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console).
- **Bring you own license (GA)**: You can bring your own license (BYOL) when you import a RedHat Enterprise Linux (RHEL) or Windows custom image to IBM Cloud VPC. Because these images are registered and licensed by you, you maintain control over your licenses and with no additional cost. Acquisition and activation of the license is between you and and the OS vendor. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about).
- **Dedicated hosts (GA)**: Dedicated hosts are available in all regions. Use a dedicated host to carve out a single-tenant compute node for {{site.data.keyword.vpc_short}}. For more information, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)

## 12 March 2021
{: #march-12-2021}

- **New profiles:** The Balanced, Compute, and Memory profile families now include three new profiles with 64, 96, and 128 vCPUs. Profiles with 64 or more vCPUs are deployed exclusively on 2nd generation Intel Xeon Platinum 8260 (Cascade Lake) running at a base speed of 2.4 GHz and an all-core turbo frequency of 3.1 GHz.

- **VPN for VPC:**
  * For high-security standards, VPN now supports SHA-512 authentication and Diffie-Hellman Group 19.
  * Gateway private IP addresses are now visible for gateway members.

## 05 March 2021
{: #march-05-2021}

- **Instance resize (Beta):** Beta users can now resize a virtual server instance by selecting a different profile to assign to the instance. Profiles are a combination of instance attributes, such as the number of vCPUs, amount of RAM, network bandwidth, and more that define the size and capablilities of the virtual server instance. For more information, see [Resizing an instance (Beta)](/docs/vpc?topic=vpc-resizing-an-instance).
- **Snapshots for VPC (Beta)** - Select Beta users can now create snapshots of their block storage boot and data volumes attached to a running virtual server instance. These snapshots can be used to create new volumes, which function like the original volume. For more information, see [About Snapshots for VPC (Beta)](/docs/vpc?topic=vpc-snapshots-vpc-about).
- **UI enhancement:** When creating or editing a VPN policy (IKE or IPsec), a new authentication option is available, SHA512. Additionally, a new Diffie-Hellman key exchange group is supported: group 19.


## 25 February 2021
{: #feb-25-2021}

- **Dedicated hosts:** You can now use dedicated hosts to carve out a single-tenant compute node for {{site.data.keyword.vpc_short}} in the following regions: Dallas, London, Tokyo, and Osaka. For more information, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)
- **Application load balancer security group integration:** For enhanced security, application load balancers can now be associated with security groups. You can specify one or more security groups when you create the application load balancer, and associate security groups with your existing application load balancers. For more information, see [Integrating an IBM Cloud Application Load Balancer for VPC with security groups](/docs/vpc?topic=vpc-alb-integration-with-security-groups).
- **UI enhancement:** On the New virtual server for VPC page, selected SSH keys persist if you leave the page and then return to it during a single logged in session. If you log out or switch your account, the selected SSH keys change.
- **VPC address prefixes are no longer restricted to RFC-1918 addresses:** You must now configure VPCs that use both non-RFC-1918 addresses and have public connectivity (floating IPs or public gateways) using a custom route that contains the new `Delegate-VPC` action. You must specify this action for destination CIDRs that are non-RFC-1918 compliant and also outside of the VPC, such as for destinations that are reachable through Direct Link (2.0), Transit Gateway, or VPC classic access. For more information about when to use the `Delegate-VPC` action, see [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana).

## 18 February 2021
{: #feb-18-2021}

**UI enhancement:** You can now provision a VPN gateway or load balancer without first creating a VPC. If you don't have an existing VPC you can provision the VPN gateway or load balancer in the default VPC. The new VPN gateway or load balancer is created along with a default VPC that includes a default security group, access control list, and subnet.

## 7 February 2021
{: #feb-07-2021}

**Virtual server instance console (Beta)**: You can now access your virtual server by connecting to a VNC or serial console. This feature provides a quick-and-easy way for you to interact with the virtual server without using a Secure Shell. This is a Beta feature that requires special approval. Contact your IBM Sales representative if you are interested in getting access. For more information, see [Accessing virtual server instances by using VNC or serial consoles (Beta)](/docs/vpc?topic=vpc-vsi_is_connecting_console)

## 26 January 2021
{: #jan-26-2021}

**New region:** The Osaka region endpoint is now in service at `https://jp-osa.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region). See also [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API.

## 11 January 2021
{: #jan-11-2021}

**Customer-managed encryption (GA)**, which supports deleting or disabling customer root keys and provides new statuses, is now available in the Washington DC (WDC) region. All VPC service regions now support this feature.

## 22 December 2020
{: #dec-22-2020}

**Customer-managed encryption (GA):** For block storage volumes and encrypted custom images, deleting or disabling a customer root key (CRK) is now managed by these VPC services. When you delete a root key, the resources become unusable for normal operations. A new `unusable` status and reason code `encryption_key_deleted` or `encryption_key_disabled` has been added to the API for `GET / volumes` and `GET / image` methods. These statuses also appear in the CLI and UI. For more information, see [Disabling root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys) and [Deleting root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys). For information on key states and resource statuses, see [User actions that impact root key states and resource status](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-root-key-states).

The Washington D.C. multi-zone region will be enabled in January 2021. This feature is available in all other multi-zone regions.
{:note}

## 18 December 2020
{: #dec-18-2020}

**Bring you own license (Beta):** You can now bring your own license (BYOL) when you import a custom image to {{site.data.keyword.vpc_short}}. This is a Beta feature that is available for evaluation and testing purposes. Contact your IBM Sales representative if you are interested in getting access. For more information, see [Bring your own license (Beta)](/docs/vpc?topic=vpc-byol-vpc-about).

## 17 December 2020
{: #dec-17-2020}

- **Checksum (SHA256) for imported images (Beta):** Now when you import a custom image, you can view the checksum that's generated for the image when it is imported to {{site.data.keyword.vpc_short}}. If you generate a checksum locally for your image before importing it, you can compare the two checksums to ensure that they are identical. For more information, see [Validating a custom image after importing (Beta)](/docs/vpc?topic=vpc-managing-images#validate-custom).
- **UI enhancements:** Default boot volume names are now appended with a millisecond timestamp.

## 11 December 2020
{: #dec-11-2020}

**VPN logging and auditing:** The ability to monitor and audit your VPNs has been added to {{site.data.keyword.vpn_vpc_short}}. For more information, see [Using IBM Log Analysis to view VPN logs](/docs/vpc?topic=vpc-using-log-analysis-to-view-vpn-logs).

## 1 December 2020
{: #december-1-2020}

**New SDK:** The [Node SDK](https://{DomainName}/apidocs/vpc?code=node) is now generally available.

## 20 November 2020
{: #november-20-2020}

- **Support for ingress routing:** Ingress routing is included as part of custom routing tables, released on 30 October 2020. Ingress routing enables you to customize routes on incoming traffic to a VPC from traffic sources external to the VPC's availability zone (IBM Cloud Direct Link 2.0, IBM Cloud Transit Gateway, or another availability zone in the same VPC). For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
- **Support for datapath log forwarding with Log Analysis:** Datapath log forwarding with {{site.data.keyword.la_full_notm}} is now available for IBM Cloud Application Load Balancer for VPC. Data and health check logs are valuable for debugging, analysis, and maintenance purposes. With the datapath logging feature enabled, your load balancer forwards these logs to your account's {{site.data.keyword.la_full_notm}} dashboard. For more information, see [Datapath log forwarding with Log Analysis](/docs/vpc?topic=vpc-datapath-logging#datapath-logging).
- **UI enhancements:** You can now provision a VPC from the within the New virtual server for VPC page.

## 12 November 2020
{: #november-12-2020}

- **Route-based virtual private network (VPN) gateways for VPC:** Use route-based {{site.data.keyword.vpn_vpc_short}} gateways to manage large, on-premises or cloud-based networks that require the ability to statically route network and connectivity over a secure, encrypted connection. {{site.data.keyword.vpn_vpc_short}} provides UI, CLI, and API support in all MZRs. For more information, see [About VPN gateways](/docs/vpc?topic=vpc-using-vpn).

- **UI enhancements:**
   - You can now provision virtual server instances without first creating a VPC. Your virtual server instance is created in a default VPC.
   - The refresh data option is removed from all details pages. Instead, users can refresh by using the browser refresh.


## 6 November 2020
{: #november-6-2020}

**Proxy protocol support for IBM Cloud Application Load Balancer for VPC (ALB):** Proxy protocol provides a way to carry client information across proxies by appending it to the traffic your load balancer sends to the back-end servers. It also allows you to use listeners if incoming traffic contains proxy protocol information set by an intermediate proxy in between the client and your load balancer. See [Enabling proxy protocol](/docs/vpc?topic=vpc-advanced-traffic-management#proxy-protocol-enablement) for details.

## 2 November 2020
{: #november-2-2020}

- **Custom routing tables and routes (GA):** {{site.data.keyword.cloud_notm}} Custom Routes for VPC enables you to configure and control the routing behavior of traffic within your VPC. This functionality provides the ability to explicitly manage traffic between and among subnets within a VPC; thereby, supporting network topologies that incorporate the use of virtual appliances, such as firewalls. This service also allows {{site.data.keyword.cloud_notm}} to continue to develop and deliver advanced features and capabilities for our {{site.data.keyword.cloud_notm}} services. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

- **IP spoofing (GA)** allows users to enable or disable the IP spoofing check on virtual server instances. This facilitates the traffic flow configured with custom routes functionality. For more information, see [About IP spoofing](/docs/vpc?topic=vpc-ip-spoofing-about).

## 30 October 2020
{: #october-30-2020}

**Virtual private endpoint gateways (GA)**: {{site.data.keyword.cloud_notm}} Virtual Private Endpoints (VPE) for VPC is now generally available. This service enables you to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, allocated from a subnet within your VPC. For more information, see [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

## 1 October 2020
{: #october-01-2020}

- **Encrypted images (GA):** Support for encrypting and importing a custom image is now generally available. For more information, see [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image).

- **UI enhancements:**
   - Improved name validation when an existing resource name is specified for a new resource
   - New way to view and select instance profiles in a side window when you provision an instance
   - Enhanced security group details page to include attached network interfaces, with a link to manage the interfaces

## 24 September 2020
{: #september-24-2020}

**New SDK:** The [Java SDK](https://{DomainName}/apidocs/vpc?code=java) is now generally available.

## 4 September 2020
{: #september-04-2020}

**Network load balancer (GA):** The {{site.data.keyword.cloud_notm}} Network load balancer (NLB) for VPC is now generally available. Use the network load balancer to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [About IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-network-load-balancers).

## 1 September 2020
{: #september-01-2020}

**New region:** The Sydney region endpoint is now in service at `https://au-syd.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region). See also [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API.

## 27 August 2020
{: #august-27-2020}

- **Large profiles (GA):** The following two 48-core profiles are now generally available:
    * cx2-48x96
    * mx2-48x384

  For more information, see [Profiles](/docs/vpc?topic=vpc-profiles).

- **UI enhancements:**
    - New design for the VPC Overview page and generation switcher
    - Updated load balancer page to include auto scale enablement

## 24 August 2020
{: #august-24-2020}

**Auto scale (GA):** Auto scale is now generally available. Use auto scale to improve performance and costs by dynamically creating virtual server instances to meet the demands of your environment. For more information, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

## 12 August 2020
{: #august-12-2020}

**New image support:** {{site.data.keyword.redhat_full}} Enterprise Linux 8 is now available in all regions.

## 7 August 2020
{: #august-07-2020}

- **Virtual private endpoints (beta)** Endpoint gateways for {{site.data.keyword.cloud_notm}} VPC are now available in a limited, allow-listed beta. Use virtual private endpoints (VPE) to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, which is allocated from a subnet within your VPC. For more information, see [About Virtual Private Endpoints for VPC (Beta)](/docs/vpc?topic=vpc-about-vpe).

- **UI enhancements:**  
   - Improved name validation on provisioning pages
   - Eliminated polling on data tables
   - Enhanced data tables

## 30 July 2020
{: #july-30-2020}

- **Larger volume capacity for block storage for VPC (beta):** Provision volumes with capacities greater than 2 TB and up to 16 TB, depending on the profile you selected. See [Expanded Capacity IOPS Tiers (Beta)](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta).

- **Expanding block storage for VPC volume capacity:** Expand the size of new block storage volumes that you create and attach to a virtual server instance on Gen 2 Compute resources. Indicate capacity in GB increments up to 16 TB, depending on your volume profile limits. For more information, see [Expanding block storage volume capacity (Beta)](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

- **Encrypted images (beta):** Import an encrypted custom image with beta support for the feature. For more information, see [Creating an encrypted custom image (Beta)](/docs/vpc?topic=vpc-create-encrypted-custom-image).

- **New SDK:** The [Python SDK](https://{DomainName}/apidocs/vpc?code=python) is now generally available.

## 23 July 2020
{: #july-23-2020}

- **Customer-managed encryption for block storage for VPC (GA):** Customer-managed encryption is now available for protecting boot and data volumes on Gen 2 compute resources. Import your own root keys to the cloud or create a key using Key Protect or Hyper Protect Crypto Services. Then, use your key to secure your data. For more information, see [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption).

- **Flow logs for VPC (GA):** FLow logs for {{site.data.keyword.cloud_notm}} VPC are now generally available. This service enables the collection, storage, and presentation of information about IP traffic flowing to and from network interfaces within a VPC. For more information, see [About IBM Cloud Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs).

## 16 July 2020
{: #july-16-2020}

- **New region:** The Tokyo region endpoint is now in service at `https://jp-tok.iaas.cloud.ibm.com`. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).
- **"Add to estimate" capability updates:** Added support for itemized virtual server pricing in the Add to estimate capability.   

## 3 July 2020
{: #july-03-2020}

- **Network load balancer (beta):** Network load balancers are available in a limited beta. Use network load balancers to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [Load balancers overview](/docs/vpc?topic=vpc-nlb-vs-elb) and [About network load balancers](/docs/vpc?topic=vpc-network-load-balancers).

- **{{site.data.keyword.vpn_vpc_short}} update:** Access VPN monitoring metrics by using {{site.data.keyword.mon_full_notm}}. For more information, see [Monitoring VPN for VPC metrics](/docs/vpc?topic=vpc-vpn-monitoring-metrics).

## 26 June 2020
{: #june-26-2020}

**Dedicated hosts (beta):** In a limited beta, use dedicated hosts to carve out a single-tenant compute node. For more information, see [Creating dedicated hosts and groups (Beta)](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances).

## 19 June 2020
{: #june-19-2020}

- **Updated custom images information:** A new [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images) checklist is available. The procedure for migrating a virtual server instance from classic infrastructure to {{site.data.keyword.cloud_notm}} VPC is enhanced. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc).

- **User interface updates:**
  - Added pagination for the security groups list table
  - On the VPN Provision page, select a subnet from a table or create subnets
  - New "upgrade pending" button if a user account is pending upgrade

- **New SDK:** The [Go SDK](https://{DomainName}/apidocs/vpc?code=go) is now generally available.

## 2 June 2020
{: #june-02-2020}

**IBM Cloud Virtual Servers for VPC on POWER service is deprecated.** As of 02 June 2020, you cannot provision new instances. Any instance that is still provisioned as of 22 August 2020 will be deleted. For more information, see the [End of Service Announcement for Virtual Servers for VPC on POWER](https://www.ibm.com/cloud/blog/announcements/end-of-service-announcement-for-virtual-servers-for-vpc-on-power).

## 21 May 2020
{: #may-21-2020}

**Auto scale for VPC (beta):** In this limited beta, use auto scale to improve performance and costs by dynamically creating virtual server instances to meet the demands of your environment. For more information, see [Creating an instance group for auto scaling (Beta)](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

## 15 May 2020
{: #may-15-2020}

**Load balancer for VPC update:** Backend encryption (HTTPS for backend pool) is now supported. See [Using load balancers for VPC](/docs/vpc?topic=vpc-load-balancers) for details.

## 1 May 2020
{: #may-01-2020}

- **Customer-managed encryption (beta):** Customer-managed encryption (BYOK) is available in beta. Bring your own encryption keys and store the keys in Key Protect or Hyper Protect Crypto Services within VPC. Use BYOK to encrypt your VPC-enabled block storage at the volume level by using your own keys. For more information, see [Creating virtual server instances with customer-managed encryption (Beta)](/docs/vpc?topic=vpc-creating-instances-byok).

- **Flow logs (beta):** Flow logs are now available in beta. Use flow logs to collect, store, and present the Internet Protocol (IP) traffic flowing to and from networks within your VPC. For more information, see [About flow logs (Beta)](/docs/vpc?topic=vpc-flow-logs).

- **UI enhancement:** Create a new subnet directly from the page where you create a new instance.

## 22 April 2020
{: #april-22-2020}

**New region:** The Frankfurt region endpoint (eu-de) is now in service at `http://eu-de.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 17 April 2020
{: #april-17-2020}

**Documentation correction:** {{site.data.keyword.cloud_notm}} interprets volume capacity units in gibibytes, but the API documentation used gigabytes. This issue is now resolved in the documentation.

## 3 April 2020
{: #april-03-2020}

**Load balancer for VPC update:** Access load balancer monitoring metrics (throughput, active connections, connection rate) using [{{site.data.keyword.mon_full_notm}}](/docs/vpc?topic=vpc-monitoring-metrics-alb).

The following cipher suites are supported for load balancer HTTPS listeners:
- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256`

## 27 March 2020
{: #march-27-2020}

- **{{site.data.keyword.mon_full_notm}} monitoring:** Monitor virtual server instances using {{site.data.keyword.mon_full_notm}}. Use the new **Add monitoring** button on the instance's **Monitoring** page to provision an instance of the monitoring service. If a monitoring instance is already provisioned for the region, use the **Launch monitoring** button to view metrics associated with the instance. For more information, see [{{site.data.keyword.mon_full_notm}} metrics](/docs/vpc?topic=virtual-servers-sysdig-monitoring-metrics).
- **Updated styling:** VPC pages in {{site.data.keyword.IBM_notm}} console now use [Carbon 10](https://www.carbondesignsystem.com/){: external}, the IBM open source design system, which improves consistency and quality.

## 9 March 2020
{: #march-09-2020}

**IBM virtual servers for VPC on POWER:** POWER-based instances are now generally available. For more information, see the following resources:

* [IBM Cloud Virtual Private Cloud on POWER](https://developer.ibm.com/linuxonpower/power-virtual-private-cloud/){: external}  
* [Profiles](/docs/vpc?topic=vpc-profiles)

## 2 March 2020
{: #march-02-2020}

- **New region:** The London region endpoint (eu-gb) is now in service at `http://eu-gb.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).
- **UI enhancement:** A new public gateway details page is available. For more information about public gateways, see [external connectivity](/docs/vpc?topic=vpc-about-networking-for-vpc#external-connectivity).

## 14 February 2020
{: #february-14-2020}

The following VPC network services are now generally available:

- **Virtual private network (VPN):** Use VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premises private network or another VPC. For details, see [Using VPN](/docs/vpc?topic=vpc-using-vpn).
- **Load balancers:** Create public and private load balancers to distribute traffic among multiple server instances within the same region of your VPC. For details, see [Using load balancers](/docs/vpc?topic=vpc-load-balancers).

## 13 February 2020
{: #february-13-2020}

**Red Hat Enterprise Linux (RHEL) and Windows stock images are now available:** Provision an instance that uses an RHEL image or a Windows image in the Dallas region. For more information, see [Images](/docs/vpc?topic=vpc-about-images).

## 10 February 2020
{: #february-10-2020}

**API change notification:** We have temporarily suspended API support for creating new instances from an existing boot volume. For more information, see t[API change log](/docs/vpc?topic=vpc-api-change-log).

## 3 February 2020
{: #february-03-2020}

- **IBM virtual servers for VPC on POWER (beta):** The beta is now open to anyone interested, no signup necessary. For more information, see the following resources:
  * [IBM Cloud Virtual Private Cloud on POWER](https://developer.ibm.com/linuxonpower/power-virtual-private-cloud/){: external}  
  * [Profiles](/docs/vpc?topic=vpc-profiles)
- **Red Hat Enterprise Linux (RHEL) and Windows stock images are now available:** Provision an instance that uses an RHEL image or a Windows image in the Washington DC region. For more information, see [Images](/docs/vpc?topic=vpc-about-images).
- **UI enhancements:** The modals for provisioning and attaching a public gateway and for creating an SSH key are now replaced with a redesigned side pane.

## 10 January 2020
{: #january-10-2020}

- **New region:** The Washington DC	region endpoint (us-east) is now in service at `http://us-east.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).
- **CLI plug-in release 0.5.10:** For more information, see [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
  * You now have an Example section in command help for creating and updating commands. Example: `ibmcloud is help instance-create`, `ibmcloud is help instance-update`, or `ibmcloud is help volume-create`.
  * Use the _resource group filter_ in list commands. Example: `ibmcloud is vpcs --resource-group-name Littleton`.
  * JSON output format support for the `ibmcloud is target --json` command.
  * `ipv4` and `primary-network-interface` options for `instance-create`. Options help specify the primary private IP for the primary network interface when you create an instance.

## 9 December 2019
{: #dec-9-2019}

- **IBM virtual servers for VPC on POWER (beta):** Create POWER-based instances for all virtual server families, including the addition of the GPU family (for POWER only). For more information, see [Profiles](/docs/vpc?topic=vpc-profiles).
- **UI enhancement:** On the VPC details page, a new section is available to view source IP addresses for any cloud service endpoints you enabled. For more information, see [Service endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc).
- **CLI plug-in release 0.5.9:** Target a specific resource group by using the '[-g YOUR_GROUP]' command option. If you specify the target resource group by using the `ibmcloud target [-g YOUR_GROUP]`command, the output displays only VPC resources inside of the specified resource group. This update also introduces enhancements to some of the commands for `get` and `list`, showing more detail for your VPC, instances, and instance profiles. For more information, see [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

## 2 December 2019
{: #dec-2-2019}

**Access control lists:** Use an access control list (ACL) to control all incoming and outgoing traffic in {{site.data.keyword.vpc_full}}. For more information, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).

## 14 November 2019
{: #nov-14-2019}

**VPC layout:** View resources that are associated with a VPC in the {{site.data.keyword.cloud_notm}} console. For more information, see [Viewing resources associated with a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#vpc-layout).

## 7 November 2019
{: #nov-7-2019}

- **Load balancer (beta):** Use the {{site.data.keyword.cloud}} Load balancer for VPC service to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [Using load balancers (Beta)](/docs/vpc?topic=vpc-load-balancers).

- **VPN (beta):** Use the {{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_short}} service to securely connect your VPC to another private network. For more information, see [Using VPN (Beta)](/docs/vpc?topic=vpc-using-vpn).

## 31 October 2019
{: #oct-31-2019}

- **Classic access:** Set up access from a VPC to your {{site.data.keyword.cloud}} classic infrastructure, including Direct Link connectivity. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

- **Advanced routing:** Control the flow of network traffic in your VPC by configuring VPC routes. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
