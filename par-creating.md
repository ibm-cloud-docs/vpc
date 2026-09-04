---

copyright:
  years: 2026
lastupdated: "2026-09-04"

keywords: public address range, create, bind

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating public address ranges
{: #par-creating}

You can create a public address range by defining its size and optionally, specifying a VPC to associate with it in any availability zone within the account.
{: shortdesc}

If the VPC is deleted while it is bound to a public address range, the address range continues to exist and can be bound later to a different VPC in any availability zone.
{: tip}

You can create public address ranges with the console, CLI, API, and Terraform.

## Before you begin
{: #par-before-you-begin}

Make sure to review [planning considerations](/docs/vpc?topic=vpc-par-planning) for public address ranges.

## Creating public address ranges in the console
{: #par-creating-ui}
{: ui}

To create public address ranges in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public Address Ranges**. The Public Address Ranges for VPC page appears.
1. Click **Create** to go to the provisioning page.
1. In the Location section, indicate the region where you want the public address range created.
1. In the Details section, complete the following information:


   * **Size**: Select the size (number of IPs) of the address range. You can choose from 1, 2, 4, 8, or 16 IPs to include in the block.

      After you create a public address range, you can't change its size.
      {: note}


   * **Name**: (Optional): Enter a name for the public address range, such as `my-public-address-range`.
   * **Resource group**: Select a resource group for the public address range.

      After you create a public address range, you can't change the resource group.
      {: note}

   * **Tags**: (Optional) Add tags to help you organize and find your resources. You can always add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).

1. (Optional) If you want to bind your public address range to an existing VPC, toggle the **Bind** switch to **On**. Then, select the VPC and availability zone that you want to bind to.

   You can bind a public address range to any VPC in a single availability zone that already exists in your account. You can also bind the address range to a VPC later. For more information, see [Binding a public address range](/docs/vpc?topic=vpc-par-unbinding-binding&interface=ui#bind-par-ui).
   {: tip}

1. Click **Create**. The public address range is requested for use.

On the Public address ranges for VPC page, your address range now shows in the table. For IBM Cloud services, the status of your public address range changes from `Updating` to `Stable`.

## Creating public address ranges from the CLI
{: #par-ordering-cli}
{: cli}

To create public address ranges from the command line, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your CLI environment. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Run the following command:

   ```sh
   ibmcloud is public-address-range-create --ipv4-address-count IPV4_ADDRESS_COUNT [--name NAME] [--vpc VPC --zone ZONE] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
   ```

   
   

   Where:

   `--ipv4-address-count`
   :   The total number of public IPv4 addresses required. Must be a power of 2.

   

   

   `--name`
   :   A name for the public address range. Names beginning with `ibm-` are reserved for provider-managed resources and are not allowed.

   `--vpc`
   :   The VPC to bind this public address range to. Requires `--zone`.

   `--zone`
   :   The zone where this public address range resides. Requires `--vpc`.

   

   `--resource-group-id`
   :   ID of the resource group. Mutually exclusive with `--resource-group-name`.

   `--resource-group-name`
   :   Name of the resource group. Mutually exclusive with `--resource-group-id`.

   `--output`
   :   The output format, only JSON is supported. One of: JSON.

   `-q, --quiet`
   :   Suppress verbose output.
   


### Command examples
{: #par-cli-create-examples}

Create a public address range named `public-address-range-1` with an address count of 8:

```sh
ibmcloud is public-address-range-create --name public-address-range-1 --ipv4-address-count 8
```
{: pre}

Create a public address range that is named `public-address-range-2` with an address count of 4 and a resource group named `Default`:

```sh
ibmcloud is public-address-range-create --name public-address-range-2 --ipv4-address-count 4 --resource-group-name Default
```
{: pre}

Create a public address range named `public-address-range-3` with an address count of 8, bound to VPC `cli-test-vpc` in zone `us-south-1` with resource group ID `72b27b5c-f4b0-48bb-b954-5becc7c1dcb3`:

```sh
ibmcloud is public-address-range-create --name public-address-range-3 --ipv4-address-count 8 --vpc cli-test-vpc --zone us-south-1 --resource-group-id 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3
```
{: pre}





## Creating public address ranges with the API
{: #par-ordering-api}
{: api}

Before you begin, set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
{: requirement}

Select one of the following options:

* Create a public address range from IBM-managed IPs, bound to a VPC:

   ```sh
   curl -sX POST \
   "$vpc_api_endpoint/v1/public_address_ranges?version=$api_version&generation=2" \
   -H "Authorization: Bearer $iam_token" \
   -H "Content-Type: application/json" \
   -d '{
         "ipv4_address_count": 8,
         "name": "my-public-address-range",
         "target": {
            "vpc": {
               "id": "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
            },
            "zone": {
               "name": "us-south-1"
            }
         }
      }'
   ```
   {: pre}

* Create an unbound public address range from IBM-managed IPs:

   ```sh
   curl -sX POST \
   "$vpc_api_endpoint/v1/public_address_ranges?version=$api_version&generation=2" \
   -H "Authorization: Bearer $iam_token" \
   -H "Content-Type: application/json" \
   -d '{
         "ipv4_address_count": 8,
         "name": "my-public-address-range"
      }'
   ```
   {: pre}





If you need to change the size of the address range or the resource group, you must delete and recreate the address range.
{: important}

## Creating public address ranges with Terraform
{: #par-create-terraform}
{: terraform}

To create a public address range with Terraform, use the following example:

```terraform
resource "ibm_is_public_address_range" "example" {
  ipv4_address_count = 8
  name               = "my-public-address-range"
  resource_group {
    id = data.ibm_resource_group.example.id
  }
  target {
    vpc {
      id = ibm_is_vpc.example.id
    }
    zone {
      name = "us-south-1"
    }
  }
}
```
{: codeblock}





## Related links
{: #after-create-par}

- [Getting started with public address ranges](/docs/vpc?topic=vpc-about-par)
- [Planning considerations for public address ranges](/docs/vpc?topic=vpc-par-planning)



- [IAM roles and actions](/docs/iam?topic=iam-iam-service-roles-actions#is.public-address-range-roles)
- [Quotas](/docs/vpc?topic=vpc-quotas#par-quotas) and [service limits](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services)
- [FAQ](/docs/vpc?topic=vpc-faq-public-address-ranges)
- [Known issues](/docs/vpc?topic=vpc-par-known-issues)
- [Troubleshooting](/docs/vpc?group=tbs-par)
