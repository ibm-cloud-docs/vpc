---

copyright:
  years: 2026
lastupdated: "2026-10-28"

keywords: vpc, public address ranges, ipv6, authorized cidr, create

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating and managing IPv6 authorized CIDRs
{: #ipv6-par-authorized-cidr}

[Public IPv6 address ranges](/docs/vpc?topic=vpc-release-notes&interface=ui#vpc-oct2726) is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

An IPv6 authorized CIDR reserves public IPv6 address space for your account in a specific zone. Use authorized CIDRs to create public address ranges that can be bound to virtual network interfaces (VNIs). You must create an authorized CIDR before you can create any IPv6 public address ranges in that zone.
{: shortdesc}

You can create, view, rename, and delete IPv6 authorized CIDRs with the console, CLI, API, and Terraform.

## Before you begin
{: #ipv6-par-authorized-cidr-before-you-begin}

* Make sure that you have the IAM action `is.public-address-range-authorized-cidr.authorized-cidr.create` to create an authorized CIDR.
* Check your quota: up to 5 IPv6 authorized CIDRs per account per zone. See [Quotas](/docs/vpc?topic=vpc-quotas#par-quotas).
* An authorized CIDR can be deleted only when no public address ranges are allocated from it (`allocation.count` is `0`).

## Creating an IPv6 authorized CIDR in the console
{: #ipv6-par-authorized-cidr-create-ui}
{: ui}

To create an IPv6 authorized CIDR in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Click the **IPv6 authorized CIDRs** tab.
1. Click **Create**.
1. In the **Location** section, select the **Zone** where you want to create the authorized CIDR.
1. In the **Details** section, complete the following fields:
   - **Name**: Enter a name for the authorized CIDR, or leave blank to auto-generate a name after creation.
   - **Resource group**: Select the resource group for this authorized CIDR.
   - **Tags**: Optional: Add tags to help organize your resources.
   - **Size**: Read-only. The size is fixed at `/64`, the standard subnet size for IPv6.
1. Click **Create**.

IBM allocates a `/64` IPv6 CIDR block from its public pool for the selected zone at no cost. A **Creating authorized CIDR** notification is displayed while the request is processed, followed by an **Authorized CIDR requested** confirmation when it is ready. The new authorized CIDR is displayed in the **IPv6 authorized CIDRs** table with its assigned CIDR block, zone, and an **Allocated resources** count of `0`.

## Viewing IPv6 authorized CIDR details in the console
{: #ipv6-par-authorized-cidr-view-ui}
{: ui}

To view the details of an IPv6 authorized CIDR, click its name in the **IPv6 authorized CIDRs** table. The details page shows two tabs:

- **Overview**: Displays the CIDR details including name, ID, IP range, resource group, location, CRN, and created date. The **Allocated resources** section shows the total number of public address ranges allocated from this CIDR and a **Manage allocated resources** button that links to the **Allocated resources** tab.
- **Allocated resources**: Lists all public address ranges allocated from this authorized CIDR. The table shows the **Public address range name**, **CIDR**, **Status** (Bound or Unbound), and **Targeted VNI** (the VNI the range is bound to, if any). When no ranges have been allocated yet, the table displays *"No allocated resources — Click 'Create public address range' to get started."* Click **Create public address range** to allocate a new range directly from this authorized CIDR.

To delete the authorized CIDR from the details page, click **Actions** > **Delete**. The authorized CIDR must have no allocated public address ranges before it can be deleted.

## Managing IPv6 authorized CIDRs in the console
{: #ipv6-par-authorized-cidr-manage-ui}
{: ui}

From the **IPv6 authorized CIDRs** tab, you can view all authorized CIDRs in the selected region. The table shows the **Name**, **CIDR**, **Zone**, and **Allocated resources** count for each entry.

To take action on an authorized CIDR, click the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") in the row. The following options are available:

- **Rename**: Enter a new name for the authorized CIDR.
- **Create public address range**: Opens the create flow to allocate an IPv6 public address range directly from this authorized CIDR.
- **Delete**: Initiates the delete flow for the authorized CIDR.

## Deleting an IPv6 authorized CIDR in the console
{: #ipv6-par-authorized-cidr-delete-ui}
{: ui}

To delete an IPv6 authorized CIDR from the console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Click the **IPv6 authorized CIDRs** tab.
1. Click the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") in the row of the authorized CIDR you want to delete, then click **Delete**.
1. Review the **Resources to delete** panel. The panel lists all public address ranges allocated from the authorized CIDR, their CIDR blocks, status (Bound or Unbound), and the targeted VNI for any bound ranges. You can switch between **Table view** and **Tree view**.

   Deleting public address ranges associated with virtual network interfaces removes all IPv6 addresses assigned to those interfaces from the public address ranges.
   {: important}

1. Select the checkbox to confirm: *"I acknowledge that deleting this IPv6 authorized CIDR also deletes all associated resources."*

   A warning is displayed: *"Are you sure you want to proceed? This action can't be undone. All associated resources are deleted with this resource."*
   {: note}

1. Click **Delete all**.

The panel shows deletion progress with a **Deleting resources** progress bar and a counter showing how many resources have been deleted. Resources move from a **Resources being deleted** table to a **Deleted resources** table as each one completes. When all resources are deleted, a confirmation message states *"IPv6 authorized CIDR (name) and all associated resources have been deleted."* Click **Close** to close the panel.

You can also delete an authorized CIDR from its details page by clicking **Actions** > **Delete**.
{: tip}

## Creating an IPv6 authorized CIDR from the CLI
{: #ipv6-par-authorized-cidr-create-cli}
{: cli}

To create an IPv6 authorized CIDR from the command line, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your CLI environment:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Run the following command:

   ```sh
   ibmcloud is public-address-range-authorized-cidr-create --zone ZONE_NAME [--name NAME] [--output JSON] [-q, --quiet]
   ```
   {: pre}

   Where:

   `--zone`
   :   The zone in which to create the IPv6 authorized CIDR, for example `us-south-1`. Required.

   `--name`
   :   A name for the authorized CIDR. Names beginning with `ibm-` are reserved. Optional.

   `--output`
   :   Output format. One of: JSON.

   `-q, --quiet`
   :   Suppress verbose output.

### Command examples
{: #ipv6-par-authorized-cidr-cli-examples}

Create an IPv6 authorized CIDR in `us-south-1`:

```sh
ibmcloud is public-address-range-authorized-cidr-create --zone us-south-1 --name my-ipv6-auth-cidr
```
{: pre}

List all IPv6 authorized CIDRs:

```sh
ibmcloud is public-address-range-authorized-cidrs
```
{: pre}

Rename an authorized CIDR:

```sh
ibmcloud is public-address-range-authorized-cidr-update my-ipv6-auth-cidr --name my-renamed-auth-cidr
```
{: pre}

Delete an authorized CIDR (only when `allocation.count` is `0`):

```sh
ibmcloud is public-address-range-authorized-cidr-delete my-ipv6-auth-cidr
```
{: pre}

## Creating an IPv6 authorized CIDR with the API
{: #ipv6-par-authorized-cidr-create-api}
{: api}

Before you begin, set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
{: requirement}

To create an IPv6 authorized CIDR, run the following command:

```sh
curl -sX POST \
  "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -H "Content-Type: application/json" \
  -d '{
    "availability_mode": "zonal",
    "ip_version": "ipv6",
    "name": "my-ipv6-auth-cidr",
    "network_prefix_length": 64,
    "zone": { "name": "us-south-1" }
  }'
```
{: pre}

The response includes the allocated `cidr` field, for example `"cidr": "2001:db8:85a3::/64"`. Note the `id` value. You need it to create public address ranges from this authorized CIDR.

After creation, only the `name` property can be updated:

```sh
curl -sX PATCH \
  "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$auth_cidr_id?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -H "Content-Type: application/json" \
  -d '{ "name": "my-renamed-auth-cidr" }'
```
{: pre}

To delete an authorized CIDR, first ensure that no public address ranges are allocated from it, then run:

```sh
curl -sX DELETE \
  "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$auth_cidr_id?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

To view the allocations from an authorized CIDR and see how much address space remains:

```sh
curl -sX GET \
  "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$auth_cidr_id/allocations?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

## Creating an IPv6 authorized CIDR with Terraform
{: #ipv6-par-authorized-cidr-create-terraform}
{: terraform}

To create an IPv6 authorized CIDR with Terraform, use the `ibm_is_public_address_range_authorized_cidr` resource:

```terraform
resource "ibm_is_public_address_range_authorized_cidr" "my_ipv6_auth_cidr" {
  availability_mode     = "zonal"
  ip_version            = "ipv6"
  name                  = "my-ipv6-auth-cidr"
  network_prefix_length = 64
  zone {
    name = "us-south-1"
  }
}
```
{: codeblock}

To read the details of an existing authorized CIDR:

```terraform
data "ibm_is_public_address_range_authorized_cidr" "example" {
  public_address_range_authorized_cidr_id = var.authorized_cidr_id
}
```
{: codeblock}

To list all authorized CIDRs:

```terraform
data "ibm_is_public_address_range_authorized_cidrs" "all_cidrs" {}
```
{: codeblock}

## Related links
{: #ipv6-par-authorized-cidr-related-links}

- [About public IPv6 address ranges](/docs/vpc?topic=vpc-ipv6-par-about)
- [Creating IPv6 public address ranges and binding to a VNI](/docs/vpc?topic=vpc-ipv6-par-creating-binding)
- [IAM roles and actions](/docs/iam?topic=iam-iam-service-roles-actions#is.public-address-range-roles)
- [Quotas](/docs/vpc?topic=vpc-quotas#par-quotas) and [service limits](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services)
- [FAQ for public address ranges](/docs/vpc?topic=vpc-faq-public-address-ranges)
