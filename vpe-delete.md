---

copyright:
  years: 2020
lastupdated: "2020-10-30"

keywords: virtual private endpoints, delete, VPE, endpoint gateway

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:term: .term}
{:generic: data-hd-programlang="generic"}
{:download: .download}

# Deleting an endpoint gateway
{: #vpe-deleting-ui-cli-api}

You can delete an endpoint gateway by using the UI, the CLI, or the API.  
{: shortdesc}

## Deleting an endpoint gateway using the UI
{: #vpe-deleting-ui}

To delete an endpoint gateway, follow these steps:

1. Navigate to the list of endpoint gateways for VPC. From the [{{site.data.keyword.cloud}} console](https://{DomainName}/vpc-ext){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Virtual private endpoint gateways** in the Network section.

   The Virtual private endpoint gateways for VPC page shows all endpoint gateways for all VPCs in the region.

2. Click the overflow menu ![overflow menu](images/overflow.png) next to the endpoint gateway you want to delete, then click **Delete**. Click **Delete** again to confirm the deletion.

If you previously selected the auto-delete option for attached IP addresses, any attached reserved IPs are also deleted.
{: note}

## Deleting an endpoint gateway using the CLI
{: #vpe-deleting-cli}

To delete an endpoint gateway by using the CLI, run the following command:

```
ibmcloud is endpoint-gateway-delete ENDPOINT_GATEWAY [-f, --force]
```

Where:

* **ENDPOINT_GATEWAY** is the ID of the endpoint gateway.
* **--force, -f** forces the operation without confirmation.

## Deleting an endpoint gateway using the API
{: #vpe-deleting-api}

To delete an endpoint gateway by using the API, follow these steps:  

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store your `EndpointGatewayId` in a variable to be used in the API command:

    ```sh
    export EndpointGatewayId=<endpoint_gateway_id>
    ```
    {:pre}      

1. Delete the endpoint gateway:

   ```sh
   curl -X DELETE -sH "Authorization:${iam_token}"
   "$vpc_api_endpoint/v1/endpoint_gateways/$EndpointGatewayId?version=$api_version&generation=2"
   ```
   {:pre}
