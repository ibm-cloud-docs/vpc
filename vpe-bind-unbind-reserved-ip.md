---

copyright:
  years: 2020, 2024
lastupdated: "2024-06-20"

keywords: virtual private endpoints, bind, unbind, reserved IP, VPE, endpoint gateways

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Binding and unbinding a reserved IP address
{: #bind-unbind-reserved-ip}
{: help}
{: support}

You can bind and unbind a reserved IP address to an endpoint gateway by using the UI, CLI, and API.
{: shortdesc}

## Binding and unbinding a reserved IP address in the UI
{: #bind-reserved-ip-ui}
{: ui}

You can bind or unbind a reserved IP address to an endpoint gateway by using the UI.

### Binding a reserved IP address
{: #bind-reserved-ip}

To reserve or bind an IP address by using the {{site.data.keyword.cloud}} console:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation Menu** icon ![menu icon](images/menu_icon.png), then click > **VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Virtual private endpoint gateways** in the Network section. The Virtual private endpoint gateways for VPC page appears.
1. Highlight the row of the gateway in the table, then click **Reserve or bind IP** from the Actions menu ![Actions menu](images/overflow.png). Alternatively, you can click the gateway name and access this link from the endpoint gateway's details page.

   If you did not reserve or bind an IP during endpoint gateway creation, this link appears in the IP address column in the table.
   {: note}

1. From the Reserved IP side panel, have IBM select an IP address for you from the subnet that is listed, or select from existing IPs.

   You can bind only one reserved IP to one endpoint gateway from each zone. To bind a reserved IP to an endpoint gateway, you must have an existing subnet. You need to also make sure that no subnets of the VPC that are bound to the same zone.
   {: important}

1. Specify whether you want to automatically delete the reserved IP if the endpoint gateway is deleted. Then, click **Reserve IP address** to bind the address to this endpoint gateway.

### Unbinding a reserved IP address
{: #unbind-reserved-ip}

Unbinding means that the selected reserved IP is no longer bound to the endpoint gateway. However, the IP address is still reserved and available to be bound again.
{: shortdesc}

To unbind an IP address by using the IBM Cloud console, follow these steps:

1. From the Virtual private endpoint gateways for VPC page, highlight the row of the gateway in the table, then click **Unbind IP** from the Actions menu ![Actions menu](images/overflow.png). Alternatively, you can unbind a reserved IP from an endpoint gateway's details page.
1. Click **Unbind IP** to confirm that you want to unbind this IP from the specified subnet.

## Binding and unbinding a reserved IP address from the CLI
{: #vpe-binding-unbinding-endpoint-gateway-cli}
{: cli}

You can bind or unbind a reserved IP address by using the CLI.

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

### Binding a reserved IP to an endpoint gateway
{: #vpe-binding-endpoint-gateway-cli}

To bind a reserved IP to an endpoint gateway from the CLI, run the following command:

```sh
  ibmcloud is endpoint-gateway-reserved-ip-bind ENDPOINT_GATEWAY \
    --reserved-ip-id RESERVED_IP_ID [--json]
```
{: pre}

Where:

* **ENDPOINT_GATEWAY** is the ID of the endpoint gateway.
* **--reserved-ip-id** is the reserved IP identity to be used as the primary IP for the network interface. If not specified, an available address on the subnet is selected and reserved automatically.
* **--json** forces the operation without confirmation.

### Unbinding a reserved IP to an endpoint gateway by using the CLI
{: #vpe-unbinding-endpoint-gateway-cli}

To unbind a reserved IP to an endpoint gateway from the CLI, run the following command:

```sh
  ibmcloud is endpoint-gateway-reserved-ip-unbind ENDPOINT_GATEWAY \
  (--address ADDRESS | --reserved-ip-id RESERVED_IP_ID) [-f, --force]
```
{: pre}

Where:

* **ENDPOINT_GATEWAY** is the ID of the endpoint gateway.
* **--address** is the reserved IP address to be unbound.
* **--reserved-ip-id** is the ID of the reserved IP address to be unbound.
* **-f, --force** forces the operation without confirmation.

## Binding and unbinding a reserved IP address with the API
{: #vpe-bind-unbind-api}
{: api}

To bind or unbind a reserved IP address with the API, perform the following prerequisites and procedures.

### Prerequisites
{: #vpe-prerequisites}

The following prerequisites must be met before you can use the API to bind or unbind reserved IP addresses:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to use in the API command:

    ```sh
    export EndpointGatewayId=<endpoint_gateway_id>
    export ReservedIPId=<reserved_ip_id>
    ```
    {: codeblock}

### Binding a reserved IP to an endpoint gateway
{: #vpe-binding-endpoint-gateway-api}

To bind an endpoint gateway for the specific VPC, see the following example:

```sh
curl -X PUT
  -sH "Authorization:${iam_token}"
  "$vpc_api_endpoint/v1/endpoint_gateways/$EndpointGatewayId/ips/$ReservedIPId?version=$api_version&generation=2"
```
{: pre}

### Unbinding a reserved IP to an endpoint gateway
{: #vpe-unbinding-endpoint-gateway-api}

To unbind an endpoint gateway for the specific VPC, see the following example:

```sh
curl -X DELETE
  -sH "Authorization:${iam_token}"
  "$vpc_api_endpoint/v1/endpoint_gateways/$EndpointGatewayId/ips/$ReservedIPId?version=$api_version&generation=2"
```
{: pre}
