---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-21"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching floating IPs to a virtual network interface
{: #vni-add-fip}

Floating IP addresses are IP addresses that are provided by the system and are reachable from the public internet. A floating cannot be attached to a virtual network interface if that virtual network interface is attached to a share mount target.
{: shortdesc}

You can attach floating IP addresses to a VNI with the console, CLI, API, or Terraform.

## Attaching a floating IP to a virtual network interface in the console
{: #vni-add-fip-ui}
{: ui}

To attach a floating IP to an existing virtual network interface, follow these steps.

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Virtual network interfaces**.
1. Click the name of the virtual network interface that you want to attach a floating IP to in the Virtual network interfaces for VPC table.
1. In the Floating IPs section, click **Attach**.
1. In the Attach floating IP panel that appears, you can choose one of the following options:
    * Reserve a new floating IP by clicking **Reserve new floating IP** and completing the requested information in the panel that appears
    * Select an existing floating IP from the Floating IP addresses menu.
1. Click **Attach** to attach the floating IP to your virtual network interface, or click **Cancel**.

## Attaching a floating IP to a virtual network interface from the CLI
{: #vni-add-fip-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

Then run the following command:

```sh
ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE_NAME | --nic TARGET_INTERFACE [--in TARGET_INSTANCE | --bm TARGET_BARE_METAL_SERVER | --vni TARGET_VIRTUAL_NETWORK_INTERFACE]) [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`FLOATING_IP_NAME`
:   ID or name of the floating IP.

`--zone`
:   ID or name of the zone.

`--nic TARGET_INTERFACE`
:   The target interface.

`--in TARGET_INSTANCE`
:   The target virtual server instance.

`--bm TARGET_BARE_METAL_SERVER`
:   The target bare metal server.

`--vni TARGET_VIRTUAL_NETWORK_INTERFACE`
:   The target virtual network interface.

`--resource-group-id RESOURCE_GROUP_ID`
:   The ID of the resource group.

`--resource-group-name RESOURCE_GROUP_NAME`
:   The name of the resource group.

`--output`
:   Specify output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppress verbose output.



### Command examples
{: #vni-attach-fip-examples-cli}



* `ibmcloud is floating-ip-reserve cli-vni-ip --vni vni2`
* `ibmcloud is floating-ip-reserve cli-vni-ip-1 --vni 1234-a56b7c89-d0e1-234f-678g-901h2ij3k45`



## Attaching a floating IP to a virtual network interface with the API
{: #vni-api-add-fip}
{: api}

To attach a floating IP address to a virtual network interface with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

    * `version` (string): The API version, in format `YYYY-MM-DD`.
    * `virtual_network_interface_id` (string): The virtual network interface identifier.
    * `floating_ip_id` (string): The floating IP identifier.

1. When all variables are initiated, attach the floating IP address to the virtual network interface:

    ```sh
    curl -X PUT \
    "$vpc_api_endpoint/v1/virtual_network_interfaces/$virtual_network_interface_id/floating_ips/$floating_ip_id?version=$version&generation=2" \
    -H "Authorization: Bearer $iam_token"
    ```
    {: codeblock}

## Attaching a floating IP to a virtual network interface with Terraform
{: #vni-terraform-add-fip}
{: terraform}

The following example attaches a floating IP address to a virtual network interface by using Terraform:

```terraform
resource "ibm_is_virtual_network_interface_floating_ip" "my_vni_floatingip" {
  virtual_network_interface = ibm_is_virtual_network_interface.my_virtual_network_interface.id
  floating_ip = ibm_is_floating_ip.my_floatingip.id
}
```
{: codeblock}
