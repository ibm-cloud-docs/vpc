---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: BYOIP, view, custom authorized CIDR, CIDR utilization, bring your own IP

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing custom authorized CIDRs
{: #view-custom-authorized-cidr}

[BYOIP for IPv4 is a beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

After IBM approves your provisioning request, view your custom authorized CIDR to monitor how the address space is being used.
{: shortdesc}

## Viewing authorized CIDRs in the console
{: #view-custom-authorized-cidr-console}
{: ui}

To view custom authorized CIDRs in the console:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Click the **Custom authorized CIDRs (BYOIP)** tab.

The **Custom authorized ranges** table displays the following information:

| Column | Description |
| --- | --- |
| Name | The name of the authorized CIDR. Click the name to open the details page. |
| CIDR | The authorized IP prefix (for example, `46.16.185.48/28`). |
| IP version | The IP version (for example, `IPv4`). |
| Availability mode | The availability scope: `Regional` (customer-provided BYOIP) or `Zonal` (IBM Classic migrated). |
| Allocated resources | The number of resources currently allocated from this authorized CIDR. |
{: caption="Custom authorized ranges table columns" caption-side="bottom"}

To view details for a specific CIDR, click the CIDR name in the table. The details page includes two tabs:

### Viewing CIDR details
{: #view-cidr-overview-tab}

On the **Overview** tab, you can review the following information in the **CIDR details** section:

- **Name**: The name of the custom authorized CIDR.
- **ID**: The unique identifier of the authorized CIDR. Click the copy icon to copy the ID to your clipboard.
- **IP range**: The CIDR block allocated for this authorized range.
- **IP version**: The IP version (`IPv4`).
- **Resource group**: The resource group assigned to this authorized CIDR.
- **Availability mode**: `Regional` or `Zonal`.
- **Location**: The region (for example, `us-south`) or zone where the CIDR is provisioned.
- **Type**: The CIDR ownership type (for example, `User-provided` or `IBM-provided`).
- **CRN**: The Cloud Resource Name for the authorized CIDR. Click the copy icon to copy the CRN to your clipboard.

To deprovision the authorized CIDR, click the **Actions...** menu in the header and select **Deprovision**.

### Viewing allocated resources
{: #view-cidr-allocated-resources-tab}

On the **Allocated resources** tab, you can view all resources allocated from the authorized CIDR.

Use the **Resource type** dropdown filter (options: `All`, `Floating IP`, `Public address range`) or the search field to filter the list of allocated resources.

The table displays the following columns:

| Column | Description |
| --- | --- |
| Resource | The name of the allocated resource (for example, floating IP or public address range). |
| CIDR or IP | The individual IP address (for floating IPs) or the CIDR block (for public address ranges). |
| Status | The binding status (`Bound` with a green indicator or `Unbound` with a yellow warning indicator). |
| Resource type | The type of resource allocated (`Floating IP` or `Public address range`). |
| Targeted resource | The name of the targeted resource (such as a VNI or VPC) that the resource is attached to, or `—` if unbound. |
| Target type | The target type (such as `Virtual network interface`), or `—` if unbound. |
{: caption="Allocated resources table columns" caption-side="bottom"}

### Creating resources from the authorized CIDR
{: #create-resources-from-cidr-details}

You can reserve floating IPs and create public address ranges directly from the **Allocated resources** tab:

1. Click **Create** on the **Allocated resources** tab.
1. Select **Floating IP** or **Public address range**. The provisioning page opens with the custom authorized CIDR pre-selected.

This flow applies to both regional (customer-brought) CIDRs and zonal (IBM Classic migrated) CIDRs. For zonal CIDRs, only the zone that is associated with the CIDR is available during creation.
{: note}

For more information, see [Creating a public address range](/docs/vpc?topic=vpc-par-creating&interface=ui) or [Creating floating IPs from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=ui).

Public address ranges created from a custom authorized CIDR require ingress routing to be configured before traffic can reach your resources. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

## Viewing authorized CIDRs from the CLI
{: #view-custom-authorized-cidr-cli}
{: cli}

Before you can use the CLI, you must install the {{site.data.keyword.cloud_notm}} CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

To list all authorized CIDRs in your account, run the following command:

```sh
ibmcloud is public-address-range-authorized-cidrs
```
{: pre}

You can filter the list by allocation profile family or availability mode:

```sh
ibmcloud is public-address-range-authorized-cidrs --allocated-profile-family user
```
{: pre}

```sh
ibmcloud is public-address-range-authorized-cidrs --availability-mode regional --output JSON
```
{: pre}

Where:

`--allocated-profile-family`
:   Filters the collection to resources with a matching allocation profile family. One of: `provider`, `user`.

`--availability-mode`
:   Filters the collection to resources with a matching availability mode. One of: `regional`, `zonal`.

To view details of a specific authorized CIDR, run the following command:

```sh
ibmcloud is public-address-range-authorized-cidr AUTHORIZED_CIDR
```
{: pre}

Where `AUTHORIZED_CIDR` is the ID or name of the authorized CIDR.

To list all allocations for an authorized CIDR, run the following command:

```sh
ibmcloud is public-address-range-authorized-cidr-allocations AUTHORIZED_CIDR
```
{: pre}

You can filter allocations by resource type:

```sh
ibmcloud is public-address-range-authorized-cidr-allocations AUTHORIZED_CIDR --allocations-resource-type floating_ip
```
{: pre}

To view details of a specific allocation, run the following command:

```sh
ibmcloud is public-address-range-authorized-cidr-allocation AUTHORIZED_CIDR --allocation ALLOCATION_ID
```
{: pre}

Where `ALLOCATION_ID` is the ID of the allocation.

## Viewing authorized CIDRs with the API
{: #view-custom-authorized-cidr-api}
{: api}

Before you begin, set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

To list all authorized CIDRs in your account, send a `GET` request:

```sh
curl -sX GET "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

You can filter the list by allocation profile family or availability mode:

```sh
curl -sX GET "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs?version=$api_version&generation=2&allocation.profile_family=user" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

```sh
curl -sX GET "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs?version=$api_version&generation=2&availability_mode=regional" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

To view details of a specific authorized CIDR, send a `GET` request with the CIDR ID:

```sh
curl -sX GET "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$authorized_cidr_id?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

To list all allocations for an authorized CIDR, send a `GET` request to the allocations endpoint:

```sh
curl -sX GET "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$authorized_cidr_id/allocations?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

You can filter allocations by resource type:

```sh
curl -sX GET "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$authorized_cidr_id/allocations?version=$api_version&generation=2&allocations[].resource_type=floating_ip" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

To view details of a specific allocation, send a `GET` request with the allocation ID:

```sh
curl -sX GET "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$authorized_cidr_id/allocations/$allocation_id?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}



## Next steps
{: #related-links-view-cidr}

* [Reserve a floating IP from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=ui).
* [Deprovision a custom authorized CIDR](/docs/vpc?topic=vpc-deprovision-custom-authorized-cidr&interface=ui).
* [Prepare a Letter of Authorization](/docs/vpc?topic=vpc-ipv4-letter-of-authorization).
