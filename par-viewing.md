---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: viewing, deleting, public address range

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing public address ranges
{: #par-viewing}

You can view public address ranges with the console, CLI, API, and Terraform.
{: shortdesc}

## Viewing public address ranges in the console
{: #view-par-ui}
{: ui}

To view public address ranges in the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.

The Public address ranges for VPC page shows the following information:

- **Name:** The name of the public address range object.
- **Status:** States whether the address range is bound or unbound to the specified VPC.
- **Lifecycle state:** States whether the binding or unbinding of the address range is successful, and if it is stable or not.
- **IP range:** The range of IP addresses included in the address range.[BYOIP (IPv4) Beta]{: tag-cyan}

   Public address ranges that were created from a custom authorized CIDR display an icon indicating "Created from an authorized CIDR".
   {: note}


- **Zone:** States the zone that the address range is bound to (if applicable).
- **Target resource:** States the VPC that the address range is bound to in the previous zone (if applicable).

[BYOIP (IPv4) Beta]{: tag-cyan}To view details of a public address range, click the name to open the details page. The details page shows:

- Name, Resource group, Location, IP range, Size, IP version, VPC, ID, CRN, Created date, and Lifecycle state.
- Authorized CIDR: If the public address range was created from a custom authorized CIDR, the linked name of the authorized CIDR is displayed. Click the link to navigate to the CIDR details page.



## Viewing public address ranges from the CLI
{: #par-view-cli}
{: cli}

To view public address ranges from the command line, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your CLI environments. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Run the following command:

   
   ```sh
   ibmcloud is public-address-range PUBLIC_ADDRESS_RANGE [--output JSON] [-q, --quiet]
   ```
   [BYOIP (IPv4) Beta]{: tag-cyan}

   ```sh
   ibmcloud is public-address-ranges [--profile-name PROFILE_NAME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups] [--output JSON] [-q, --quiet]
   ibmcloud is public-address-range PUBLIC_ADDRESS_RANGE [--output JSON] [-q, --quiet]
   ```
   {: pre}

   
   

   Where:

   `PUBLIC_ADDRESS_RANGE`
   :   The ID or name of the public address range to view.

   [BYOIP (IPv4) Beta]{: tag-cyan}

   `--profile-name`
   :   Filter the list by profile name. Use this option to view only public address ranges from IBM-managed IP pools or only those from custom authorized CIDRs.

   `--resource-group-id`
   :   Filter by resource group ID. Mutually exclusive with `--resource-group-name`.

   `--resource-group-name`
   :   Filter by resource group name. Mutually exclusive with `--resource-group-id`.

   `--all-resource-groups`
   :   Query all resource groups.

   

   `--output`
   :   The output format, only JSON is supported. One of: JSON.

   `-q, --quiet`
   :   Suppress verbose output.
   

### Command examples
{: #par-cli-view-examples}

View the public address range `$par-id`:

```sh
ibmcloud is public-address-range $par-id
```
{: pre}



View all public address ranges:

```sh
ibmcloud is public-address-ranges
```
{: pre}



[BYOIP (IPv4) Beta]{: tag-cyan}Filter public address ranges by profile name:

```sh
ibmcloud is public-address-ranges --profile-name provider-ipv4
```
{: pre}






## Viewing public address ranges with the API
{: #par-view-api}
{: api}

Before you begin, set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
{: requirement}

Select one of the following options:

* View all public address ranges for an account:

   ```sh
   curl -sX GET \
            "$vpc_api_endpoint/v1/public_address_ranges?version=$api_version&generation=2" \
            -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

[BYOIP (IPv4) Beta]{: tag-cyan}
* Filter by profile name to view only custom authorized CIDR-based ranges:

   ```sh
   curl -sX GET \
            "$vpc_api_endpoint/v1/public_address_ranges?version=$api_version&generation=2&profile.name=public-address-range-user-ipv4" \
            -H "Authorization: Bearer $iam_token"
   ```
   {: pre}





* View a specific public address range:

   ```sh
   curl -sX GET \
            "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
            -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

## Viewing public address ranges with Terraform
{: #par-view-terraform}
{: terraform}

To view a public address range with Terraform, use the following example:

```terraform

data "ibm_is_public_address_range" "public_address_range_instance" {
  name = "example-public-address-range"
}
data "ibm_is_public_address_range" "public_address_range_instance1" {
  # name = "example-public-address-range"
  identifier =  ibm_is_public_address_range.public_address_range_instance.id
}
```
{: codeblock}

To get a list of public address ranges:

```terraform
data "ibm_is_public_address_ranges" "public_address_range_instances_example_testing" {
}
```
{: pre}

## Next steps
{: #after-view-par}

* [Delete a public address range](/docs/vpc?topic=vpc-par-deleting).
