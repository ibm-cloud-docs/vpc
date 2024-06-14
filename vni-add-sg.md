---

copyright:
  years: 2023, 2024
lastupdated: "2024-04-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching security groups to a virtual network interface
{: #vni-add-sg}

Security groups give you a convenient way to apply rules that establish filtering to a target of a virtual server instance, based on its IP address. Virtual network interfaces can be a target for a security group.
{: shortdesc}

You can add security groups to a VNI with the UI, CLI, API, or Terraform.

## Attaching a security group to a virtual network interface in the UI
{: #vni-add-sg-ui}
{: ui}

To add a security group to an existing virtual network interface, follow these steps.

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** icon ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Virtual network interfaces**.
1. Click the name of the virtual network interface that you want to add a security group to in the Virtual network interfaces for VPC table.
1. Click the **Attached resources** tab.
1. In the Security groups section, click **Attach**.
1. In the Attach security group panel that appears, select a security group from the list.
1. Click **Attach** to attach the security group to your virtual network interface, or click **Cancel**.

## Attaching a security group to a virtual network interface from the CLI
{: #vni-add-sg-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

Then run the following command:

```sh
ibmcloud is security-group-target-add GROUP TARGET [--vpc VPC] [(--trt load_balancer | endpoint_gateway | vpn_server | virtual_network_interface) | --in INSTANCE | --bm BARE_METAL_SERVER] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`GROUP`
:   ID or name of the security group.

`TARGET`
:   ID or name of the bound target resource for security group. The following types are supported target resource types: `network_interface`, `load_balancer`, `endpoint_gateway`, `vpn_server`, `virtual_network_interface`.

`--trt value`
:   The bound target resource type, this option is only required if you use the target name instead of ID. One of: `load_balancer`, `endpoint_gateway`, `vpn_server`, `virtual_network_interface`.

### Command example
{: #vni-add-sg-cli-command-examples}

`ibmcloud is security-group-target-add my-sg my-vni --trt virtual_network_interface`

## Attaching a security group to a virtual network interface from the API
{: #vni-add-sg-api}
{: api}

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

    * `version` (string): The API version, in format `YYYY-MM-DD`.
    * `security_group_id` (string): The security group identifier.
    * `virtual_network_interface_id` (string): The virtual network interface identifier.

1. When all variables are initiated, add the virtual network interface as a target of the security group:

    ```sh
    curl -X PUT \
    "$vpc_api_endpoint/v1/security_groups/$security_group_id/targets/$virtual_network_interface_id?version=$version&generation=2" \
    -H "Authorization: Bearer $iam_token"
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
