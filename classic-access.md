---
copyright:
  years: 2018, 2026
lastupdated: "2026-04-07"

keywords: classic, access, classic access, VRF, peering

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up access to Classic infrastructure
{: #setting-up-access-to-classic-infrastructure}

**End of Marketing (EOM) for Classic access VPC** - Starting 31 October 2024, the “Classic access” option is not available in the IBM Cloud console (UI). If your account doesn't have any Classic-access-enabled VPCs, as of 31 December 2024, you can't create VPCs with “Classic access” enabled (including through the API, CLI, SDK, and Terraform). Instead, you can use [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started) to connect your VPCs to the Classic network. If your account has existing Classic-access-enabled VPCs, those VPCs continue to work without any change and you can continue to create Classic-access-enabled VPCs in that account through API and CLI.
{: deprecated}

You can set up access from a VPC to your {{site.data.keyword.cloud}} Classic infrastructure, including {{site.data.keyword.cloud_notm}} Direct Link connectivity. Only one VPC per region can communicate with Classic resources.
{: shortdesc}

The recommended method to interconnect Classic network to VPC is by using [{{site.data.keyword.cloud_notm}} Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started). This method connects VPCs to various resources locally and across regions.
{: important}

Alternatively, when you set up a VPC for Classic access, every virtual server or bare metal server without a public interface in your Classic account can send and receive packets to and from the Classic-access VPC. Firewalls, gateways, network ACLs, or security groups can filter some or all of this traffic. As a best practice, allow only the traffic that is required for your applications to function properly.

For virtual server instances and bare metal instances on the Classic infrastructure that use a public interface, you must add a route that points back to your Classic-enabled VPC. This route must include the subnets of your Classic-enabled VPC as a destination. The route must also point to a gateway address for traffic that departs the private interface of the host as the next hop.

## Prerequisites
{: #vpc-prerequisites}

Your Classic account must be enabled for Virtual Router Forwarding (VRF). If your account is not VRF-enabled, see [Enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint) to learn more about the conversion process.

All subnets in a Classic access VPC are shared into the Classic infrastructure VRF, which uses IP addresses in the `10.0.0.0/8` space. To avoid IP address conflicts, don't use IP addresses in the `10.0.0.0/14`, `10.200.0.0/14`, `10.198.0.0/15`, and `10.254.0.0/16` blocks in your Classic access VPC. Also, don't use addresses from your Classic infrastructure subnets. To view the list of your Classic infrastructure subnets, see [View all Subnets](/docs/subnets?topic=subnets-view-all-subnets).

## Creating a Classic access VPC
{: #create-a-classic-access-vpc}

You can create a Classic access VPC by using the {{site.data.keyword.cloud_notm}} console, CLI, or API.

A VPC must be set up for Classic access when it is created. You can't update a VPC to add or remove Classic access.
{: important}

### Using the {{site.data.keyword.cloud_notm}} console to create a Classic access VPC
{: #create-a-classic-access-vpc-from-the-user-interface}
{: ui}

On the **New virtual private cloud** page, select **Enable access to Classic resource** under **Classic access**.

### Using the CLI to create a Classic access VPC
{: #create-a-classic-access-vpc-using-the-cli}
{: cli}

Use the option `--classic-access` when you create the VPC, for example:

```sh
ibmcloud is vpc-create my-access-vpc --classic-access true
```
{: pre}


### Using the API to create a Classic access VPC
{: #create-a-classic-access-vpc-using-the-api}
{: api}

Pass in the `classic_access` parameter when you create the VPC, for example:

```bash
curl -X POST "$vpc_api_endpoint/v1/vpcs?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### Classic access VPC default address prefixes
{: #classic-access-default-address-prefixes}

Classic virtual servers aren't available in the Madrid MZR.
{: important}

When a Classic access VPC is created, a default address prefix is also created in each zone of the region. Unlike a VPC without Classic access, the default address prefixes for a Classic access VPC are not in the `10.0.0.0/8` range. Which is the range that is used for private IP addresses of local Classic resources. This range prevents Classic access VPC private addresses from colliding with private addresses of Classic resources.

For x86-64 dedicated host profiles, the Madrid region supports only dedicated host profiles with instance storage. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui).

To prevent address prefixes from being created, you can add the `"address_prefix_management": "manual"` parameter when you create the VPC by using the API:

```bash
curl -H "Authorization:$iam_token" "$iaas_endpoint/v1/vpcs?generation=2&version=$api_version" -X POST -d '{ "name": "my-access-vpc", "address_prefix_management": "manual", "classic_access": true}'
```
{: pre}


## Limitations
{: #vpc-limitations}

* Only private networks (also known as back-end networks) in Classic infrastructure can be connected to your VPC.
* Only subnets allocated to your Classic infrastructure with {{site.data.keyword.cloud_notm}} provisioning systems can be connected to your VPC.
* Only one VPC per region can be enabled for Classic access.
* All Classic access VPCs must have globally unique address prefixes that do not overlap.
* If your Classic infrastructure includes an imported default route from Direct Link, the imported default route is used by your Classic Access VPC. In this scenario, the public gateway and floating IPs in your Classic Access VPC do not provide internet access. When the default route is no longer imported through Direct Link, then public gateways and floating IPs can again provide internet access.
