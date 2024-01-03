---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-18"

keywords: virtual private endpoints, list, VPE, endpoint gateway

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Listing endpoint gateways in the region
{: #vpe-listing-endpoint-gateways}

You can list all virtual private endpoint gateways in the region by using the UI, CLI, or API.
{: shortdesc}

## Listing endpoint gateways in the UI
{: #vpe-listing-endpoint-gateways-ui}
{: ui}

To list all endpoint gateways by using the {{site.data.keyword.cloud_notm}} console:

From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation Menu** icon![menu icon](/images/menu_icon.png), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>Virtual private endpoint gateways** in the Network section.

The Virtual private endpoint gateways for VPC page appears. Use this table to view endpoint gateways in the region.

## Listing endpoint gateways from the CLI
{: #vpe-listing-endpoint-gateways-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To list all endpoint gateways in the region from the CLI, run the following command:

```sh
ibmcloud is endpoint-gateways \
   [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]
```
{: pre}

Where:

* **--resource-group-id** is the ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
* **--resource-group-name** is the name of the resource group. This option is mutually exclusive with **--resource-group-id**.
* **--json** formats output in JSON.

## Listing endpoint gateways with the API
{: #vpe-listing-endpoint-gateways-api}
{: api}

To list endpoint gateways with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store your `EndpointGatewayId` in a variable to be used in the API command:

    ```sh
    export EndpointGatewayId=<endpoint_gateway_id>
    ```
    {: pre}

1. List endpoint gateways:

   ```sh
   curl  -sH "Authorization:${iam_token}"
   "$vpc_api_endpoint/v1/endpoint_gateways?version=$api_version&generation=2"
   ```
   {: pre}
