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

[IPv4 BYOIP is an allowlisted beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

After IBM approves your provisioning request, view your custom authorized CIDR to monitor how the address space is being used.
{: shortdesc}

## Viewing authorized CIDRs in the console
{: #view-custom-authorized-cidr-console}
{: ui}

After IBM approves your request and provisions the CIDR, you can find it on the **Custom authorized CIDRs (BYOIP)** tab. The table shows the CIDR name, IP range, IP version, availability mode (regional or zonal), and allocated resources.

To view details for a specific CIDR, click the CIDR name in the table. The details page includes two tabs:

### Viewing CIDR details and utilization
{: #view-cidr-overview-tab}

On the **Overview** tab, you can review the following information:

- **CIDR details**: Name, ID, IP range, IP version, availability mode, location, type (user-provided), and created date.
- **CIDR utilization**: Address space (for example, `192.168.0.0 to 192.168.0.256`), available addresses, space used, and allocated resources. The graphical view shows how the address space is divided across floating IPs, public address ranges, and available IPs.

Click **View allocated resources** to review the resources allocated from the CIDR.

### Viewing allocated resources
{: #view-cidr-allocated-resources-tab}

On the **Allocated resources** tab, you can see all resources that are allocated from the authorized CIDR, and available (unallocated) address space. To filter by resource type, use the **Resource type** menu (options: All, Floating IP, Public address range).

The following table describes each column:

| Column | Description |
| --- | --- |
| Resource | The name of the allocated resource, or "Available" for unallocated addresses |
| CIDR or IP | The CIDR block for public address ranges, the IP address for floating IPs, or the unallocated address block for available address space |
| Status | Bound, Unbound, or Available |
| Resource type | Floating IP, Public address range, or blank for available addresses |
| Targeted resource | The VNI, instance, or VPC that the resource is attached to (if bound) |
| Target type | Virtual server instance, Virtual network interface, or other target type |
{: caption="Allocated resources table columns" caption-side="bottom"}

### Creating resources from the allocated resources table
{: #create-resources-from-cidr-details}

You can reserve floating IPs and create public address ranges directly from the CIDR details page:

Click **Create** at the top of the page and select **Floating IP** or **Public address range**. The Reserve floating IP page opens with the authorized CIDR pre-selected.

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

## Viewing authorized CIDRs with Terraform
{: #view-custom-authorized-cidr-terraform}
{: terraform}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).

To retrieve all authorized CIDRs in your account, use the `ibm_is_public_address_range_authorized_cidrs` data source:

```terraform
data "ibm_is_public_address_range_authorized_cidrs" "example" {
}
```
{: codeblock}

To retrieve details of a specific authorized CIDR, use the `ibm_is_public_address_range_authorized_cidr` data source:

```terraform
data "ibm_is_public_address_range_authorized_cidr" "example" {
  authorized_cidr_id = var.authorized_cidr_id
}
```
{: codeblock}

## Next steps
{: #related-links-view-cidr}

* [Reserve a floating IP from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=ui).
* [Deprovision a custom authorized CIDR](/docs/vpc?topic=vpc-deprovision-custom-authorized-cidr&interface=ui).
* [Prepare a Letter of Authorization](/docs/vpc?topic=vpc-ipv4-letter-of-authorization).
