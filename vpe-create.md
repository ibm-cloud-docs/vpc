---

copyright:
  years: 2020
lastupdated: "2020-10-30"

keywords: virtual private endpoints, vpe, endpoint gateway

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:note: .note}
{:tip: .tip}
{:important: .important}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Creating an endpoint gateway
{: #ordering-endpoint-gateway}
{: help}
{: support}

You can create an endpoint gateway for an {{site.data.keyword.cloud}} service that you want to access on your private VPC network by using the UI, CLI, or API.
{: shortdesc}

## Before you begin
{: #vpe-before-you-begin}

Prior to creating an endpoint gateway, ensure that you review [Planning for virtual private endpoint gateways](/docs/vpc?topic=vpc-planning-considerations) and have satisfied the following conditions:

  * A VPC
  * A subnet in at least one availability zone if you intend on binding an IP address at the same time you provision the endpoint gateway
  * An instance of the service
  * Appropriate [IAM permissions](/docs/vpc?topic=vpc-vpe-iam) to create an endpoint gateway, create or bind a reserved IP, and view or list the target service
  * Verification that the service you are configuring supports VPE 

## Using the UI
{: #vpe-creating-ui}

To create an endpoint gateway by using the {{site.data.keyword.cloud_notm}} console, follow these steps:
{: shortdesc}

1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Virtual private endpoint gateways** in the Network section. The Virtual private endpoint gateways for VPC page appears.

  ![Virtual private endpoint gateways for VPC page](./images/vpe-dashboard.png "Virtual private endpoint gateways for VPC page")

1. Click **Create** to go to the provisioning page.
1. In the Details section, enter values for the following fields:

   * **Name** - Type a unique name for your endpoint gateway.
   * **Virtual private cloud** - Select a VPC from the available list where you need the VPE IP address. The systems provides you with a list of IBM services that you can access within the region of the VPC network to associate with the endpoint gateway. If the target service is outside the VPC's region, you can change it for an updated list of services.

    If the intended service or service instance does not appear, revalidate your IAM permissions. You can also choose to allocate a reserved IP address to bind to the gateway, or specify a VPC subnet to allocate a reserved IP address for binding to the gateway.
    {: note}

   * **Resource group** - Select a resource group for the endpoint gateway. You can use the default group for this endpoint gateway, or choose from the list. For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).

1. Select the {{site.data.keyword.cloud_notm}} service that you want to access using this endpoint gateway. The region is pre-selected to optimize performance.
1. Select a reserved IP address. You can choose:

   * **Select one for me** - Enter a name for the IP address, or one is chosen for you. Select a subnet from the list and indicate if you want this reserved IP to be deleted automatically if the endpoint gateway is deleted.
   * **Select from existing IPs** - Choose from a list of existing IP addresses.
   * **Select one later** - Select an IP address after creating the endpoint gateway.

1. The order Summary shows pricing estimates for your review. Review and click **Create endpoint gateway**.

## Using the CLI
{: #vpe-ordering-cli}

To create an endpoint gateway by using the CLI, follow these steps:

1. List the IBM Cloud service instances that are qualified to be set as endpoint gateway targets:

   ```sh
   ibmcloud is endpoint-gateway-targets
   ```

1. Create an endpoint gateway by running the following command:

  ```sh
  ibmcloud is endpoint-gateway-create \
    --vpc-id VPC_ID \
    --target TARGET [--name NAME] [(--reserved-ip-id RESERVED_IP_ID1 --reserved-ip-id RESERVED_IP_ID2 ...) \
    | (--new-reserved-ip NEW_RESERVED_IP1 --new-reserved-ip NEW_RESERVED_IP2 ...)] \
    [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] \
    [--json]  
  ```

Where:

* **--vpc-id** is the ID of the VPC.
* **--target** is the name, or CRN, for the user's instance of an IBM Cloud service.
* **--name** is the new name of the endpoint gateway.
* **--reserved-ip-id** is the ID of the reserved IP to be bound to the endpoint gateway.
* **-new-reserved-ip: RESERVED_IP_JSON|@RESERVED_IP_JSON_FILE** is the new reserved IP configuration in JSON format or a JSON file.
* **--resource-group-id** is the ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
* **--resource-group-name** is the name of the resource group. This option is mutually exclusive with **--resource-group-id**.
* **--json** is the format output in JSON.

## Using the API
{: #vpe-ordering-api}

To create an endpoint gateway by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API command:

   * `ResourceGroupId` - First, get your resource group and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

   * `VpcId` - Find by using the **list vpc** command (with the preceding variables) and then populate the variable based on the provided ID:

    ```sh
    export VpcId=<your_VPC_id>
    ```
    {: pre}

   * `TargetCrn` - The name, or CRN, of the instance of the IBM Cloud service where you want to set the endpoint gateway. You can use the command `ibmcloud is endpoint-gateway-targets` to list the IBM Cloud service instances that are qualified to be set as endpoint gateway targets.

    ```sh
    export TargetCrn=<CRN of a target service>
    ```
    {: pre}

   * `SubnetID` (Optional) - The subnet ID.

     ```sh
     export SubnetID=<your_subnet_id>
     ```
     {: pre}

1. When all variables are initiated, do one of the following:

   * To create an endpoint gateway for the specific VPC:   

      ```
      curl -X POST\
      -sH "Authorization:${iam_token}"
      "$vpc_api_endpoint/v1/endpoint_gateways?version=$api_version&generation=2" \
      -d { \
      "name": endpoint-gateway-1", \
      "vpc": {"id":"'$VpcId'"}, \
      "target": { \
      "crn": "$TargetCrn", \
      "resource_type": "provider_cloud_service" \
      }, \
      }'
      ```
      {: codeblock}

   * To create an endpoint gateway with an associated reserved IP address:

      ```
      curl -X POST
      -sH "Authorization:${iam_token}"
      "$vpc_api_endpoint/v1/endpoint_gateways?version=$api_version&generation=2" \
      -d { \
      "name": endpoint-gateway-1", \
      "vpc": {"id":"'$VpcId'"}, \
      "target": { \
      "crn": "$TargetCrn", \
      "resource_type": "provider_cloud_service" \
      }, \
      "ips": [ \
      {"subnet": { "id": "$SubnetId" } } \
      ], \
      }'
      ```
      {: codeblock}
      
## Next steps
{: #next-steps-create}

After you create an endpoint gateway for your service instance and assign a reserved IP address, you must bind the IP addresses from your VPC network to the endpoint gateway. See [Binding and unbinding a reserved IP address](/docs/vpc?topic=vpc-bind-unbind-reserved-ip) for details. These bound IP addresses become the VPE to access the service mapped to the endpoint gateway.

After an endpoint gateway is created for an {{site.data.keyword.cloud_notm}} service, it presents the endpoints associated with it. You can edit the endpoint gateway, such as binding/unbinding IPs, changing the resource group, and updating tags. If you need to change the service endpoints for any reason, you must delete and recreate the endpoint gateway.
{: important}
