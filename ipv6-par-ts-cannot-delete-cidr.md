---

copyright:
  years: 2026
lastupdated: "2026-10-28"

keywords: troubleshoot, public address range, ipv6, authorized cidr, cannot delete, allocation count

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I delete an IPv6 authorized CIDR?
{: #troubleshoot-ipv6-par-cannot-delete-cidr}
{: troubleshoot}
{: support}

[Public IPv6 address ranges](/docs/vpc?topic=vpc-release-notes&interface=ui#vpc-oct2726) is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

Attempting to delete an IPv6 authorized CIDR returns an error, or the delete operation fails.
{: shortdesc}

Attempting to delete an IPv6 authorized CIDR returns an error, or the delete operation fails.
{: tsSymptoms}

An authorized CIDR can be deleted only when its `allocation.count` is `0`. If any IPv6 public address ranges are still allocated from the authorized CIDR, whether bound to a VNI or not, the delete operation is blocked.
{: tsCauses}

Complete the following steps:
{: tsResolve}

1. List all IPv6 public address ranges allocated from this authorized CIDR and confirm they exist.

   - **Console**: Go to **Infrastructure** > **Network** > **Public address ranges**, then click the **IPv6 authorized CIDRs** tab. Click the name of the authorized CIDR and review the **Allocated resources** tab for all associated public address ranges.
   - **CLI**:

     ```sh
     ibmcloud is public-address-ranges --output JSON | \
       jq '.[] | select(.authorized_cidr.id == "AUTHORIZED_CIDR_ID") | {id, name, cidr, lifecycle_state}'
     ```
     {: pre}

   - **API**:

     ```sh
     curl -sX GET \
       "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$auth_cidr_id/allocations?version=$api_version&generation=2" \
       -H "Authorization: Bearer $iam_token"
     ```
     {: pre}

1. For each public address range allocated from the authorized CIDR, unbind it from any VNI (if bound) and then delete it.

   - **Console**: On the **Allocated resources** tab, click the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") for each range and select **Unbind**, then **Delete**.
   - **CLI**:

     ```sh
     ibmcloud is public-address-range-update PAR_ID --reset-target
     ibmcloud is public-address-range-delete PAR_ID
     ```
     {: pre}

   - **API**: First unbind, then delete:

     ```sh
     curl -sX PATCH \
       "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
       -H "Authorization: Bearer $iam_token" \
       -H "Content-Type: application/json" \
       -d '{"target": null}'
     curl -sX DELETE \
       "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
       -H "Authorization: Bearer $iam_token"
     ```
     {: pre}

1. Confirm that the `allocation.count` for the authorized CIDR is `0` before proceeding.

   - **Console**: Refresh the **Allocated resources** tab. When the table is empty, the count is `0`.
   - **CLI**:

     ```sh
     ibmcloud is public-address-range-authorized-cidr AUTHORIZED_CIDR_ID --output JSON | jq .allocation.count
     ```
     {: pre}

   - **API**:

     ```sh
     curl -sX GET \
       "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$auth_cidr_id?version=$api_version&generation=2" \
       -H "Authorization: Bearer $iam_token"
     ```
     {: pre}

     Check the `allocation.count` field in the response. The value must be `0` before deletion is permitted.

1. Delete the authorized CIDR.

   - **Console**: Click **Actions** > **Delete** on the authorized CIDR row or details page. Confirm the deletion in the panel that opens.
   - **CLI**:

     ```sh
     ibmcloud is public-address-range-authorized-cidr-delete AUTHORIZED_CIDR_ID
     ```
     {: pre}

   - **API**:

     ```sh
     curl -sX DELETE \
       "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$auth_cidr_id?version=$api_version&generation=2" \
       -H "Authorization: Bearer $iam_token"
     ```
     {: pre}

## Related links
{: #ipv6-par-ts-cannot-delete-cidr-related}

- [About public IPv6 address ranges](/docs/vpc?topic=vpc-ipv6-par-about)
- [Creating and managing IPv6 authorized CIDRs](/docs/vpc?topic=vpc-ipv6-par-authorized-cidr)
- [Creating IPv6 public address ranges and binding to a VNI](/docs/vpc?topic=vpc-ipv6-par-creating-binding)
- [Known issues for public address ranges](/docs/vpc?topic=vpc-par-known-issues)
