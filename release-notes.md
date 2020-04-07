---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-05"

keywords: vpc release notes, changes, updates, vpc

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

Use these release notes to learn about the latest changes to {{site.data.keyword.vpc_full}}.
{:shortdesc}

<!--## 09 March 2020
{: #mar-09-2020}

- **IBM Virtual Servers for VPC on POWER:** POWER-based instances are now generally available. For more information, see the following resources:
  * [IBM Cloud Virtual Private Cloud on POWER](https://developer.ibm.com/linuxonpower/power-virtual-private-cloud/){: external}  
  * [Profiles](/docs/vpc?topic=vpc-profiles)-->
  
## 03 April 2020
{: #apr-03-2020}

- **Load Balancer for VPC Update** You can now access load balancer monitoring metrics (throughput, active connections, connection rate) using [IBM Cloud Monitoring with Sysdig](/docs/vpc?topic=vpc-monitoring-metrics-sysdig). 
- The following cipher suites are supported for load balancer HTTPS listeners:
    - `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
    - `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
    - `TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256`
    - `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
    - `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
    - `TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256`

## 27 March 2020
{: #mar-27-2020}

- **{{site.data.keyword.mon_full_notm}} monitoring** You can now monitor virtual server instances using {{site.data.keyword.mon_full_notm}}. Use the new **Add monitoring** button on the instance's **Monitoring** page to provision an instance of the monitoring service. If a monitoring instance is already provisioned for the region, use the **Launch monitoring** button to view metrics associated with the instance. For more information, see [Sysdig monitoring metrics](/docs/vpc?topic=virtual-servers-sysdig-monitoring-metrics). 
- **Updated styling** VPC pages in {{site.data.keyword.IBM_notm}} console now use [Carbon 10](https://www.carbondesignsystem.com/){: external}, IBM's open-source design system, which improves consistency and quality.

## 02 March 2020
{: #mar-02-2020}

- **New region** The London region is now available. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).
- **UI enhancement** There is a new public gateway details page. For more information about public gateways, see [external connectivity](/docs/vpc?topic=vpc-about-networking-for-vpc#external-connectivity).

## 14 February 2020
{: #feb-14-2020}

The following VPC network services are now generally available:

- **Virtual Private Network (VPN)**: Use VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premises private network or another VPC. For details, see [Using VPN](/docs/vpc?topic=vpc-using-vpn).
- **Load balancers**: Create public and private load balancers to distribute traffic among multiple server instances within the same region of your VPC. For details, see [Using load balancers](/docs/vpc?topic=vpc-load-balancers). 


## 13 February 2020
{: #feb-13-2020}

- **Red Hat Enterprise Linux (RHEL) and Windows stock images are now available** You can provision an instance that uses a RHEL image or a Windows image in the Dallas region. For more information, see [Images](/docs/vpc?topic=vpc-about-images).

## 10 February 2020
{: #feb-10-2020}

- **API change notification:**  We have temporarily suspended API support for creating new instances from an existing boot volume. For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log).

## 03 February 2020
{: #feb-03-2020}

- **IBM Virtual Servers for VPC on POWER (Beta):** No sign-up necessary. The beta is now open to anyone interested. For more information, see the following resources:
  * [IBM Cloud Virtual Private Cloud on POWER](https://developer.ibm.com/linuxonpower/power-virtual-private-cloud/){: external}  
  * [Profiles](/docs/vpc?topic=vpc-profiles)
- **Red Hat Enterprise Linux (RHEL) and Windows stock images are now available** You can provision an instance that uses a RHEL image or a Windows image in the Washington DC region. For more information, see [Images](/docs/vpc?topic=vpc-about-images).
- **UI enhancement** The modals for provisioning and attaching a public gateway, as well as, the modal for creating an SSH key are now replaced with an easier side pane design.

## 10 January 2020
{: #jan-10-2020}

- **New region** The Washington DC region is now available. For more information, see [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).
- **CLI plug-in release 0.5.10** For more information, see [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
  * You now have an Example section in command help for creating and updating commands. Example: `ibmcloud is help instance-create`, `ibmcloud is help instance-update`, or `ibmcloud is help volume-create`.
  * You can now use the _resource group filter_ in list commands. Example: `ibmcloud is vpcs --resource-group-name Littleton`.
  * JSON output format support for the `ibmcloud is target --json` command.
  * `ipv4` and `primary-network-interface` options for `instance-create`. These options help specify the primary private IP for the primary network interface when you create an instance.

## 9 December 2019
{: #dec-9-2019}

- **IBM Virtual Servers for VPC on POWER (Beta)** In Beta, you can create POWER-based instances for all virtual server families, including the addition of the GPU family (for POWER only). For more information, see [Profiles](/docs/vpc?topic=vpc-profiles).
- **UI enhancement** On the VPC details page, there is a new section to view source IP addresses for any cloud service endpoints you enabled. For more information, see [Service endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc).
- **CLI plug-in release 0.5.9** You can now target a specific resource group by using the '[-g YOUR_GROUP]' command option. If you specify the target resource group by using the `ibmcloud target [-g YOUR_GROUP]`command, the output only displays VPC resources inside of the specified resource group. There were also enhancements to some of the commands for `get` and `list`, showing more detail for your VPC, instances, and instance profiles. For more information, see [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

## 2 December 2019
{: #dec-2-2019}

- **Access control lists** You can use an access control list (ACL) to control all incoming and outgoing traffic in {{site.data.keyword.vpc_full}}. For more information, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).

## 14 November 2019
{: #nov-14-2019}

- **VPC layout** You can quickly view the resources that are associated with a VPC in the {{site.data.keyword.cloud_notm}} console. For more information, see [Viewing resources associated with a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#vpc-layout).

## 7 November 2019
{: #nov-7-2019}

- **Load balancer beta** You can use the {{site.data.keyword.cloud}} Load Balancer for VPC service to distribute traffic among multiple server instances within the same region of your VPC. For more information, see [Using load balancers (Beta)](/docs/vpc?topic=vpc-load-balancers).

- **VPN beta** You can use the {{site.data.keyword.cloud}} VPN for VPC service to securely connect your VPC to another private network. For more information, see [Using VPN (Beta)](/docs/vpc?topic=vpc-using-vpn).

## 31 October 2019
{: #oct-31-2019}

- **Classic access** You can set up access from a VPC to your {{site.data.keyword.cloud}} classic infrastructure, including Direct Link connectivity. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

- **Advanced Routing** You can control the flow of network traffic in your VPC by configuring VPC routes. For more information, see [Setting up advanced routing](/docs/vpc?topic=vpc-advanced-routing).
