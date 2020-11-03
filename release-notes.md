---

copyright:
  years: 2019, 2020
lastupdated: "2020-11-02"

keywords: release notes, changes, updates

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:external: target="_blank" .external}

# Release notes
{: #release-notes}

Use the release notes to learn about new and changed {{site.data.keyword.vpc_full}} features.
{:shortdesc}

## 2 November 2020
{: #november-2-2020}

- **Custom routing tables and routes (GA):** {{site.data.keyword.cloud_notm}} Custom Routes for VPC enables you to configure and control the routing behavior of traffic within your VPC. This functionality provides the ability to explicitly manage traffic between and among subnets within a VPC; thereby, supporting network topologies that incorporate the use of virtual appliances, such as firewalls. This service also allows {{site.data.keyword.cloud_notm}} to continue to develop and deliver advanced features and capabilities for our {{site.data.keyword.cloud_notm}} services. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

- **IP spoofing (GA)** allows users to enable or disable the source/destination check on virtual server instances. This facilitates the traffic flow configured with custom routes functionality. For more information, see [About IP spoofing](/docs/vpc?topic=vpc-ip-spoofing-about).

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

- **VPN for VPC update:** Access VPN monitoring metrics by using [IBM Cloud Monitoring with Sysdig VPN metrics](/docs/vpc?topic=vpc-sysdig-monitoring-metrics).

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

**Load balancer for VPC update:** Access load balancer monitoring metrics (throughput, active connections, connection rate) using [IBM Cloud Monitoring with Sysdig](/docs/vpc?topic=vpc-monitoring-metrics-sysdig).

The following cipher suites are supported for load balancer HTTPS listeners:
- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256`

## 27 March 2020
{: #march-27-2020}

- **{{site.data.keyword.mon_full_notm}} monitoring:** Monitor virtual server instances using {{site.data.keyword.mon_full_notm}}. Use the new **Add monitoring** button on the instance's **Monitoring** page to provision an instance of the monitoring service. If a monitoring instance is already provisioned for the region, use the **Launch monitoring** button to view metrics associated with the instance. For more information, see [Sysdig monitoring metrics](/docs/vpc?topic=virtual-servers-sysdig-monitoring-metrics).
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

- **VPN (beta):** Use the {{site.data.keyword.cloud}} VPN for VPC service to securely connect your VPC to another private network. For more information, see [Using VPN (Beta)](/docs/vpc?topic=vpc-using-vpn).

## 31 October 2019
{: #oct-31-2019}

- **Classic access:** Set up access from a VPC to your {{site.data.keyword.cloud}} classic infrastructure, including Direct Link connectivity. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

- **Advanced routing:** Control the flow of network traffic in your VPC by configuring VPC routes. For more information, see [Setting up advanced routing](/docs/vpc?topic=vpc-advanced-routing).
