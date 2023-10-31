---

copyright:
  years:  2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a virtual network interface
{: #vni-create}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

A virtual network interface (VNI) can be created without attaching to a target. Therefore, a virtual network interface can exist even when its target is removed. For example, within a VPC, your instance has a public IP address by which the instance can be reached from the internet, and a private IP address by which it can be reached from other instances in your VPC. You configured the instance with custom security group rules. If you delete the instance, the public IP address and security groups are detached. The private IP address is released and can be reserved again by another resource. If you want to create a new instance with the same public IP address, private IP address, and security groups as the deleted instance, you must reserve the private IP address again, and individually attach the previous public IP address and security groups.

You can use a virtual network interface to manage the IP addresses and security groups in a separate resource with a lifecycle that is independent of your instances. Create a virtual network interface with a private IP address, public IP address, and security groups. Then, create an instance with this virtual network interface. Later you can delete the instance, preserving the virtual network interface, and create a new instance with the same virtual network interface.

## Before you begin
{: #vni-prerequisites}

To create a virtual network interface, you must have the following prerequisites:

* A VPC instance
* A subnet in which to create a virtual network interface
* You must have IAM Administrator role permission to configure IP spoofing and infrastructure NAT.

You can create a VNI with the UI, CLI, API, or Terraform.

## Creating a virtual network interface in the UI
{: #vni-create-ui}
{: ui}

To create a virtual network interface in the UI, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the Menu icon ![Navigation Menu icon](../../icons/icon_hamburger.svg) from the upper left, then click **VPC Infrastructure > Virtual network interfaces**.
1. On the Virtual network interfaces for VPC page, click **Create**.
1. In the Location section, edit the following fields, if necessary.
    * **Geography**: Indicates the geography where you want the virtual network interface created.
    * **Region**: Indicates the region where you want the virtual network interface created.
1. In the Details section, complete the following information:
   * **Name**: Enter a unique name for the virtual network interface, such as `my-virtual-network-interface`.
   * **Resource group**: Select a resource group for the virtual network interface.
   * **Tags**: (optional) Add tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Access management tags**: (optional) Add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
1. In the Network configuration section, select the VPC in which you want to create your virtual network interface. If you need to create a VPC, click **Create VPC** to get started configuring a VPC.
1. In the Subnet section, select a subnet in which to create the virtual network interface. If you need to create a subnet, click **Create**.
1. Click the switch to enable or disable Allow IP spoofing. IP spoofing supports only virtual server instances and bare metal servers. File shares are not supported.
1. Click the switch for Infrastructure NAT to the wanted state.
    * **Enabled** includes one floating IP address, and supports virtual servers, bare metal servers, and file shares.
    * **Disabled** supports multiple floating IP addresses only on bare metal servers. Virtual servers and file shares as virtual network interface targets are not supported.
1. In the Primary IP section, make the following selections.
    * **Reserving method**: Select whether you want a primary IP address created for you, or if you want to specify one manually. If you specify your own, type an existing reserved IP address for your virtual network interface, or select one from the existing reserved IP list menu.
    * **Auto release**: Click the switch to enable or disable auto release for this virtual network interface.
1. In the Floating IPs section (optional), click **Attach**. In the side panel that appears, you can either select from the existing list of floating IP addresses, or select **Reserve new Floating IP** and complete the information that is requested.

    If a floating IP is attached, the virtual network interface will not be accepted as file share mount target. If infrastructure NAT is enabled, at most one floating IP can be attached.
    {: note}

1. In the Secondary IP section (optional), click **Attach**. Select a reserving method, and specify whether auto release is enabled.

    A virtual network interface with secondary IPs attached cannot be accepted as a file share mount target.
    {: note}

1. In the Security groups section, select at least one and at most five security groups to control traffic at the networking level. You can select security groups from the list, or create one by clicking **Create**. For more information about creating a security group, see [Creating security groups](/docs/vpc?topic=vpc-using-security-groups).
1. Review the information in the Summary panel, and click **Create virtual network interface**.

## Creating a virtual network interface from the CLI
{: #virtual-network-interface-create}
{: cli}

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

```sh
export IBMCLOUD_IS_FEATURE_VNI_PHASE_II=true
```
{: pre}

To create a virtual network interface from the CLI, enter the following command:

```sh
ibmcloud is virtual-network-interface-create [--name NAME] [--allow-ip-spoofing false | true] [--auto-delete false | true] [--enable-infrastructure-nat false | true] [[--rip RIP | [--rip-address RIP_ADDRESS --rip-auto-delete RIP_AUTO_DELETE --rip-name RIP_NAME]]] [--subnet SUBNET] [--ips RESERVED_IPS_JSON | @RESERVED_IPS_JSON_FILE] [--sgs SGS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--vpc VPC] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`--name`
:   The name for this virtual network interface.

`--allow-ip-spoofing`
:   Indicates whether source IP spoofing is allowed on this interface. If `false`, source IP spoofing is prevented on this interface. If `true`, source IP spoofing is allowed on this interface. One of: `false`, `true`.

`--auto-delete`
:   Indicates whether this virtual network interface will be automatically deleted when target is deleted. Must be `false` if the virtual network interface is unbound. One of: `false`, `true`.

`--enable-infrastructure-nat`
:   If `true`, the VPC infrastructure performs any needed NAT operations. If `false`, packets are passed unchanged to/from the network interface, allowing the workload to perform any needed NAT operations. One of: `false`, `true`.

`--rip`
:   ID or name of the Reserved IP to bind to the virtual network interface.

`--rip-address`
:   The IP address of the Reserved IP to bind to the virtual network interface.

`--rip-auto-delete`
:   Indicates whether this reserved IP will be automatically deleted when either target is deleted, or the reserved IP is unbound.

`--rip-name`
:   The name for this reserved IP to bind to the virtual network interface.

`--subnet`
:   The associated subnet.

`--ips ips RESERVED_IPS_JSON | @RESERVED_IPS_JSON_FILE`
:   Secondary reserved IP addresses in JSON or JSON file, to bind to the virtual network interface. For the data schema, check the IPs property in the API documentation. One of: `RESERVED_IPS_JSON`, `@RESERVED_IPS_JSON_FILE`.

`--sgs`
:   IDs or Names of the security groups to use for the virtual network interface.

`--resource-group-id`
:   ID of the resource group. This ID is mutually exclusive with `--resource-group-name`.

`--resource-group-name`
:   Name of the resource group. This name is mutually exclusive with `--resource-group-id`.

`--vpc`
:   ID or name of the VPC to which this VNI is associated.

`--output`
:   Specify output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppress verbose output.

### Command examples
{: #command-examples-virtual-network-interface-create}

- `ibmcloud is virtual-network-interface-create --subnet 7208-d42716a5-6df2-416c-979d-f26330b9eod1`
- `ibmcloud is virtual-network-interface-create --name cli-vni-1 --allow-ip-spoofing true --auto-delete true --enable-infrastructure-nat true --rip 7208-d4c0abbe-3fc2-4696-9fe1-4eb3dc9af976 --ips '[{"id":"7208-d83b7e58-3c3d-47d0-89c5-02d9a20c72fd"},{"address":"10.240.64.13", "auto_delete": false, "name": "srip2"}]' --sgs r006-aa7c7658-e503-4456-b342-8d6a89e05115,r006-4fb388f1-2b6e-4013-b279-7a8748f4d6ca --resource-group-id 11caaa983d9c4beb82690daab08717e9`
- `ibmcloud is virtual-network-interface-create --name cli-vni-2 --allow-ip-spoofing true --auto-delete true --enable-infrastructure-nat true --rip cli-rip-1 --subnet my-subnet --vpc vpc-cli-1 --ips '[{"id":"7208-d83b7e58-3c3d-47d0-89c5-02d9a20c72fd"},{"address":"10.240.64.14", "auto_delete": false, "name": "srip3"}]' --sgs cli-sg,sanctity-contest-only-filing --resource-group-name Default`
- `ibmcloud is virtual-network-interface-create --name cli-vni-4 --allow-ip-spoofing false --auto-delete false --enable-infrastructure-nat false --subnet 7208-bfe017e7-6e71-415a-8615-0ee787fbeef9 --rip-address 10.240.64.15 --rip-auto-delete false --rip-name primar-ip-1 --ips '[{"id":"7208-2772a45f-c062-4e22-bafb-32ea792da56b"},{"address":"10.240.64.17", "auto_delete": false, "name": "sec-ip-2"}]' --sgs r006-aa7c7658-e503-4456-b342-8d6a89e05115,r006-4fb388f1-2b6e-4013-b279-7a8748f4d6ca --resource-group-id 11caaa983d9c4beb82690daab08717e9`
- `ibmcloud is virtual-network-interface-create --name cli-vni-3 --allow-ip-spoofing true --auto-delete false --enable-infrastructure-nat true --subnet my-subnet --vpc vpc-cli-1 --rip-address 10.240.64.18 --rip-auto-delete true --rip-name primar-ip-2 --ips '[{"id":"7208-d42716a5-6df2-416c-979d-f26330b9d0b1"},{"address":"10.240.64.19", "auto_delete": true, "name": "sec-ip-3"}]' --sgs cli-sg,sanctity-contest-only-filing --resource-group-name Default`

## Creating a virtual network interface with the API
{: #vni-api-create}
{: api}

To create a virtual network interface with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

    * `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, create the virtual network interface:

    ```sh
    curl -X POST \
    "$vpc_api_endpoint/v1/virtual_network_interfaces?version=$version&generation=2" \
    -H "Authorization: Bearer $iam_token"
    -d '{
          "name": "my-virtual-network-interface",
          "primary_ip": {
            "address": "10.0.0.5"
          },
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

## Creating a virtual network interface with Terraform
{: #vni-terraform-create}
{: terraform}

The following example creates a virtual network interface by using Terraform:

```terraform
resource "ibm_is_virtual_network_interface" "my_virtual_network_interface_instance" {
  allow_ip_spoofing = true
  auto_delete = false
  enable_infrastructure_nat = true
  name = "my-virtual-network-interface"
  subnet = ibm_is_subnet.my_subnet.id
}
```
{: codeblock}

For more information, see the [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_virtual_network_interface){: external}
