---
copyright:
  years: 2018, 2024
lastupdated: "2024-10-03"

keywords: classic, access, classic access, VRF, peering

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up access to classic infrastructure
{: #setting-up-access-to-classic-infrastructure}
 
**End of Market (EOM) Announcement for deprecation of Classic access VPC** - Starting 3 Nov 2024, the “Classic access” option will not be available in the IBM Cloud console (UI). Additionally, if your account doesn't have any classic-access-enabled VPCs, as of 31 Dec 2024, you can't create VPCs with “Classic access” enabled (including through the CLI, Terraform, and the API). Instead, [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started) can be used to connect your VPCs to the Classic network. If your account has existing classic-access-enabled VPCs, those VPCs will continue to work without any change and you can continue to create classic-access-enabled VPCs in that account through API and CLI.
{: deprecated} 
  
You can set up access from a VPC to your {{site.data.keyword.cloud}} classic infrastructure, including {{site.data.keyword.cloud_notm}} Direct Link connectivity. Only one VPC per region can communicate with classic resources.
{: shortdesc}

The recommended method to interconnect classic network to VPC is by using [{{site.data.keyword.cloud_notm}} Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started). This allows you to connect VPCs to various resources locally and across regions.
{: important}

Alternatively, when you set up a VPC for classic access, every virtual server instance or bare metal server without a public interface in your classic account can send and receive packets to and from the classic-access VPC. Firewalls, gateways, network ACLs, or security groups can filter some or all of this traffic. As a best practice, allow only the traffic that is required for your applications to function properly.

For virtual server instances and bare metal instances on the classic infrastructure that use a public interface, you must add a route that points back to your classic-enabled VPC. This route must include the subnets of your classic-enabled VPC as a destination. The route must also point to a gateway address for traffic that leaves the private interface of the host as the next hop.
{: important}

## Prerequisites
{: #vpc-prerequisites}

Your classic account must be enabled for Virtual Router Forwarding (VRF). If your account is not VRF-enabled, see [Converting to VRF](/docs/account?topic=account-vrf-service-endpoint#vrf) to learn more about the conversion process.

All subnets in a classic access VPC are shared into the classic infrastructure VRF, which uses IP addresses in the `10.0.0.0/8` space. To avoid IP address conflicts, don't use IP addresses in the `10.0.0.0/14`, `10.200.0.0/14`, `10.198.0.0/15`, and `10.254.0.0/16` blocks in your classic access VPC. Also, don't use addresses from your classic infrastructure subnets. To view the list of your classic infrastructure subnets, see [View all Subnets](/docs/subnets?topic=subnets-view-all-subnets).
{: important}

## Creating a classic access VPC
{: #create-a-classic-access-vpc}

You can create a classic access VPC by using the {{site.data.keyword.cloud_notm}} console, CLI, or API.

A VPC must be set up for classic access when it is created: you cannot update a VPC to add or remove classic access.
{: important}

### Using the {{site.data.keyword.cloud_notm}} console to create a classic access VPC
{: #create-a-classic-access-vpc-from-the-user-interface}
{: ui}

On the **New virtual private cloud** page, select **Enable access to classic resource** under **Classic access**.

### Using the CLI to create a classic access VPC
{: #create-a-classic-access-vpc-using-the-cli}
{: cli}

Use the flag `--classic-access` when you create the VPC, for example:

```sh
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### Using the API to create a classic access VPC
{: #create-a-classic-access-vpc-using-the-api}
{: api}

Pass in the `classic_access` parameter when you create the VPC, for example:

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### Classic access VPC default address prefixes
{: #classic-access-default-address-prefixes}

Classic Virtual Servers aren't available in the Madrid MZR.
{: important}

When a classic access VPC is created, a default address prefix is also created in each zone of the region. Unlike a VPC without classic access, the default address prefixes for a classic access VPC are not in the `10.0.0.0/8` range, which is the range used for private IP addresses of local classic resources. This prevents classic access VPC private addresses from colliding with private addresses of classic resources.

For x86-64 dedicated host profiles, the Madrid region only supports dedicated host profiles with instance storage. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui).
{: important}

To prevent address prefixes from being created, you can add the `"address_prefix_management": "manual"` parameter when you create the VPC by using the API:

```bash
curl -H "Authorization:$iam_token" "$iaas_endpoint/v1/vpcs?generation=2&version=$api_version" -X POST -d '{ "name": "my-access-vpc", "address_prefix_management": "manual", "classic_access": true}'
```
{: pre}


## Limitations
{: #vpc-limitations}

* Only private networks (also known as back-end networks) in classic infrastructure can be connected to your VPC.
* Only subnets allocated to your classic infrastructure with {{site.data.keyword.cloud_notm}} provisioning systems can be connected to your VPC.
* Only one VPC per region can be enabled for classic access.
* All classic access VPCs must have globally unique address prefixes that do not overlap.
* If your classic infrastructure includes an imported default route from Direct Link, the imported default route is used by your Classic Access VPC. In this scenario, the public gateway and floating IPs in your Classic Access VPC do not provide internet access. When the default route is no longer imported via Direct Link, then public gateways and floating IPs once again provide internet access.
