---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-09"

keywords: virtual private endpoints, view details, VPE, endpoint gateway
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing details of an endpoint gateway
{: #vpe-viewing-details-of-an-endpoint-gateway}

You can view details about a specific endpoint gateway, or see a summary of endpoint gateways for VPC by using the UI, CLI, or API.
{: shortdesc}

Only services that support Virtual Private Endpoints for VPC show up in the list of endpoint gateway targets.
{: note}

## Viewing details of an endpoint gateway using the UI
{: ui}

To view details of an endpoint gateway by using the {{site.data.keyword.cloud}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Virtual private endpoint gateways** in the Network section.

   The Virtual private endpoint gateways for VPC page shows all endpoint gateways for all VPCs in the region.

1. To view the details of a specific gateway, click an individual gateway name. From the details page, you can view additional information about the endpoint gateway, view its bound reserved IPs and their subnets, delete an endpoint gateway, and more.

   ![Endpoint gateway details page](./images/vpe-details.png "Endpoint gateway details page")

Descriptions of the fields in the endpoint gateway details are:

| Field | Description |
|-------|-------------|
| Name | The name of the endpoint gateway. Click on the pencil icon to update the name.|
| Resource group | The resource group of the endpoint gateway. |
| Service | The service offering name of the target for this endpoint gateway. |
| Service instance | The service instance name of the service target for this endpoint gateway. |
| Service private endpoint | The fully qualified domain name for the target service. |
| Virtual Private Cloud | The VPC where the endpoint gateway is located. Click on the name to view details of the VPC.|
| CRN | The service instance's CRN value. |
| Subnets | The bound reserved IP addresses' subnets to this endpoint gateway. |
{: caption="Table 1. Details about your endpoint gateway" caption-side="top"}

The Reserved IPs table shows all bound, reserved IPs to this endpoint gateway. Descriptions of the fields in the Reserved IPs table are:

| Field | Description |
|-------|-------------|
| Name | The reserved IP name. |
| Subnet | The subnet name of the reserved IP. Click to see the subnet's details. |
| Location | The reserved IP's location and subnet. |
| IP address | The IP address value of the reserved IP. Click to copy it. |
| Auto delete | Move the **Auto Delete** switch to enable or disable deletion for the reserved IP. If it is enabled (green), this reserved IP is deleted automatically when the target is deleted. |
| Actions | Click the overflow menu ![overflow menu](images/overflow.png) to display a menu of context-specific actions that you can take. There are 4 actions in the menu (Rename, Release, Unbind and Copy UUID for this reserved IP). |
{: caption="Table 2. Details about your endpoint gateway bound reserved IPs" caption-side="top"}

## Viewing details of an endpoint gateway using the CLI
{: #vpe-viewing-details-cli}
{: cli}

To view details of an endpoint gateway by using the CLI, enter the following command:

```
  ibmcloud is endpoint-gateway ENDPOINT_GATEWAY [--json]  
```
{: pre}

Where:

* **ENDPOINT_GATEWAY** is the ID of the endpoint gateway.
* **--json** formats the output in JSON format.

## Viewing details of an endpoint gateway using the API
{: #vpe-viewing-details-api}
{: api}

To view details of an endpoint gateway by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API command:

    ```
    export EndpointGatewayId=<endpoint_gateway_id>
    ```
    {: codeblock}

1. When the variables are initiated, view details of the endpoint gateway:

   ```sh
   curl  -sH "Authorization:${iam_token}"
     "$vpc_api_endpoint/v1/endpoint_gateways/$EndpointGatewayId?version=$api_version&generation=2"
   ```
   {: pre}
