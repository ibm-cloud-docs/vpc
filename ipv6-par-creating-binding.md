---

copyright:
  years: 2026
lastupdated: "2026-10-27"

keywords: vpc, public address ranges, ipv6, create, bind, vni

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating IPv6 public address ranges and binding to a VNI
{: #ipv6-par-creating-binding}

[Public IPv6 address ranges is an allowlisted beta feature](/docs/vpc?topic=vpc-release-notes&interface=ui#vpc-oct2726) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You can create an IPv6 public address range from an IPv6 authorized CIDR and bind it to a virtual network interface (VNI). After binding, traffic for any IP in the range is routed automatically to the VNI.
{: shortdesc}

You can create and bind IPv6 public address ranges with the console, CLI, API, and Terraform.

## Before you begin
{: #ipv6-par-creating-binding-before-you-begin}

* An IPv6 authorized CIDR must exist in the target zone. See [Creating IPv6 authorized CIDRs](/docs/vpc?topic=vpc-ipv6-par-authorized-cidr).
* A VNI must exist in the same zone as the authorized CIDR.
* Make sure that you have the IAM action `is.public-address-range-authorized-cidr.authorized-cidr.create` and standard public address range create and update permissions.
* Review [planning considerations](/docs/vpc?topic=vpc-ipv6-par-about#ipv6-par-planning) for IPv6 public address ranges.

After binding a public address range to a VNI, the range remains allocated from the authorized CIDR even when unbound. Deleting the range releases the address space back to the authorized CIDR.
{: tip}

## Creating an IPv6 public address range in the console
{: #ipv6-par-create-ui}
{: ui}

To create an IPv6 public address range in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Click **Create** on the **Public address ranges** tab, or click **Create public address range** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") next to an authorized CIDR on the **IPv6 authorized CIDRs** tab.
1. For **IP version**, select **IPv6**.
1. Select the **Location** where you want to create the public address range.
1. In the **Details** section, complete the following fields:
   - **Name**: Enter a name for the public address range, or leave blank to auto-generate a name after creation.
   - **Resource group**: Select a resource group.
   - **Tags**: Optional: Add tags to help organize your resources.
   - **Authorized CIDR**: Select the IPv6 authorized CIDR to allocate from. If no authorized CIDR exists for the selected zone, the field shows **No CIDR available** and the rest of the form is disabled. Click **Create authorized CIDR** to open the authorized CIDR creation panel without leaving this flow. After the authorized CIDR is created, the **Authorized CIDR** field is automatically populated with the new CIDR and the form is re-enabled. You can then complete the remaining fields and click **Create**.
   - **Reserving method**: Select how the IP range is assigned:
     - **Select a range for me**: IBM selects an available block of the specified size from the authorized CIDR. Select the **Size** (prefix length) from the following options: `/128` (1 address), `/127` (2 addresses), `/126` (4 addresses), `/120` (256 addresses), `/112` (65,536 addresses), `/96` (more than 4 billion addresses), or `/64` (full authorized CIDR).
     - **Let me specify**: Enter a specific CIDR block in the **IP address range** field. The block must fall within the available address space of the selected authorized CIDR.
1. Optional: In the **Binding** section, select a virtual network interface to bind the range to immediately. You can also bind the range later after creation.
1. Click **Create**.

The range is created and is displayed in the **Public address ranges** list alongside any existing IPv4 ranges. IPv6 ranges are identified by their IPv6 CIDR in the **IP range** column and show **VNI** in the **Target type** column. If you selected a VNI, the status shows **Bound**. If you did not select a VNI, the status shows **Unbound**.

## Creating an IPv6 public address range from the CLI
{: #ipv6-par-create-cli}
{: cli}

To create an IPv6 public address range from the command line, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your CLI environment:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Run one of the following commands:

   ```sh
   ibmcloud is public-address-range-create
     (--authorized-cidr AUTHORIZED_CIDR --network-prefix-length NETWORK_PREFIX_LENGTH | --cidr CIDR)
     [--name NAME] [--vni VNI]
     [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
     [--output JSON] [-q, --quiet]
   ```
   {: pre}

   Where:

   `--authorized-cidr`
   :   The ID or name of the IPv6 authorized CIDR to allocate this public address range from. IBM selects an available block of the specified prefix length. Requires `--network-prefix-length`.

   `--network-prefix-length`
   :   The prefix length of the IPv6 block to allocate. Supported values: `64`–`128`. Requires `--authorized-cidr`.

   `--cidr`
   :   An exact IPv6 CIDR block to allocate. Must be an unallocated block within an authorized CIDR in your account. Mutually exclusive with `--authorized-cidr`.

   `--name`
   :   A name for the public address range. Names beginning with `ibm-` are reserved.

   `--vni`
   :   The ID or name of the VNI to bind this range to. The VNI must be in the same zone as the authorized CIDR.

   `--resource-group-id`
   :   ID of the resource group. Mutually exclusive with `--resource-group-name`.

   `--resource-group-name`
   :   Name of the resource group. Mutually exclusive with `--resource-group-id`.

### Command examples
{: #ipv6-par-create-cli-examples}

Create an IPv6 public address range from an authorized CIDR, let IBM select a `/112` block, and bind to a VNI:

```sh
ibmcloud is public-address-range-create --authorized-cidr my-ipv6-auth-cidr --network-prefix-length 112 --name my-ipv6-par --vni my-vni
```
{: pre}

Create an IPv6 public address range with a specific CIDR block (unbound):

```sh
ibmcloud is public-address-range-create --cidr 2001:db8:85a3::8a2e:0:0/112 --name my-ipv6-par-specific
```
{: pre}

## Creating an IPv6 public address range with the API
{: #ipv6-par-create-api}
{: api}

Before you begin, set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
{: requirement}

Select one of the following options:

* Create an IPv6 public address range from an authorized CIDR, bound to a VNI (IBM selects the block):

   ```sh
   curl -sX POST \
   "$vpc_api_endpoint/v1/public_address_ranges?version=$api_version&generation=2" \
   -H "Authorization: Bearer $iam_token" \
   -H "Content-Type: application/json" \
   -d '{
         "name": "my-ipv6-par",
         "authorized_cidr": { "id": "'$authorized_cidr_id'" },
         "network_prefix_length": 112,
         "target": {
            "virtual_network_interface": { "id": "'$vni_id'" }
         }
      }'
   ```
   {: pre}

* Create an IPv6 public address range from a specific CIDR block, unbound:

   ```sh
   curl -sX POST \
   "$vpc_api_endpoint/v1/public_address_ranges?version=$api_version&generation=2" \
   -H "Authorization: Bearer $iam_token" \
   -H "Content-Type: application/json" \
   -d '{
         "name": "my-ipv6-par",
         "cidr": "2001:db8:85a3::8a2e:0:0/112"
      }'
   ```
   {: pre}

## Creating an IPv6 public address range with Terraform
{: #ipv6-par-create-terraform}
{: terraform}

To create an IPv6 public address range from an authorized CIDR, bound to a VNI:

```terraform
resource "ibm_is_public_address_range" "my_ipv6_par" {
  name                  = "my-ipv6-par"
  network_prefix_length = 112
  authorized_cidr {
    id = ibm_is_public_address_range_authorized_cidr.my_ipv6_auth_cidr.id
  }
  target {
    virtual_network_interface {
      id = ibm_is_virtual_network_interface.my_vni.id
    }
  }
}
```
{: codeblock}

## Binding an IPv6 public address range to a VNI
{: #ipv6-par-binding}

You can bind an existing unbound IPv6 public address range to a VNI, or move a range from one VNI to a different VNI. The VNI must be in the same zone as the authorized CIDR the range was allocated from.

You don't need to unbind first when moving to a different VNI. The range is automatically unbound from its current VNI and bound to the new one.
{: tip}

### Binding in the console
{: #ipv6-par-bind-ui}
{: ui}

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Highlight the row of the unbound IPv6 public address range, then click **Bind** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. In the **Bind public address range** panel, the **IP range** and **Location** fields are read-only. From the **Virtual network interface** list, select the VNI in the same zone as the authorized CIDR.
   - If the VNI that you want isn't listed, click the refresh icon next to the list to update it.
   - If no VNI exists in the zone, click **Create virtual network interface** to create one. For more information, see [Creating a virtual network interface](/docs/vpc?topic=vpc-vni-create&interface=ui).
1. Click **Attach**.

After the range is bound, the **Status** column shows **Bound**, and the **Target resource** and **Target type** columns show the VNI name and **VNI**.

### Moving to a different VNI in the console
{: #ipv6-par-move-ui}
{: ui}

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Highlight the row of the bound IPv6 public address range, then click **Edit** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. In the **Edit public address range** panel, select a different VNI from the **Virtual network interface** list.
1. Click **Attach**.

### Binding from the CLI
{: #ipv6-par-bind-cli}
{: cli}

```sh
ibmcloud is public-address-range-update PUBLIC_ADDRESS_RANGE --vni VNI [--output JSON] [-q, --quiet]
```
{: pre}

Example:

```sh
ibmcloud is public-address-range-update my-ipv6-par --vni my-vni
```
{: pre}

### Binding with the API
{: #ipv6-par-bind-api}
{: api}

```sh
curl -sX PATCH \
  "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -H "Content-Type: application/json" \
  -d '{
        "target": {
           "virtual_network_interface": { "id": "'$vni_id'" }
        }
     }'
```
{: pre}

### Binding with Terraform
{: #ipv6-par-bind-terraform}
{: terraform}

Update the `target` block in your resource configuration:

```terraform
resource "ibm_is_public_address_range" "my_ipv6_par" {
  name                  = "my-ipv6-par"
  network_prefix_length = 112
  authorized_cidr {
    id = ibm_is_public_address_range_authorized_cidr.my_ipv6_auth_cidr.id
  }
  target {
    virtual_network_interface {
      id = ibm_is_virtual_network_interface.my_vni.id
    }
  }
}
```
{: codeblock}

## Unbinding an IPv6 public address range
{: #ipv6-par-unbinding}

After unbinding, the range is still allocated from the authorized CIDR and can be bound again to any VNI in the same zone.
{: tip}

### Unbinding in the console
{: #ipv6-par-unbind-ui}
{: ui}

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Highlight the row of the IPv6 public address range, then click **Unbind** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. Click **Unbind** to confirm.

### Unbinding from the CLI
{: #ipv6-par-unbind-cli}
{: cli}

```sh
ibmcloud is public-address-range-update my-ipv6-par --reset-target
```
{: pre}

### Unbinding with the API
{: #ipv6-par-unbind-api}
{: api}

```sh
curl -sX PATCH \
  "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -H "Content-Type: application/json" \
  -d '{ "target": null }'
```
{: pre}

## Related links
{: #ipv6-par-creating-binding-related-links}

- [About public IPv6 address ranges](/docs/vpc?topic=vpc-ipv6-par-about)
- [Creating IPv6 authorized CIDRs](/docs/vpc?topic=vpc-ipv6-par-authorized-cidr)
- [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about)
- [IAM roles and actions](/docs/iam?topic=iam-iam-service-roles-actions#is.public-address-range-roles)
- [Quotas](/docs/vpc?topic=vpc-quotas#par-quotas) and [service limits](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services)
- [FAQ for public address ranges](/docs/vpc?topic=vpc-faq-public-address-ranges)
