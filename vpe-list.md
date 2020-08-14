---

copyright:
  years: 2020
lastupdated: "2020-08-10"

keywords: virtual private endpoint, list, listing, vpe, endpoint gateways

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

# Listing endpoint gateways in the region (Beta)
{: #vpe-listing-endpoint-gateways}

You can list all {{site.data.keyword.cloud}} Virtual Private Endpoint (VPE) for VPC endpoint gateways in the region using the UI, CLI, or API.
{: shortdesc}

## Using the UI
{: #vpe-listing-endpoint-gateways-ui}

To list all endpoint gateways using the IBM Cloud console:

From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to Menu icon ![Menu icon](/images/menu_icon.png) > **VPC Infrastructure > Endpoint gateways** under the Network section.

The Endpoint gateways for VPC page appears. Use this table to view endpoint gateways in the region.

  ![Endpoint gateways for VPC page](./images/vpe-dashboard.png "Endpoint gateways for VPC page")

## Using the CLI
{: #vpe-listing-endpoint-gateways-cli}

To list all endpoint gateways in the region using the CLI, run the following command:

```
ibmcloud is endpoint-gateways \
   [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]
```

Where:

* **--resource-group-id** is the ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
* **--resource-group-name** is the name of the resource group. This option is mutually exclusive with **--resource-group-id**.
* **--json** formats output in JSON.

## Using the API
{: #vpe-listing-endpoint-gateways-api}

To list endpoint gateways:

```sh
curl  -sH "Authorization:${iam_token}"
"$vpc_api_endpoint/v1/endpoint_gateways?version=$api_version&generation=2"
```
{: pre}
