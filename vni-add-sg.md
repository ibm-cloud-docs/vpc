---

copyright:
  years:  2023
lastupdated: "2023-12-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching security groups to a virtual network interface
{: #vni-add-sg}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

You can add security groups to a VNI with the UI, CLI, API, or Terraform.

## Attaching a security group to a virtual network interface in the UI
{: #vni-add-sg-ui}
{: ui}

To add a security group to an existing virtual network interface, follow these steps.

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Menu** icon ![menu icon](../../icons/icon_hamburger.svg), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>Virtual network interfaces**.
1. Click the name of the virtual network interface that you want to add a security group to in the Virtual network interfaces for VPC table.
1. Click the **Attached resources** tab.
1. In the Security groups section, click **Attach*.
1. In the Attach security group panel that appears, select a security group from the list.
1. Click **Attach** to attach the security group to your virtual network interface, or click **Cancel**.

## Attaching a security group to a virtual network interface from the CLI
{: #vni-add-sg-cli}
{: cli}

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

```sh
export IBMCLOUD_IS_FEATURE_VNI_PHASE_II=true
```
{: pre}

Then run the following command:

```sh
ibmcloud is security-group-target-add GROUP TARGET [--vpc VPC] [(--trt load_balancer | endpoint_gateway | vpn_server | virtual_network_interface) | --in INSTANCE | --bm BARE_METAL_SERVER] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`GROUP`
:   ID or name of the security group.

`TARGET`
:   ID or name of the bound target resource for security group. The following types are supported target resource types: `network_interface`, `load_balancer`, `endpoint_gateway`, `vpn_server`, `virtual_network_interface`

`--trt value`
:   The bound target resource type, this option is only required if you use the target name instead of ID. One of: `load_balancer`, `endpoint_gateway`, `vpn_server`, `virtual_network_interface`.`

### Command example
{: #vni-add-sg-cli-command-examples}

`ibmcloud is security-group-target-add my-sg my-vni --trt virtual_network_interface`

## Attaching a security group to a virtual network interface from the API
{: #vni-add-sg-api}
{: api}

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

    * `version` (string): The API version, in format `YYYY-MM-DD`.
    * `virtual_network_interface_id` (string): The virtual network interface identifier.
    * `security_groups` (array): The information about the security group you wish to attach.
        * `crn` (string): The CNR of this security group.
        * `href` (string): The canonical URL of this security group.
        * `id` (string): The unique identifier of this security group.
        * `name` (string): The name for this security group. The name is unique across all security groups for the VPC.

1. When all variables are initiated, add the security group to the virtual network interface:

```sh
curl -X POST "https://au-syd.iaas.cloud.ibm.com/v1/virtual_network_interfaces?version=2023-10-18&generation=2" -H  "accept: application/json" -H  "Content-Type: application/json" -d "{\"name\":\"my-virtual-network-interface\",\"target\":{\"id\":\"69e55145-cc7d-4d8e-9e1f-cc3fb60b1793\"}}"
```
{: codeblock}

## Attaching a security group to a virtual network interface with Terraform
{: #vni-terraform-add-sg}
{: terraform}

The following example adds a security group to a virtual network interface by using Terraform:

```terraform
resource "ibm_is_security_group_target" "example" {
  security_group = ibm_is_security_group.example.id
  target         = ibm_is_virtual_network_interface.my_vni.id
}
```
{: codeblock}
