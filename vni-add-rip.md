---

copyright:
  years: 2023, 2024
lastupdated: "2024-04-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching a reserved IP to a virtual network interface
{: #vni-add-rip}

A VNI is created with a primary IP address, which can be an existing reserved IP or created with the virtual network interface. The primary IP address of a virtual network interface is a reserved IP address.
{: shortdesc}

You can add a reserved IP to a VNI with the UI, CLI, API, or Terraform.


## Determining the primary reserved IP for a virtual network interface in the UI
{: #vni-add-rip-ui}
{: ui}

In the UI, the primary IP address of the virtual network interface is a reserved IP. Reserved IPs can be added to a virtual network interface in three ways.

* When you create a virtual network interface, you must specify a subnet. If you specify _only_ a subnet, a reserved IP address is automatically allocated from that subnet, and assigned as the primary IP address of the virtual network interface.
* While you are creating a virtual network interface, you can select an existing reserved IP address that isn't already attached to another resource. This reserved IP address is used as the primary IP address for your virtual network interface.
* If you specify a subnet and give it an IP address that is a valid address in the subnet, but isn't currently allocated to a reserved IP, a reserved IP is created with that address. This reserved IP address is used as the primary IP address for your virtual network interface.

For more information about adding a primary (reserved) IP to your virtual network interface, see step 10 in [Creating a virtual network interface](/docs/vpc?topic=vpc-vni-create&interface=ui).

## Determining the primary reserved IP for a virtual network interface from the CLI
{: #vni-add-rip-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To add a reserved IP to a virtual network interface from the CLI, enter the following command:

```sh
ibmcloud is subnet-reserved-ip-create SUBNET [--vpc VPC] [--name NAME] [--address ADDRESS] [--auto-delete true | false] [--target TARGET] [--trt endpoint_gateway | virtual_network_interface] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`SUBNET`
:   The ID or name of the subnet.

`--vpc`

:   The ID of the VPC.

`--name`
:  The name of the reserved IP.

`--address`
:   The IP reserved IP address.

`--auto-delete`
:   Indicates whether this virtual network interface will be automatically deleted when target is deleted. Must be `false` if the virtual network interface is unbound. One of: `false`, `true`.

`--target value`
:   The ID or name of the target resource. The following types are supported target resource types: `endpoint_gateway`, `virtual_network_interface`.

`--trt value`
:   The bound target resource type, this option is only required if you use the target name instead of ID. One of: `endpoint_gateway`, `virtual_network_interface`.

`--output`
:   Specify output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppress verbose output.

### Command examples
{: #vni-add-rip-cli-command-examples}

* `ibmcloud is subnet-reserved-ip-create my-subnet --name my-reserved-ip --address 10.240.64.10 --target my-vpe --trt endpoint_gateway`
* `ibmcloud is subnet-reserved-ip-create my-subnet --name my-reserved-ip --address 10.240.64.10 --target my-vni --trt virtual_network_interface`

## Determining the primary reserved IP for a virtual network interface from the API
{: #vni-add-rip-api}
{: api}

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

    * `version` (string): The API version, in format `YYYY-MM-DD`.
    * `virtual_network_interface_id` (string): The virtual network interface identifier.
    * `reserved_ip_id` (string): The reserved IP identifier.

1. When all variables are initiated, add the reserved IP:

    ```sh
    curl -X PUT \
    "$vpc_api_endpoint/v1/virtual_network_interfaces/$virtual_network_interface_id/ips/$reserved_ip_id?version=$version&generation=2" \
    -H "Authorization: Bearer $iam_token"
    ```
    {: codeblock}

## Determining the primary reserved IP for a virtual network interface with Terraform
{: #vni-terraform-add-rip}
{: terraform}

The following example adds a reserved IP address to a virtual network interface by using Terraform:

```terraform
resource "ibm_is_virtual_network_interface_ip" "vni_reservedip" {
	virtual_network_interface = ibm_is_virtual_network_interface.my_vni.id
	reserved_ip = ibm_is_subnet_reserved_ip.my_reservedip.reserved_ip
}
```
{: codeblock}
