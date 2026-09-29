---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: byoip, custom authorized CIDR, bring your own IP, public address range

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning a custom authorized CIDR
{: #provision-custom-authorized-cidr}

[IPv4 BYOIP is an allowlisted beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You can provision a custom authorized CIDR to bring your own publicly routable IPv4 address range into {{site.data.keyword.vpc_short}}. By using your own IP ranges, you can retain your established IP reputation and continue to pass through externally controlled allowlists. A custom authorized CIDR acts as a pool of IP addresses that you can subdivide and allocate to public address ranges.
{: shortdesc}

Customer-owned (BYOIP) authorized CIDRs are regional. When you provision a CIDR, you specify a region, and that CIDR can be used only to allocate resources within that region. To use the same IP range in a different region, you must open a new support case to deprovision it from the current region and provision it in the new region.
{: important}

## Before you begin
{: #before-you-begin-byoip}

Provisioning a custom authorized CIDR requires an IBM Support case. If you bring your own IP (BYOIP), you must also provide a signed Letter of Authorization (LoA); IBM-assigned CIDRs do not require an LoA. IBM reviews the request and provisions the CIDR in your account. The provisioning process typically takes a minimum of two weeks.

Before you request provisioning of a custom authorized CIDR, review the following requirements and constraints:

- A custom authorized CIDR can be provisioned only through an IBM Support case. You cannot provision an authorized CIDR yourself. To request provisioning, see [Provisioning a custom authorized CIDR](/docs/vpc?topic=vpc-provision-custom-authorized-cidr&interface=ui).
- You must own the public IPv4 address range that you want to use.
- The CIDR block must be within the supported size range: minimum `/24`, maximum `/8`.
- The CIDR block must not overlap with other address ranges that are already in use in your account.
- The authorized CIDR address space is allocated in blocks when you create public address ranges or floating IPs from it. You cannot create sub-authorized CIDRs; the CIDR itself is the single pool from which allocations are made. Size the address space appropriately for your workload needs.
- Each account can have up to five authorized CIDRs per region.
- An authorized CIDR cannot be moved from one region to another. To use the same IP range in a different region, you must open a new support case to deprovision it from the current region and then provision it in the new region.
- Authorized CIDRs are cloud resources and have a Cloud Resource Name (CRN) that includes `public-address-range-authorized-cidr`. They are onboarded to the Resource Controller and Global Search, and can be tagged and searched.
- To allocate a public address range from an authorized CIDR, the user must have the `is.public-address-range-authorized-cidr.authorized-cidr.operate` IAM action.

## Provisioning a custom authorized CIDR in the console
{: #provision-custom-authorized-cidr-console}
{: ui}

To request provisioning of a custom authorized CIDR:

1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Click the **Custom authorized CIDRs (BYOIP)** tab.
1. Click **Provision custom CIDR**.
1. In the **Provision custom CIDR** dialog, review the information about what is required for the request, then click **Create support case**. The system redirects you to the Support Center.

   If you bring your own IP (BYOIP), ensure that you have a signed Letter of Authorization (LoA) on company letterhead ready to attach. Click **Download template** in the dialog to obtain the LoA template, or see [Template: Letter of Authorization for IBM to announce IPv4 prefixes](/docs/vpc?topic=vpc-ipv4-letter-of-authorization). An LoA is not required for IBM-assigned CIDRs.
   {: tip}

   The provisioning process can take at least two weeks. Track progress in your support case.
   {: note}

1. In the support case form, complete the following fields.

   The fields differ depending on your support plan:

   - **Platinum, Premium, or Advanced support plan**: Select **Category**: `Account`, **Topic**: `Account`, **Subtopic**: `Bring your own IP to VPC`.
   - **Basic support plan**: Under **Report an issue**, select **Topic**: `Virtual Private Cloud (VPC)`, **Subtopic**: `Bring your own IP to VPC`. A two-way support case is opened to process your request.

   Then complete the remaining fields:
   - **Subject**: Enter `Authorize custom CIDR (BYOIP)`
   - **Description**: Include the following details:
     - Select **Provision CIDR** to indicate the request type.
     - For the version, enter **IPv4**.
     - For CIDR ownership, enter **Customer** or **IBM**.
     - Specify the destination region where the CIDR must be provisioned.
     - Add any additional information relevant to your request.
   - **Attachments**: If the CIDR or CIDRs are customer-provided, attach your signed LoA file (for example, `LOA_BYOIP_company.docx`)
1. Complete any other optional information, then review the case summary and click **Submit case**.

After IBM approves your request and provisions the CIDR, it appears in the **Custom authorized CIDRs (BYOIP)** tab. You can then create public address ranges or reserve floating IPs from it.

For public address ranges, you must configure ingress routing before traffic can reach your resources. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
{: important}

## Next steps
{: #related-links-byoip}
{: ui}

* [View a custom authorized CIDR](/docs/vpc?topic=vpc-view-custom-authorized-cidr&interface=ui).
* [Reserve a floating IP from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=ui).
* [Deprovision a custom authorized CIDR](/docs/vpc?topic=vpc-deprovision-custom-authorized-cidr&interface=ui).
* [Prepare a Letter of Authorization](/docs/vpc?topic=vpc-ipv4-letter-of-authorization).

## Provisioning a custom authorized CIDR from the CLI
{: #provision-custom-authorized-cidr-cli}
{: cli}

Before you can use the CLI, you must install the {{site.data.keyword.cloud_notm}} CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

Before you can work with a custom authorized CIDR in the CLI, you must open an IBM Support case through the Public address range for VPC UI. If you bring your own IP (BYOIP), you must also attach a signed [Letter of Authorization (LoA)](/docs/vpc?topic=vpc-ipv4-letter-of-authorization). For more information, see [Provisioning a custom authorized CIDR in the console](/docs/vpc?topic=vpc-provision-custom-authorized-cidr&interface=ui#provision-custom-authorized-cidr-console). IBM reviews the request and provisions the CIDR in your account.
{: important}

After IBM provisions the custom authorized CIDR, you can verify that it is available in your account by running the following command:

```sh
ibmcloud is public-address-range-authorized-cidrs
```
{: pre}

For more information about viewing authorized CIDRs, see [Viewing a custom authorized CIDR](/docs/vpc?topic=vpc-view-custom-authorized-cidr&interface=cli).

You can then create public address ranges or reserve floating IPs from the CIDR. Specify a CIDR block from the authorized CIDR when you run `ibmcloud is public-address-range-create --cidr CIDR`. For more information, see [Creating a public address range](/docs/vpc?topic=vpc-par-creating&interface=cli). You can also reserve floating IPs from the CIDR by using `ibmcloud is floating-ip-reserve --address ADDRESS`. For more information, see [Reserving floating IPs from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=cli).

### Next steps
{: #related-links-byoip-cli}
{: cli}

* [View a custom authorized CIDR](/docs/vpc?topic=vpc-view-custom-authorized-cidr&interface=cli).
* [Reserve a floating IP from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=cli).
* [Deprovision a custom authorized CIDR](/docs/vpc?topic=vpc-deprovision-custom-authorized-cidr&interface=cli).
* [Prepare a Letter of Authorization](/docs/vpc?topic=vpc-ipv4-letter-of-authorization).

## Provisioning a custom authorized CIDR with the API
{: #provision-custom-authorized-cidr-api}
{: api}

Before you begin, set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

Before you can work with a custom authorized CIDR using the API, you must open an IBM Support case through the Public address range for VPC UI. If you bring your own IP (BYOIP), you must also attach a signed [Letter of Authorization (LoA)](/docs/vpc?topic=vpc-ipv4-letter-of-authorization). For more information, see [Provisioning a custom authorized CIDR in the console](/docs/vpc?topic=vpc-provision-custom-authorized-cidr&interface=ui#provision-custom-authorized-cidr-console). IBM reviews the request and provisions the CIDR in your account.
{: important}

After IBM provisions the CIDR, you can list all authorized CIDRs in your account by sending a `GET` request:

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

### Using your CIDR with the API
{: #using-your-cidr-api}

After IBM provisions the custom authorized CIDR, you can create public address ranges or reserve floating IPs from it. Send a `POST` request to `/v1/public_address_ranges` and specify the `cidr` field with a CIDR block from the authorized CIDR. For more information, see [Creating a public address range](/docs/vpc?topic=vpc-par-creating&interface=api). You can also reserve floating IPs from the CIDR by specifying an `address` from your range. For more information, see [Reserving floating IPs from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=api).

Public address ranges created from a custom authorized CIDR require ingress routing to be configured before traffic can reach your resources. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

### Next steps
{: #related-links-byoip-api}
{: api}

* [View a custom authorized CIDR](/docs/vpc?topic=vpc-view-custom-authorized-cidr&interface=api).
* [Reserve a floating IP from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=api).
* [Deprovision a custom authorized CIDR](/docs/vpc?topic=vpc-deprovision-custom-authorized-cidr&interface=api).
* [Prepare a Letter of Authorization](/docs/vpc?topic=vpc-ipv4-letter-of-authorization).

## Provisioning a custom authorized CIDR with Terraform
{: #provision-custom-authorized-cidr-terraform}
{: terraform}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).

Before you can work with a custom authorized CIDR using Terraform, you must open an IBM Support case through the Public address range for VPC UI. If you bring your own IP (BYOIP), you must also attach a signed [Letter of Authorization (LoA)](/docs/vpc?topic=vpc-ipv4-letter-of-authorization). For more information, see [Provisioning a custom authorized CIDR in the console](/docs/vpc?topic=vpc-provision-custom-authorized-cidr&interface=ui#provision-custom-authorized-cidr-console). IBM reviews the request and provisions the CIDR in your account.
{: important}

After IBM provisions the CIDR, you can retrieve all authorized CIDRs in your account by using the `ibm_is_public_address_range_authorized_cidrs` data source:

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

### Using your CIDR with Terraform
{: #using-your-cidr-terraform}

After IBM provisions the custom authorized CIDR, you can create public address ranges or reserve floating IPs from it. Use the `ibm_is_public_address_range` resource and specify the `cidr` argument with a CIDR block from the authorized CIDR. For more information, see [Creating a public address range](/docs/vpc?topic=vpc-par-creating&interface=terraform). You can also reserve floating IPs from the CIDR by using the `ibm_is_floating_ip` resource with the `address` argument. For more information, see [Reserving floating IPs from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=terraform).

Public address ranges created from a custom authorized CIDR require ingress routing to be configured before traffic can reach your resources. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

For more information about the Terraform data sources for authorized CIDRs, see the [Terraform registry documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_public_address_range_authorized_cidrs){: external}.

### Next steps
{: #related-links-byoip-terraform}
{: terraform}

* [View a custom authorized CIDR](/docs/vpc?topic=vpc-view-custom-authorized-cidr&interface=terraform).
* [Reserve a floating IP from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create&interface=terraform).
* [Deprovision a custom authorized CIDR](/docs/vpc?topic=vpc-deprovision-custom-authorized-cidr&interface=terraform).
* [Prepare a Letter of Authorization](/docs/vpc?topic=vpc-ipv4-letter-of-authorization).
