---

copyright:
  years:  2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating a virtual network interface
{: #vni-updating}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

You can update a VNI with the UI, CLI, API, or Terraform.

## Updating a virtual network interface in the UI
{: #vni-update-ui}
{: ui}

To update an existing virtual network interface, follow these steps.

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the Menu icon ![Navigation Menu icon](../../icons/icon_hamburger.svg) from the upper left, then click **VPC Infrastructure > Virtual network interfaces**.
1. Click the name of the virtual network interface in the Virtual network interfaces for VPC table.
1. In the Overview view of the Details page, you can click the Edit icon ![Edit icon](/images/edit.png) to edit the change of the virtual network interface.
1. Click the switch for Infrastructure NAT to the wanted state.
    * **Enabled** includes one floating IP address, and supports virtual servers, bare metal servers, and file shares.
    * **Disabled** supports multiple floating IP addresses only on bare metal servers. Virtual servers and file shares as virtual network interface targets are not supported.
1. Click the switch for Allow IP spoofing to the wanted state. IP spoofing supports only virtual server instances and bare metal servers. File shares are not supported.
1. Click the switch to enable or disable auto release for this virtual network interface.

    Auto release cannot be enabled without a target device.
    {: note}

1. In the Attached resources section, use the Display resource list menu to view the security groups or secondary IPs attached to the virtual network interface.
    * Clicking **Manage attached resources** in the Attached resources section takes you to the Attached resources tab.
1. To create devices or attach existing devices, follow the links in the Target device details section.
1. In the Floating IPs section, click **Attach** to reserve a new floating IP or attach an existing floating IP.

    If a floating IP is attached, the virtual network interface is be accepted as a file share mount target. If infrastructure NAT is enabled, at most one floating IP can be attached.
    {: note}

1. In the Attached resources tab, view secondary IPs and security groups that are already attached, or create secondary IPs or security groups to attach to your virtual network interface.

## Updating a virtual network interface from the CLI
{: #virtual-network-interface-update-cli}
{: cli}

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

```sh
export IBMCLOUD_IS_FEATURE_VNI_PHASE_II=true
```
{: pre}

To update a virtual network interface from the CLI, enter the following command:

```sh
ibmcloud is virtual-network-interface-update VIRTUAL_NETWORK_INTERFACE --name NEW_NAME [--allow-ip-spoofing false | true] [--auto-delete false | true] [--enable-infrastructure-nat false | true] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`VIRTUAL_NETWORK_INTERFACE`
:   ID or name of the virtual network interface.

`--name`
:   New name of the virtual network interface.

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
{: #cli-command-examples-virtual-network-interface-update}

- `ibmcloud is virtual-network-interface-update 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --name new-vni`
- `ibmcloud is virtual-network-interface-update new-vni --name new-share`
- `ibmcloud is virtual-network-interface-update 7208-8918786e-5958-42fc-9e4b-410c5a58b164 --name cli-vni-1 --allow-ip-spoofing false --auto-delete false --enable-infrastructure-nat false`
- `ibmcloud is virtual-network-interface-update cli-vni-1 --name cli-vni-2 --allow-ip-spoofing false --auto-delete true --enable-infrastructure-nat false`

## Updating a virtual network interface with the API
{: #vni-api-update}
{: api}

To update a virtual network interface with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

    * `version` (string): The API version, in format `YYYY-MM-DD`.
    * `virtual_network_interface_id` (string): The virtual network interface identifier.

1. When all variables are initiated, update the virtual network interface:

    ```sh
    curl -X PATCH \
    "$vpc_api_endpoint/v1/virtual_network_interfaces/$virtual_network_interface_id?version=$version&generation=2" \
    -H "Authorization: Bearer $iam_token" \
    -d '{
          "name": "my-virtual-network-interface-updated"
    }'
    ```
    {: codeblock}

## Updating a virtual network interface with Terraform
{: #vni-terraform-update}
{: terraform}

The following example updates a virtual network interface by using Terraform:

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
