---

copyright:
  years: 2026
lastupdated: "2026-08-03"

keywords: viewing, deleting, public address range

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting public address ranges
{: #par-deleting}

You can delete public address ranges with the console, CLI, API, and Terraform.
{: shortdesc}

## Deleting public address ranges in the console
{: #delete-par-ui}
{: ui}

To delete public address ranges in the IBM Cloud console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**. The Public address ranges for VPC page appears.
1. Highlight the row of the address range in the table, then click **Delete** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. Click **Delete** to confirm that you want to delete this address range from the VPC.



## Deleting public address ranges from the CLI
{: #par-deleting-cli}
{: cli}

To delete public address ranges from the command line, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your CLI environments. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Enable the following feature flag:

   ```sh
   export IBMCLOUD_IS_FEATURE_PUBLIC_ADDRESS_RANGE=true
   ```
   {: pre}

   You'll receive a notification in the command line when updates to the IBM Cloud CLI and its plug-ins are available. It's important to keep your CLI up to date to access the latest commands. To check the current version of all installed plug-ins, run **`ibmcloud plugin list`**.
   {: tip}

1. Run the following command:

   ```sh
   ibmcloud is public-address-range-delete (PUBLIC_ADDRESS_RANGE1 PUBLIC_ADDRESS_RANGE2 ...) [-f, --force] [--output JSON] [-q, --quiet]
   ```
   

   Where:

   `PUBLIC_ADDRESS_RANGE`
   :   The ID or name of the public address range to delete.

   `-f, --force`
   :   Force the operation without confirmation.

   `--output`
   :   The output format, only JSON is supported. One of: JSON.

   `-q, --quiet`
   :   Suppress verbose output.
   

### Command example
{: #par-delete-cli-examples}

Delete the public address range `$par-id`:

```sh
ibmcloud is public-address-range-delete $par-id
```
{: pre}





## Deleting public address ranges with the API
{: #par-deleting-api}
{: api}

Before you begin, set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
{: requirement}

To delete a public address range, follow these steps:

1. If the address range is bound to a VPC, unbind it first. See [Binding, unbinding, and moving public address ranges](/docs/vpc?topic=vpc-par-unbinding-binding&interface=api).
1. Delete the address range:

   ```sh
   curl -sX DELETE \
            "$vpc_api_endpoint/v1/public_address_ranges/$par_id?version=$api_version&generation=2" \
            -H "Authorization: Bearer $iam_token"
   ```
   {: pre}



## Deleting public address ranges with Terraform
{: #par-delete-terraform}
{: terraform}

To delete a specific public address range with Terraform, run the following command. Replace `ibm_is_public_address_range.here` with the resource name that you defined in your Terraform configuration:

```terraform
terraform destroy --target ibm_is_public_address_range.here
```
{: pre}

To delete all resources that are defined in your `.tf` file:

```terraform
terraform destroy -auto-approve
```
{: pre}

## Related links
{: #after-delete-par}

- [About public address ranges](/docs/vpc?topic=vpc-about-par)
- [Creating public address ranges](/docs/vpc?topic=vpc-par-creating)
