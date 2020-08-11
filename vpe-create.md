---

copyright:
  years: 2020
lastupdated: "2020-08-10"

keywords: flow logs, ordering, getting started

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

You can create an endpoint gateway for an IBM Cloud service that you want to access on your private VPC network by using the UI, CLI, or API.
{: shortdesc}

## Before you begin checklist
{: #vpe-before-you-begin}

Prior to creating an endpoint gateway, ensure that you review and have satisfied the following conditions:

### Required

* An existing VPC virtual network in the target region
* A unique name for the endpoint gateway
* A subnet in at least one availability zone if you intend on binding an IP address at the time the endpoint gateway is provisioned
* An instance of the service
* Appropriate [IAM permissions](/docs/vpc?topic=vpc-vpe-iam) to create an endpoint gateway, create or bind a reserved IP, and view/list the target service
* Verify support for VPEs and best practices in the individual IBM service documentation

### Optional

* Custom resource group
* Specific IP from the target subnet
* Tags

## Using the UI
{: #vpe-creating-ui}

To create an endpoint gateway by using the IBM Cloud console, follow these steps:
{: shortdesc}

1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to Menu icon ![Menu icon](/images/menu_icon.png) > **VPC Infrastructure > Endpoint gateways** under the Network section. The Endpoint gateways for VPC page appears.

  ![Endpoint gateways for VPC page](./images/vpe-dashboard.png "Endpoint gateways for VPC page")

2. Click **New endpoint gateway** to go to the IBM Cloud Endpoint Gateways provisioning page.
3. In the Details section, enter values for the following fields:

   * **Name** - Type a unique name for your endpoint gateway.
   * **Virtual private cloud** - Select a VPC from the available list where the VPE IP address is needed. You are provided with a list of IBM services that you have access to within the region of the VPC virtual network to associate with the endpoint gateway. If the target service is outside the region displayed, you can change the **VPC Region** for an updated list of services.

      If the intended service or service instance is not displayed, revalidate your IAM permissions. You can also choose to allocate a reserved IP address to bind to the gateway, or specify a VPC subnet to allocate a reserved IP address from to bind to the gateway.
      {: note}

   * **Resource group** - Select a resource group for the endpoint gateway. You can use the default group for this endpoint gateway, or choose from the list (if defined). For more information, see [Best practices for organizing resources in a resource group](/docs/resources?topic=resources-bp_resourcegroups).

4. Select the IBM Cloud service you want to access using this endpoint gateway.  

5. Select a reserved IP address. You can:   

   * **Select one for me** - Enter a name for the IP address, or one is chosen for you. Select a subnet from the list and indicate if you want this reserved IP to be deleted automatically if the endpoint gateway is deleted.
   * **Select from existing IPs** - Choose from a list of existing IP addresses.
   * **Select one later** - Select an IP address after creating the endpoint gateway.  

6. An order summary shows pricing estimates for your review. Review and click **Create endpoint gateway**.

     ![Create an endpoint gateway ordering page](./images/vpe-create.png "Create an endpoint gateway ordering page")

## Using the CLI
{: #vpe-ordering-cli}

To create an endpoint gateway by using the CLI, follow these steps:
{:shortdesc}

1. List the IBM Cloud service instances that are qualified to be set as endpoint gateway target:

```
ibmcloud is endpoint-gateway-targets
```

2. Create an endpoint gateway by running the following command:

  ```
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

To create an endpoint gateway using the API, follow these steps:
{: shortdesc}

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with
the right variables.
2. Store the following variables to be used in the API commands:

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

   * `TargetCrn` - Is the name, or CRN, of the instance of the IBM Cloud service where you want to set the endpoint gateway. You can use the command `ibmcloud is endpoint-gateway-targets` to list the IBM Cloud service instances that are qualified to be set as endpoint gateway target.  

    ```sh
    export TargetCrn=<CRN of a target service>
    ```
    {: pre}

   * `SubnetID` (Optional) - The subnet ID.

     ```sh
     export SubnetID=<your_subnet_id>
     ```
     {: pre}

2. When all variables are initiated, do one of the following:

   * Create an endpoint gateway for the specific VPC:

      ```sh
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
      {: pre}

   * Or create an endpoint gateway with an associated reserved IP address:

      ```sh
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
      {: pre}

## Next steps

After you create an endpoint gateway for your service instance and assign a reserved IP address, you must bind the IP addresses from your VPC network to the endpoint gateway. See [Binding and unbinding a reserved IP address](/docs/vpc?topic=vpc-bind-unbind-reserved-ip) for details. These "bound" IP addresses become the VPE to access the service mapped to the endpoint gateway.

After an endpoint gateway is created for an IBM Cloud service, it presents the endpoints associated with the IBM Cloud service. You can perform edits on the endpoint gateway such as binding/unbinding IPs, changing the resource group, and updating tags. If you need to change the service endpoints for any reason, you must delete and recreate the endpoint gateway.
{: important}

## Accessing your virtual private endpoint after setting up your endpoint gateway

You can access an IBM service using VPE in the following ways:

* If you are currently accessing an IBM service using a URL, you can use your service endpoint to access the service.  
* If the service you are accessing doesn't have a service endpoint, you can use any IP address bound to your endpoint gateway to access it.

Currently, only the IBM Domain Name System (DNS) and Network Time Protocol (NTP) services are supported.
{: note}
