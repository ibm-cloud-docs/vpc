---

copyright:
  years: 2018, 2026
lastupdated: "2026-01-08"

keywords: limitations, restrictions

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Service limitations
{: #limitations}

Limitations might change as capabilities are added, so feel free to check back from time to time.
{: shortdesc}

## General restrictions
{: #general-restrictions}

The following features are not supported, including all properties associated with these features:

* The following concept is not supported:
    * IPV6

* Virtual server instance name change: If you update the name of a virtual server, the name change might not appear consistently in different areas of the {{site.data.keyword.cloud}} console. For example, the virtual server name change might not be reflected in the {{site.data.keyword.cloud_notm}} console, or on the billing invoice, yet it appears correctly in the user's list of running instances.

* Direct Link on Classic access to VPC is supported through [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure) only. IBM Cloud Direct Link does not have this limitation.

* Nested virtualization on virtual server instances is not a supported configuration.

* Protocols other than ICMP, TCP, and UDP are not supported at the moment.

### Billing note
{: #billing-note}

When the VPC billing system reports network traffic from load balancers and VPNs, the resources are identified with a nonstandard name. The resource is identified by using the prefix `instance-`, followed by the last 16 digits of the back-end virtual server instance.

For example, if the load balancer UUID is `10000`, but it is running in a virtual server instance that has an internal UUID of `75007600770078007900`. The last 16 digits of the instance UUID are: `7600770078007900`.

The CRN identifies the load balancer as follows:`crn:v1:bluemix:public:is:eu-gb:a/0000000001::load-balancer:instance-7600770078007900`.

## Virtual private cloud restrictions
{: #virtual-private-cloud-restrictions}

An {{site.data.keyword.vpc_short}} cannot be peered with other VPCs natively. It is possible to connect VPCs by using either Transit Gateway, VPN Gateways, or Floating IPs.

* Both VPN Gateways and Floating IPs can't use an automatic route advertisement between the two VPCs. Static routes must be used in each VPC to enable layer 3 connectivity between the two VPCs. See [Example: Connecting two VPCs by using the VPN](/docs/vpc?topic=vpc-vpn-example) for how you can achieve VPC-to-VPC connectivity by using this method.
* With Transit Gateway, it advertises the root subnets of each VPC allowing traffic to be routed without the use of static routes. For more information, see [Getting started with IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started).

## Compute restrictions
{: #compute-restrictions}

* Start and Stop actions are not registered under virtual server instance activity in the console.
* The placement group of the instance can't be changed after an instance is provisioned with a placement group. You must delete the instance to remove it from the placement group.

## Storage restrictions
{: #storage-restrictions}

Block Storage volume names must be unique across the entire VPC infrastructure. A volume that is created on VPC compute resources can't have the same name as a volume created on the classic infrastructure. Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter.

## LinuxONE (s390x processor architecture) virtual server instance restrictions
{: #LinuxONE-vsi-restrictions}

The following feature is not supported:

* Dedicated hosts
* VPC Metadata service

## z/OS virtual server instance restrictions
{: #zos-vsi-restrictions}

* For limitations of z/OS virtual server instances, see [{{site.data.keyword.waziaas_full_notm}} documentation](https://www.ibm.com/docs/en/wazi-aas/1.1.0?topic=known-limitations){: external}.
