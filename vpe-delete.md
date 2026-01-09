---

copyright:
  years: 2020, 2026
lastupdated: "2026-01-09"

keywords: virtual private endpoints, delete, VPE, endpoint gateway

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting an endpoint gateway
{: #vpe-deleting-ui-cli-api}

You can delete an endpoint gateway by using the UI, the CLI, or the API.
{: shortdesc}

## Deleting an endpoint gateway in the console
{: #vpe-deleting-ui}
{: ui}

To delete an endpoint gateway, follow these steps:

1. Navigate to the list of endpoint gateways for VPC. From the [{{site.data.keyword.cloud}} console](/login){: external}, select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Virtual private endpoint gateways**.

   The Virtual private endpoint gateways for VPC page shows all endpoint gateways for all VPCs in the region.

1. Click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") next to the endpoint gateway that you want to delete, then click **Delete**. Click **Delete** again to confirm the deletion.

If you previously selected the auto-release option for attached IP addresses, any attached reserved IPs are also released.
{: note}

## Deleting an endpoint gateway from the CLI
{: #vpe-deleting-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To delete an endpoint gateway from the CLI, run the following command:

```sh
ibmcloud is endpoint-gateway-delete ENDPOINT_GATEWAY [-f, --force]
```
{: pre}

Where:

* **ENDPOINT_GATEWAY** is the ID of the endpoint gateway.
* **-f, --force** forces the operation without confirmation.



## Deleting an endpoint gateway with the API
{: #vpe-deleting-api}
{: api}

To delete an endpoint gateway with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store your `EndpointGatewayId` in a variable to be used in the API command:

    ```sh
    export EndpointGatewayId=<endpoint_gateway_id>
    ```
    {: pre}

1. Delete the endpoint gateway:

   ```sh
   curl -X DELETE -sH "Authorization:${iam_token}"
   "$vpc_api_endpoint/v1/endpoint_gateways/$EndpointGatewayId?version=$api_version&generation=2"
   ```
   {: pre}
