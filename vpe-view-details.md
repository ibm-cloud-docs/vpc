---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-24"

keywords: virtual private endpoints, view details, VPE, endpoint gateway

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing details of an endpoint gateway
{: #vpe-viewing-details-of-an-endpoint-gateway}

You can view details about a specific endpoint gateway, or see a summary of endpoint gateways for VPC by using the console, CLI, or API.
{: shortdesc}

Only services that support Virtual Private Endpoints for VPC show up in the list of endpoint gateway targets.
{: note}

## Viewing details of an endpoint gateway in the UI
{: #viewing-details-of-an-endpoint-gateway-using-the-ui}
{: ui}

To view details of an endpoint gateway by using the {{site.data.keyword.cloud}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Virtual private endpoint gateways**.

   The Virtual private endpoint gateways for VPC page shows all endpoint gateways for all VPCs in the region.

1. To view the details of a specific gateway, click an individual gateway name.

   From the details page, you can view additional information about the endpoint gateway, view its bound reserved IP addresses and their subnets, delete an endpoint gateway, attach a security group, and more.

Descriptions of the fields for the virtual endpoint gateway details page are:

| Field | Description |
|-------|-------------|
| Name | The name of the endpoint gateway. Click the Edit icon ![Edit icon](images/edit.png) to update the name.|
| Resource group | The resource group of the endpoint gateway. |
| Virtual private cloud | The VPC where the endpoint gateway is located. Click to see the VPC's details.|
| Subnets | The bound reserved IP addresses' subnets to this endpoint gateway. Click to see the subnet's details. |
| Service name | The service offering name of the target for this endpoint gateway. |
| Service instance name | The service instance name of the service target for this endpoint gateway. |
| Service endpoint | The fully qualified domain name for the target service. |
| Target CRN | The service instance's target CRN value. |
{: caption="Details about your endpoint gateway" caption-side="bottom"}

From the Attached resources section, you can select to view either Reserved IP or Security groups. Click **Manage attached resources** to switch to the Attached resources view.

The Reserved IPs table shows all bound, reserved IP addresses to this endpoint gateway. Descriptions of the fields in the Reserved IPs table are:

| Field | Description |
|-------|-------------|
| Name | The reserved IP name. |
| Subnet | The subnet name of the reserved IP. Click to see the subnet's details. |
| Location | The reserved IP's location and subnet. |
| IP address | The IP address value of the reserved IP. Click to copy it. |
| Auto delete | Move the **Auto Delete** switch to enable or disable deletion for the reserved IP. If it is enabled (green), this reserved IP is deleted automatically when the target is deleted. |
| Actions | Click the Actions icon ![Actions menu](../icons/action-menu-icon.svg "Actions") to display a menu of context-specific actions that you can take. The menu contains 4 actions: Rename, Release, Unbind, and Copy UUID for this reserved IP. |
{: caption="Details about your endpoint gateway bound reserved IPs" caption-side="bottom"}

Scroll to the Security groups section to view attached security groups. You can click **Attach** to attach a security group, or click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") to edit rules, or detach a security group.

   At least one security group must be attached.
   {: note}

## Viewing details of an endpoint gateway from the CLI
{: #vpe-viewing-details-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To view details of an endpoint gateway from the CLI, enter the following command:

```sh
  ibmcloud is endpoint-gateway ENDPOINT_GATEWAY [--json]
```
{: pre}

Where:

* `ENDPOINT_GATEWAY` is the ID of the endpoint gateway.
* `--json` formats the output in JSON format.




## Viewing details of an endpoint gateway with the API
{: #vpe-viewing-details-api}
{: api}

To view details of an endpoint gateway with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API command:

    ```sh
    export EndpointGatewayId=<endpoint_gateway_id>
    ```
    {: codeblock}

1. When the variables are initiated, view details of the endpoint gateway:

   ```sh
   curl  -sH "Authorization:${iam_token}"
     "$vpc_api_endpoint/v1/endpoint_gateways/$EndpointGatewayId?version=$api_version&generation=2"
   ```
   {: pre}
