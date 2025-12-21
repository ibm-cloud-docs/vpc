---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-21"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating a virtual network interface
{: #vni-updating}

If you need to change attributes of a virtual network interface, you can update it by using the console, CLI, API, or Terraform. You can change its name, allow or disallow IP spoofing, enable or disable Auto-deletion, Infrastructure NAT, and protocol state filtering.
{: shortdesc}

## Updating a virtual network interface in the console
{: #vni-update-ui}
{: ui}

To update an existing virtual network interface, follow these steps.

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Virtual network interfaces**.
1. Click the name of the virtual network interface in the Virtual network interfaces for VPC table.
1. In the Overview view of the Details page, you can click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit the name of the virtual network interface.
1. Select the switch for Infrastructure NAT to the wanted state.
    * **Enabled** includes one floating IP address, and supports virtual servers, bare metal servers, and file shares.
    * **Disabled** supports multiple floating IP addresses only on bare metal servers. Virtual servers and file shares as virtual network interface targets are not supported.
1. Select the switch for Allow IP spoofing to the wanted state. IP spoofing supports only virtual server instances and bare metal servers. File shares are not supported.
1. Select the switch to enable or disable auto release for this virtual network interface.

    Auto release cannot be enabled without a target device.
    {: note}

1. Click **Edit** ![Edit icon](images/edit.png) to change the [protocol state filtering mode](/docs/vpc?topic=vpc-vni-about&interface=ui#protocol-state-filtering), then select one of the following options.

   * **Auto** (default): Filtering is enabled or disabled based on the virtual network interface's target resource:
      * Bare metal server (Disabled)
      * Virtual server instance (Enabled)
   * **Enabled**: Forces TCP connections to align with the [RFC793](https://www.ietf.org/rfc/rfc793.txt){: external} standard and any packets to be allowed by corresponding security group rules and network ACLs.
   * **Disabled**: Permits packets to be allowed only by corresponding security group rules and network ACLs.
1. In the Attached resources section, use the Display resource list menu to view the security groups or secondary IP addresses attached to the virtual network interface.
    * Clicking **Manage attached resources** in the Attached resources section takes you to the Attached resources tab.
1. To create devices or attach existing devices, follow the links in the Target device details section.
1. In the Floating IPs section, click **Attach** to reserve a new floating IP or attach an existing floating IP.

    If a floating IP is attached, the virtual network interface is accepted as a file share mount target. If infrastructure NAT is enabled, at most one floating IP can be attached.
    {: note}

1. In the Attached resources tab, you can view secondary IP addresses and security groups that are already attached. You can also create more secondary IP addresses or security groups to attach to your virtual network interface.

## Updating a virtual network interface from the CLI
{: #virtual-network-interface-update-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

```sh
export IBMCLOUD_IS_FEATURE_VNI_ENABLE_PROTOCOL_STATE_FILTERING=true
```
{: pre}

To update a virtual network interface from the CLI, enter the following command:

```sh
ibmcloud is virtual-network-interface-update VIRTUAL_NETWORK_INTERFACE --name NEW_NAME [--allow-ip-spoofing false | true] [--auto-delete false | true] [--enable-infrastructure-nat false | true] [--protocol-state-filtering-mode auto | disabled | enabled] [--output JSON] [-q, --quiet]
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
:   Indicates whether this virtual network interface is to be automatically deleted when target is deleted. Must be `false` if the virtual network interface is unbound. One of: `false`, `true`.

`--enable-infrastructure-nat`
:   If `true`, the VPC infrastructure performs any needed NAT operations. If `false`, packets are passed unchanged through the network interface, allowing the workload to perform any needed NAT operations. One of: `false`, `true`.

`--protocol-state-filtering-mode`
:   The status of the protocol state filtering mode. One of `auto`, `enabled`, `disabled`.
       * **Auto** (default): Filtering is enabled or disabled based on the virtual network interface's target resource.
          * Bare metal server (Disabled)
          * Virtual server instance (Enabled)
          * File share mount (Enabled)
       * **Enabled**: Forces the TCP connections to align with the [RFC793](https://www.ietf.org/rfc/rfc793.txt){: external} standard and any packets to be allowed by corresponding security group rules and network ACLs.
       * **Disabled**: Permits packets to be allowed only by corresponding security group rules and network ACLs.

`--output`
:   Specify output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppress verbose output.

### Command examples
{: #cli-command-examples-virtual-network-interface-update}

- `ibmcloud is virtual-network-interface-update 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --name new-vni`
- `ibmcloud is virtual-network-interface-update new-vni --name new-share`
- `ibmcloud is virtual-network-interface-update 7208-8918786e-5958-42fc-9e4b-410c5a58b164 --name cli-vni-1 --allow-ip-spoofing false --auto-delete false --enable-infrastructure-nat false --protocol-state-filtering-mode auto`
- `ibmcloud is virtual-network-interface-update cli-vni-1 --name cli-vni-2 --allow-ip-spoofing false --auto-delete true --enable-infrastructure-nat false --protocol-state-filtering-mode disabled`

## Updating a virtual network interface with the API
{: #vni-api-update}
{: api}

To update a virtual network interface with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands. See the following examples:

    * `version` (string): The API version, in format `YYYY-MM-DD`.
    * `virtual_network_interface_id` (string): The virtual network interface identifier.

1. When all variables are initiated, update the virtual network interface:

    ```sh
    curl -X PATCH \
        "$vpc_api_endpoint/v1/virtual_network_interfaces/$virtual_network_interface_id?version=$version&generation=2" \
        -H "Authorization: Bearer $iam_token" \
        -d '{
              "name": "my-virtual-network-interface",
              "primary_ip": {
                "address": "10.0.0.5"
              },
              "protocol_state_filtering_mode": "disabled",
              "security_groups": [
                {
                  "id": "be5df5ca-12a0-494b-907e-aa6ec2bfa271"
                },
                {
                  "id": "032e1387-71ba-4e83-b268-a53edf94af19"
                }
              ],
              "subnet": {
                "id": "032e1387-71ba-4e83-b268-a53edf94af19"
              }
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
  protocol_state_filtering_mode = "auto"
}
```
{: codeblock}
