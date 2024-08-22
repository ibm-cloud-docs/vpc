---

copyright:
  years: 2023, 2024
lastupdated: "2024-08-22"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adding secondary IP addresses to a virtual network interface
{: #attach-secondary-ip-addresses}

A secondary IP addresses in virtual network interfaces can help you operate network appliances such as load balancers or firewalls that have multiple IP addresses for each network interface. These addresses must be in the same subnet as your network interface, and will be associated with the security group that your network interface is a member of.
{: shortdesc}

Secondary IP addresses can be created when you create your virtual network interface, or they can be added to the virtual network interface after the VNI creation. For steps to add a secondary IP while you create a VNI, see [Creating a virtual network interface](/docs/vpc?topic=vpc-vni-create).

You can add secondary IP addresses to a VNI with the UI, CLI, API, or Terraform.

## Adding a secondary IP address to an existing virtual network interface in the UI
{: #vni-existing-secondary-ip-ui}
{: ui}

To add a secondary IP address to an existing virtual network interface, follow these steps.

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** icon ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Virtual network interfaces**.
1. Click the name of the virtual network interface that you want to add a secondary IP address to in the Virtual network interfaces for VPC table.
1. Click the **Attached resources** tab.
1. In the Secondary IPs section, click **Attach**.
1. In the Attach secondary IPs panel that appears, specify whether you want an address created for you, or if you want to type in your own address.
    - If you select to have an address created for you, specify the quantity of addresses you want to create.
    - If you select to specify your own address, you can select from a list of existing IP addresses, or type in an IP address in the field provided.
1. Select whether you want auto-release enabled for the secondary IP addresses.
1. Click **Attach** to attach the secondary IP address to your virtual network interface, or click **Cancel**.

## Adding a secondary IP address to an existing virtual network interface from the CLI
{: #vni-existing-secondary-ip-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

```sh
export IBMCLOUD_IS_FEATURE_VNI_PHASE_II=true
```
{: pre}

Then run the following command:

```sh
ibmcloud is virtual-network-interface-update VIRTUAL_NETWORK_INTERFACE --name NEW_NAME [--allow-ip-spoofing false | true] [--auto-delete false | true] [--enable-infrastructure-nat false | true] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`VIRTUAL_NETWORK_INTERFACE`
:   ID or name of the virtual network interface.

`--name`
:   Name of the virtual network interface.

`--allow-ip-spoofing`
:   Indicates whether source IP spoofing is allowed on this interface. If `false`, source IP spoofing is prevented on this interface. If `true`, source IP spoofing is allowed on this interface. One of: `false`, `true`.

`--auto-delete`
:   Indicates whether this virtual network interface will be automatically deleted when target is deleted. Must be `false` if the virtual network interface is unbound. One of: `false`, `true`.

`--enable-infrastructure-nat`
:   If `true`, the VPC infrastructure performs any needed NAT operations. If `false`, packets are passed unchanged to/from the network interface, allowing the workload to perform any needed NAT operations. One of: `false`, `true`.

`--output`
:   Specify output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppress verbose output.

### Command examples
{: #vni-existing-secondary-ip-cli-command-examples}

- `ibmcloud is virtual-network-interface-update 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --name new-vni`
- `ibmcloud is virtual-network-interface-update new-vni --name new-share`
- `ibmcloud is virtual-network-interface-update 7208-8918786e-5958-42fc-9e4b-410c5a58b164 --name cli-vni-1 --allow-ip-spoofing false --auto-delete false --enable-infrastructure-nat false`
- `ibmcloud is virtual-network-interface-update cli-vni-1 --name cli-vni-2 --allow-ip-spoofing false --auto-delete true --enable-infrastructure-nat false`

## Attaching a secondary IP to an existing virtual network interface from the API
{: #vni-existing-secondary-ip-api}
{: api}

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

    * `version` (string): The API version, in format `YYYY-MM-DD`.
    * `virtual_network_interface_id` (string): The virtual network interface identifier.
    * `secondary_ip` (array): The information about the secondary IP that you want to attach.

1. When all variables are initiated, add the secondary IP to the existing virtual network interface: [examples only show rename]{: tag-red}

    ```sh
    curl -X PATCH \
    "$vpc_api_endpoint/v1/virtual_network_interfaces/$virtual_network_interface_id?version=$version&generation=2" \
    -H "Authorization: Bearer $iam_token" \
    -d '{
          "name": "my-virtual-network-interface-updated"
    }'
    ```
    {: codeblock}

## Adding a secondary IP to an existing virtual network interface with Terraform
{: #vni-terraform-existing-secondary-ip}
{: terraform}

The following example adds a secondary IP to an existing virtual network interface by using Terraform: [examples only show rename]{: tag-red}

```terraform
resource "ibm_is_virtual_network_interface" "my_virtual_network_interface_instance" {
  allow_ip_spoofing = true
  auto_delete = false
  enable_infrastructure_nat = true
  name = "my-virtual-network-interface-2"
  subnet = ibm_is_subnet.my_subnet.id
}
```
{: codeblock}
