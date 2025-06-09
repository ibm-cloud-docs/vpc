---

copyright:
  years: 2023, 2025
lastupdated: "2025-06-09"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a virtual network interface
{: #vni-deleting}

When you no longer need a virtual network interface, you can delete it by using the console, CLI, API, or Terraform. Make sure that no attachments remain in the virtual network interface before attempting to delete.
{: shortdesc}

## Deleting a virtual network interface in the console
{: #vni-delete-ui}
{: ui}

To delete a virtual network interface, follow these steps.

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Virtual network interfaces**.
1. Click the name of the virtual network interface that you want to delete in the Virtual network interfaces for VPC table.
1. Click the **Actions** menu on the Details page, and select **Delete**.
1. Confirm that you want to delete this virtual network interface in the window that appears, or click **Cancel**.

## Deleting one or more virtual network interfaces from the CLI
{: #virtual-network-interface-delete-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

```sh
export IBMCLOUD_IS_FEATURE_VNI_PHASE_II=true
```
{: pre}

To delete one or more virtual network interfaces from the CLI, enter the following command:

```sh
ibmcloud is virtual-network-interface-delete (VIRTUAL_NETWORK_INTERFACE1 VIRTUAL_NETWORK_INTERFACE2 ...) [-f, --force] [-q, --quiet]
```
{: pre}

Where:

`VIRTUAL_NETWORK_INTERFACE1`
:   ID or name of the virtual network interface.

`VIRTUAL_NETWORK_INTERFACE2`
:   ID or name of the virtual network interface.

`--force, -f`
:   Forces the operation without confirmation.

`-q, --quiet`
:   Suppress verbose output.

### Command examples
{: #cli-command-examples-virtual-network-interface-delete}

- `ibmcloud is virtual-network-interface-delete my-vni-share-99 cli-vni-demo-00`
- `ibmcloud is virtual-network-interface-delete r006-866fc826-6f30-444f-b55e-0d697cf8b4bb`

## Deleting a virtual network interface with the API
{: #vni-api-delete}
{: api}

To delete a virtual network interface with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

    * `version` (string): The API version, in format `YYYY-MM-DD`.
    * `virtual_network_interface_id` (string): The virtual network interface identifier.

    ```sh
    curl -X DELETE \
    "$vpc_api_endpoint/v1/virtual_network_interfaces/$virtual_network_interface_id?version=$version&generation=2" \
    -H "Authorization: Bearer $iam_token"
    ```
    {: codeblock}

## Deleting a virtual network interface with Terraform
{: #vni-terraform-delete}
{: terraform}

The following example deletes a virtual network interface by using Terraform:

```terraform
terraform destroy --target ibm_is_virtual_network_interface.my_virtual_network_interface_instance
```
{: codeblock}
