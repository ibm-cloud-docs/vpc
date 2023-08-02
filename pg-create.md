---
copyright:
  years: 2022
lastupdated: "2022-11-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating public gateways
{: #create-public-gateways}
{: help}
{: support}

You can create and attach a public gateway to specific Virtual Private Cloud (VPC) subnets and virtual server instances. Before you begin, make sure to review the use cases that are listed in [About public gateways](/docs/vpc?topic=vpc-about-public-gateways).
{: shortdesc}

## Before you begin
{: #pg-before-you-begin}

Before you create a public gateway, you must satisfy the following prerequisites:

1. Make sure that at least one VPC and subnet exist. For more information, see [Creating a VPC and subnet](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#creating-a-vpc-and-subnet).
1. Attach any floating IP addresses to your virtual server instances (optional). For more information, see [Reserving a floating IP address](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console&interface=ui#reserving-a-floating-ip-address).

Attaching floating IP addresses to virtual server instances enables communication to and from that particular instance, independent of whether the subnet is attached to a public gateway.
{: note}

Only one public gateway per zone is allowed in a VPC, but a public gateway can be attached to multiple subnets in a zone.
{: important}

## Creating a public gateway in the UI
{: #pg-creating-ui}
{: ui}

To create a public gateway using the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. Go to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external} and log in to your account.

1. Select the Menu icon ![Menu icon](./images/menu_icon.png), then click **VPC Infrastructure** > **Public gateways**. The Public gateways for VPC page shows.
1. Click **Create** to go to the **Create public gateway** page.
1. Select the **Edit location** icon ![Edit location icon](../icons/edit-tagging.svg "Edit location") and enter values for the following fields:

   * **Geography** - Select a continent for your public gateway.
   * **Region** - Select a region for your public gateway.
   * **Zone** - Select a zone for your public gateway.

1. Enter values for the following fields under details:

   * **Public gateway name** - Type a unique name for your public gateway.
   * **Resource group** - Select a resource group for your public gateway collector. You can use the default group for this public gateway, or select from the list (if defined). For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).
      After provisioning is complete, you cannot change the resource group.
      {: important}

   * **VPC** - Select a VPC. You can use the default VPC for this public gateway, or select from the list (if defined). For more information, see [Getting started with Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started&interface=ui).
   * **Tags** - Optionally, add user tags. User tags are visible account-wide. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Access management tags** - Optionally, add access management tags to help organize access control relationships. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).

1. Copy a **sample API call** (optional).
1. Click **Create**.
1. If you don't have a paid account, **Upgrade your account** might appear instead of **Create**. If this option is shown, click to update your account to a paid account before creating your public gateway. For more information, see [Upgrading your account](/docs/account?topic=account-upgrading-account).

## Creating a public gateway from the CLI
{: #pg-creating-cli}
{: cli}

Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet.

To create a public gateway, run the following command:

```sh
ibmcloud is public-gateway-create my-gateway $vpc us-south-3
```
{: pre}

From the output that's returned, save the ID in a variable so you can use it later, for example:

```sh
gateway="0738-446c0c63-f0b1-4043-b30d-644f55fde391"
```
{: pre}

To attach the public gateway to your subnet, run the following command:

```sh
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}

Only one public gateway per zone is allowed in a VPC, but that public gateway can be attached to multiple subnets in the zone. To find the public gateway for a zone, run the `ibmcloud is public-gateways` command and look for the particular VPC and zone values.
{: tip}

## Creating a public gateway with the API
{: #pg-creating-api}
{: api}

Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet.

Create a public gateway for the zone:

```bash
curl -X POST "$vpc_api_endpoint/v1/public_gateways?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-gateway",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Save the ID of the public gateway in a variable.

```bash
gateway="0738-ad0cded3-53a3-4d4a-9809-8c59b50d2b80"
```
{: pre}

Attach the public gateway to your subnet.

```bash
curl -X PUT "$vpc_api_endpoint/v1/subnets/$subnet/public_gateway?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

Only one public gateway per zone is allowed in a VPC, but that public gateway can be attached to multiple subnets in the zone. To find the public gateway for a zone, run the 'ibmcloud is public-gateways` command and look for the particular VPC and zone values.
{: tip}

You can then retrieve and view the public gateway that is attached to the subnet by running the following command.

```bash
curl -X GET "$vpc_api_endpoint/v1/subnets/$subnet/public_gateway?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

## Next steps
{: #pg-next-steps}

- [Viewing resources associated with a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console&interface=cli#vpc-layout)
