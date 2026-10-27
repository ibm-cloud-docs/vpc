---

copyright:
  years: 2026
lastupdated: "2026-10-27"

keywords: public address range, bind, unbind

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Binding, unbinding, and moving public address ranges
{: #par-unbinding-binding}

You can bind, unbind, and move public address ranges to a VPC in an availability zone with the console, CLI, API, and Terraform.[Beta]{: tag-cyan} IPv6 public address ranges bind to a virtual network interface (VNI). For IPv6 binding instructions, see [Creating IPv6 public address ranges and binding to a VNI](/docs/vpc?topic=vpc-ipv6-par-creating-binding).
{: shortdesc}

## Before you begin
{: #before-you-begin-binding-public-address-range}

Review the following requirements and considerations before binding, unbinding, or moving a public address range:

* Make sure to review [planning considerations](/docs/vpc?topic=vpc-par-planning) for public address ranges.
* The binding of a public address range must include both a VPC and an availability zone.[Beta]{: tag-cyan} For IPv6 public address ranges, the binding target is a VNI, not a VPC. The VNI must be in the same zone as the authorized CIDR the range was allocated from.
* When using a public address range, you must [configure ingress routing](/docs/vpc?topic=vpc-about-custom-routes) after binding the address range to direct traffic to the appropriate resource. Binding the address range to a VPC does not automatically route traffic to the resource.[Beta]{: tag-cyan} IPv6 public address ranges do not require ingress routing. Traffic routes to the VNI automatically after binding.

## Binding, unbinding, and moving public address ranges in the console
{: #bind-unbind-par-ui}
{: ui}

You can bind, unbind, and move a public address range to a VPC in an availability zone in the console.

[Beta]{: tag-cyan}IPv6 public address ranges are bound to a VNI instead of a VPC. The **Bind public address range** and **Edit public address range** panels differ for IPv6, and the button is **Attach**. For the console steps, see [Binding an IPv6 public address range to a VNI](/docs/vpc?topic=vpc-ipv6-par-creating-binding#ipv6-par-binding).

### Binding a public address range in the console
{: #bind-par-ui}

To bind a public address range in the {{site.data.keyword.cloud}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**. The Public address ranges for VPC page is displayed.
1. Highlight the row of the address range in the table, then click **Bind** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. From the **Bind public address range** page, select the VPC and its corresponding availability zone where you want to bind the address range.
1. Click **Bind** to bind the public address range to the VPC.

### Unbinding a public address range in the console
{: #unbind-par-ui}

To unbind a public address range in the IBM Cloud console, follow these steps:

After a public address is unbound, it is still reserved and available to be bound again.
{: tip}

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**. The Public address ranges for VPC page is displayed.
1. Highlight the row of the address range in the table, then click **Unbind** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. Click **Unbind** to confirm that you want to unbind this address range from the VPC.

### Moving a public address range in the console
{: #moving-par-ui}

If the public address range is bound to a VPC in an availability zone, you can update and bind (move) it to a different VPC and availability zone. The public address range is automatically unbound from its VPC and availability zone and then bound to the newly specified VPC and availability zone.

You don't need to unbind the address range from its original target first.
{: tip}

To move a public address range in the console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**. The Public address ranges for VPC page is displayed.
1. Highlight the row of the address range in the table, then click **Edit** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. From the **Edit public address range** page, select the VPC and its availability zone where you want to move the address range.
1. Click **Save** to bind the public address range to the new VPC.

## Binding, unbinding, and moving public address ranges from the CLI
{: #par-binding-unbinding-cli}
{: cli}

To bind, unbind, or move a reserved IP address from the command line, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your CLI environments. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Run the following command:

   ```sh
   ibmcloud is public-address-range-update PUBLIC_ADDRESS_RANGE [--name NAME] ([--vpc VPC --zone ZONE] | [--reset-target]) [--output JSON] [-q, --quiet]
   ```

   [Beta]{: tag-cyan}

   ```sh
   ibmcloud is public-address-range-update PUBLIC_ADDRESS_RANGE [--name NAME] ([--vpc VPC --zone ZONE] | [--vni VNI] | [--reset-target]) [--output JSON] [-q, --quiet]
   ```
   {: pre}

   

   Where:

   `PUBLIC_ADDRESS_RANGE`
   :   The ID or name of the public address range to update.

   `--name`
   :   A new identifier for the public address range, if you want to rename it.

   `--vpc`
   :   The VPC you want to attach the public address range to. When specifying the `--vpc` option, `--zone` is required.[Beta]{: tag-cyan} Not valid for IPv6 public address ranges. Use `--vni` instead.

   `--zone`
   :   The zone where you want this public address range to reside. When specifying the `--zone` option, `--vpc` is required.[Beta]{: tag-cyan} Not valid for IPv6 public address ranges.

   [Beta]{: tag-cyan}
   `--vni`
   :   The ID or name of the VNI to bind an IPv6 public address range to. Mutually exclusive with `--vpc --zone`.
   

   `--reset-target`
   :   Unbind a public address range.

   `-f, --force`
   :   Force the operation without confirmation.

   `--output`
   :   The output format, only JSON is supported. One of: JSON.

   `-q, --quiet`
   :   Suppress verbose output.

### Command examples
{: #par-cli-updating-examples}

If a public address is not bound to a VPC, you can bind it to a VPC in any availability zone:

```sh
ibmcloud is public-address-range-update r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2 --name public-address-range-1 --vpc cli-test-vpc --zone us-south-1
```
{: pre}

Unbind a public address range from a VPC and leave it unbound:

```sh
ibmcloud is public-address-range-update r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2 --name public-address-range-1 --reset-target
```
{: pre}

Move a public address range from one VPC to another:

```sh
ibmcloud is public-address-range-update r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2 --vpc cli-test-vpc --zone us-south-2
```
{: pre}

[Beta]{: tag-cyan}Bind an IPv6 public address range to a VNI:

```sh
ibmcloud is public-address-range-update r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2 --vni my-vni
```
{: pre}

Move an IPv6 public address range to a different VNI (in the same zone):

```sh
ibmcloud is public-address-range-update r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2 --vni my-other-vni
```
{: pre}



## Binding, unbinding, and moving public address ranges with the API
{: #par-bind-unbind-api}
{: api}

To bind, unbind, or move public address ranges with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Store the following values in variables to be used in the API command:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, select one of the following actions:

   * Bind a public address range to a specific VPC:

      ```sh
        curl -X PATCH \
               "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
               -H "Authorization: Bearer $iam_token" \
               -H "Content-Type: application/json" \
               -d '{
                     "target": {
                        "vpc": {
                           "id": "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
                        },
                        "zone": {
                           "name": "us-south-2"
                        }
                     }
                  }'
      ```
      {: pre}

   [Beta]{: tag-cyan}
   * Bind an IPv6 public address range to a VNI:

      ```sh
        curl -X PATCH \
               "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
               -H "Authorization: Bearer $iam_token" \
               -H "Content-Type: application/json" \
               -d '{
                     "target": {
                        "virtual_network_interface": {
                           "id": "'$vni_id'"
                        }
                     }
                  }'
      ```
      {: pre}

   

   * Unbind a public address range from a specific VPC:

      ```sh
         curl -X PATCH \
               "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
               -H "Authorization: Bearer $iam_token" \
               -H "Content-Type: application/json" \
               -d '{
                     "target": null
                  }'
      ```
      {: pre}

      After a public address is unbound, it is still reserved and can be bound again.
      {: note}

   * Move a public address range from one VPC to another:

      ```sh
         curl -X PATCH \
               "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
               -H "Authorization: Bearer $iam_token" \
               -H "Content-Type: application/json" \
               -d '{
                     "target": {
                        "vpc": {
                           "id": "r006-9999aaaa-bbbb-cccc-dddd-1234567890ab"
                        },
                        "zone": {
                           "name": "us-south-3"
                        }
                     }
                  }'
      ```
      {: pre}

## Binding, unbinding, and moving public address ranges with Terraform
{: #par-bind-unbind-terraform}
{: terraform}

To bind, unbind, or move a public address range with Terraform, use the following example:

```terraform
resource "ibm_is_public_address_range" "public_address_range_instance" {
	ipv4_address_count = "16"
  name = "example-public-address-range"
  resource_group{
      id = "11caaa983d9c4beb82690daab08717e9"
  }
  target{
    vpc {
      id = ibm_is_vpc.testacc_vpc.id
    }
    zone {
      name = "us-south-3"
    }
  }
}
```
{: codeblock}

[Beta]{: tag-cyan}To bind an IPv6 public address range to a VNI, use `virtual_network_interface` in the `target` block:

```terraform
resource "ibm_is_public_address_range" "ipv6_bind_example" {
  name                  = "my-ipv6-par"
  network_prefix_length = 112
  authorized_cidr {
    id = ibm_is_public_address_range_authorized_cidr.example.id
  }
  target {
    virtual_network_interface {
      id = ibm_is_virtual_network_interface.example.id
    }
  }
}
```
{: codeblock}



## Next steps
{: #after-binding-par}

* [View public address ranges](/docs/vpc?topic=vpc-par-viewing).
* [Delete a public address range](/docs/vpc?topic=vpc-par-deleting).
