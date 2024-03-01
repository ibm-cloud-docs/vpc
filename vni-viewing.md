---

copyright:
  years: 2023
lastupdated: "2023-12-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing details of a virtual network interface
{: #vni-viewing}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

With virtual network interfaces, where you view resources has changed. In older network interfaces, you can view the details of the resources by going to the resource directly. In a virtual network interface, you can view the resource's details from within the virtual network interface.
{: shortdesc}

You can view the details of a VNI with the UI, CLI, API, or Terraform.

## Viewing details of a virtual network interface from the UI
{: #virtual-network-interface-view-ui}
{: ui}

To view details of a virtual network interface from the UI, take the following steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** icon ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>Virtual network interfaces**.
1. Click the name of the virtual network interface that you want to view in the Virtual network interfaces for VPC table.
1. In the Overview tab, you can view the following items:

    * The details of your virtual network interface
    * The resources that are attached to the virtual network interface
    * The target resource details, and where to add target resources if none are present
    * Any floating IPs that are attached
1. In the Attached resources tab, you can view the secondary IP addresses and security groups that are attached.

## Viewing details of a virtual network interface with the CLI
{: #virtual-network-interface-view-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

```sh
export IBMCLOUD_IS_FEATURE_VNI_PHASE_II=true
```
{: pre}

To view details of a virtual network interface from the CLI, enter the following command:

```sh
ibmcloud is virtual-network-interface VIRTUAL_NETWORK_INTERFACE [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`VIRTUAL_NETWORK_INTERFACE`
:   ID or name of the virtual network interface.

`--output`
:   Specify output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppress verbose output.

### Command examples
{: #cli-command-examples-virtual-network-interface}

- `ibmcloud is virtual-network-interface r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2`
- `ibmcloud is virtual-network-interface new-share-vni`

## Viewing details of a virtual network interface with API
{: #virtual-network-interface-view-api}
{: api}

To view the details of a virtual network interface with the API, run the following command:

```sh
curl -X GET \
"$vpc_api_endpoint/v1/virtual_network_interfaces/$network_interface_id?version=$version&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

## Viewing details of a virtual network interface with Terraform
{: #virtual-network-interface-view-terraform}
{: terraform}

The following example shows how to view the details of a virtual network interface by using Terraform:

```terraform
data "ibm_is_virtual_network_interface" "example"
{
  virtual_network_interface = ibm_is_virtual_network_interface.my_vni.id
}
```
{: codeblock}
