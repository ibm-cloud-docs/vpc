---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: BYOIP, floating IP, custom authorized CIDR, reserve, bring your own IP

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Reserving floating IPs from a custom authorized CIDR
{: #byoip-fip-create}

[IPv4 BYOIP is an allowlisted beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You can reserve a floating IP address from a custom authorized CIDR instead of from IBM-managed IP pools. This setup allows you to use your own publicly routable IP addresses as floating IPs that are attached to virtual network interfaces or public gateways in your VPC.
{: shortdesc}

## Before you begin
{: #byoip-fip-before-you-begin}

- You must have a [custom authorized CIDR](/docs/vpc?topic=vpc-provision-custom-authorized-cidr) that is provisioned and in `stable` status.
- Floating IPs from a custom authorized CIDR can only be attached to a virtual network interface (VNI) or a public gateway. They cannot be attached to legacy network interfaces.

## Reserving a floating IP from a custom authorized CIDR in the console
{: #byoip-fip-create-ui}
{: ui}

To reserve a floating IP from a custom authorized CIDR in the {{site.data.keyword.cloud_notm}} console:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Network > Floating IPs**.
1. Click **Reserve**.
1. In the **Location** section, select the zone where you want to reserve the floating IP.
1. In the **Details** section, complete the following fields:
   - **Name**: Enter a unique name for the floating IP using lowercase alphanumeric characters and hyphens only (without spaces).
   - **Resource group**: Select the resource group for the floating IP.
   - **Tags** (optional): Add tags to help organize your resources. You can add more tags later.
   - **Access management tags** (optional): Add access management tags in `key:value` format. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
1. In **Public address source**, select **Custom authorized CIDRs**.

   When you select **IBM CIDRs** (the default), the system assigns an IP address from IBM-managed pools. When you select **Custom authorized CIDRs**, you allocate an IP from your own authorized range.
   {: tip}

1. From the **Authorized CIDR** menu, select the custom authorized CIDR that you want to allocate the IP from.

   Only authorized CIDRs that are in `stable` status and associated with the selected zone are available.
   {: note}

1. (Optional) In the **Binding** section, bind the floating IP to a resource:
   - **Resource**: Select an instance, server, or virtual network interface from the dropdown.
   - **Network interface**: After you select a resource, choose the specific network interface to bind to.
1. Review the **Total estimated cost** displayed at the bottom of the panel. VPC egress public bandwidth charges apply to data traffic transferred through the floating IP.
1. Click **Reserve**.

After the floating IP is reserved, it appears in the Floating IPs list with the address from your custom authorized CIDR. The **Status** column shows **Bound** or **Unbound** depending on whether you selected a binding target.

## Creating a floating IP from a custom authorized CIDR from the CLI
{: #byoip-fip-create-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To reserve a floating IP from a custom authorized CIDR, use the `--address` flag to specify an unallocated IP address from your authorized CIDR:

```sh
ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE_NAME | --nic TARGET_INTERFACE [--in TARGET_INSTANCE | --bm TARGET_BARE_METAL_SERVER] | --vni VNI | --reset-target) [--address ADDRESS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
```
{: pre}

For example, to reserve a floating IP with a specific address from your authorized CIDR:

```sh
ibmcloud is floating-ip-reserve my-byoip-fip --address 46.16.186.112 --zone us-south-2
```
{: pre}

Where:

`FLOATING_IP_NAME`
:   The name for the floating IP.

`--address`
:   An unallocated IP address within a custom authorized CIDR in your account. Required if neither `--nic` nor `--zone` is specified.

`--zone`
:   Name of the target zone.

`--nic`
:   The ID or name of the network interface to be bound.

`--vni`
:   ID or name of the virtual network interface.

To bind the floating IP to a virtual network interface at creation time:

```sh
ibmcloud is floating-ip-reserve my-byoip-fip --address 46.16.186.112 --vni vni2
```
{: pre}

To list floating IPs filtered by a specific profile:

```sh
ibmcloud is floating-ips --profile-name byoip-ipv4
```
{: pre}

The address must be an unallocated IP within a custom authorized CIDR in your account. Floating IPs from a custom authorized CIDR can be attached only to a VNI or a public gateway.
{: important}

## Creating a floating IP from a custom authorized CIDR with the API
{: #byoip-fip-create-api}
{: api}

Before you begin, set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
{: requirement}

To reserve a floating IP from a custom authorized CIDR, specify the `address` field in the request body:

```sh
curl -sX POST "$vpc_api_endpoint/v1/floating_ips?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -H "Content-Type: application/json" \
  -d '{
    "address": "192.168.0.4",
    "name": "my-byoip-fip",
    "zone": {
      "name": "us-south-2"
    }
  }'
```
{: pre}

To reserve and bind to a virtual network interface in a single request:

```sh
curl -sX POST "$vpc_api_endpoint/v1/floating_ips?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -H "Content-Type: application/json" \
  -d '{
    "address": "192.168.0.4",
    "name": "my-byoip-fip",
    "target": {
      "id": "69e55145-cc7d-4d8e-9e1f-cc3fb60b1793"
    }
  }'
```
{: pre}

The address must be an unallocated IP within a custom authorized CIDR in your account.
{: important}



## Next steps
{: #byoip-fip-create-related}

* [Deprovision a custom authorized CIDR](/docs/vpc?topic=vpc-deprovision-custom-authorized-cidr&interface=ui).
* [Prepare a Letter of Authorization](/docs/vpc?topic=vpc-ipv4-letter-of-authorization).
