---

copyright:
  years: 2020, 2024
lastupdated: "2024-2-22"

keywords: virtual private endpoints, VPE, endpoint gateway

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Creating an endpoint gateway
{: #ordering-endpoint-gateway}
{: help}
{: support}

You can create an endpoint gateway for an {{site.data.keyword.cloud}} or third-party service or application (Private Path beta) that you want to access on your private VPC network. You can use the UI, CLI, API, or Terraform.
{: shortdesc}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

## Before you begin
{: #vpe-before-you-begin}

Before creating an endpoint gateway, ensure that you review [Planning for virtual private endpoint gateways](/docs/vpc?topic=vpc-planning-considerations) and have satisfied the following conditions:

* A VPC
* A subnet in at least one availability zone if you intend on binding an IP address at the same time you provision the endpoint gateway
* An instance of the service
* Appropriate [IAM permissions](/docs/vpc?topic=vpc-vpe-iam) to create an endpoint gateway, create or bind a reserved IP, and view or list the target service
* Verification that the service you are configuring is enabled for VPE

## Creating an endpoint gateway in the UI
{: #vpe-creating-ui}
{: ui}

To create an endpoint gateway in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Virtual private endpoint gateways** in the Network section. The Virtual private endpoint gateways for VPC page appears.
1. Click **Create** to go to the provisioning page.
1. In the Details section, enter values for the following fields:
   * **Name** - Type a unique name for your endpoint gateway.
   * **Resource group** - Select a resource group for the endpoint gateway. You can use the default group for this endpoint gateway, or choose from the list. For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).
   * **Tags** - Optionally, add tags to organize, track usage costs, or manage access to your resources.
   * **Access management tags** - You can also optionally add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Virtual private cloud** - Select a VPC from the available list where you need the VPE IP address. The systems provides you with a list of IBM services that you can access within the region of the VPC network to associate with the endpoint gateway. If the target service is outside the VPC's region, you can change it for an updated list of services.

    If the intended service or service instance does not appear, revalidate your IAM permissions. You can also choose to allocate a reserved IP address to bind to the gateway, or specify a VPC subnet to allocate a reserved IP address for binding to the gateway.
    {: tip}

1. In the Security groups section, select the checkboxes of the security groups that you want to attach from the security group table. You can then manage these security groups and tighten their security rules for inbound traffic toward your endpoint gateways.

    When you create an endpoint gateway without specifying a security group, the VPC default security group is attached to the endpoint gateway. For more information, see [Configuring ACLs and security groups for use with endpoint gateways](/docs/vpc?topic=vpc-configure-acls-sgs-endpoint-gateways).

   Endpoint gateways created prior to the support for security groups do not have a security group that is attached. They also allow all inbound traffic. If you attach a security group to a VPE that does not have any attached security groups, you cannot revert that VPE back to having no security groups. You can revert to the previous "allow all inbound traffic" behavior by attaching a security group with rules for allowing all inbound traffic. However, such a rule is inherently less secure than having a more restrictive security group in place, and is not recommended.
   {: important}

1. In the Request connection to a service section, select either an {{site.data.keyword.cloud_notm}} or non-{{site.data.keyword.cloud_notm}} service to access using this endpoint gateway.

   * For IBM Cloud services, select an available IBM Cloud service offering from the menu, then select its region. A region is pre-selected to optimize performance.
   * (Private Path beta only) For non-IBM Cloud services, enter the cloud resource name (CRN) of the Private Path service (obtained from your service provider).

      Your connection request is sent to the service provider for review. The review may be automated based on the provider's chosen accouont policy, or it could take days if the provider has chosen to manually review their requests. If your request is permitted, you will receive notification and can then access the service. If your request is denied, contact the service provider.
      {: note}

1. In the Reserved IP section, select a reserved IP address. You can choose:

   * **Select one for me** - Enter a name for the IP address, or one is chosen for you. Select a subnet from the list and enable the Auto-delete toggle if you want this reserved IP to be deleted automatically if the endpoint gateway is deleted.
   * **Select from existing IPs** - Choose from a list of existing IP addresses.
   * **Select one later** - Select an IP address at some point after creating the endpoint gateway.

1. Review the order Summary, then click **Create virtual private endpoint gateway**. The endpoint gateway is requested for use.

## Creating an endpoint gateway from the CLI
{: #vpe-ordering-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create an endpoint gateway from the CLI, follow these steps:

1. List the IBM Cloud service instances that are qualified to be set as endpoint gateway targets:

   ```sh
   ibmcloud is endpoint-gateway-targets
   ```
   {: pre}

1. Create an endpoint gateway by running the following command:

   ```sh
   ibmcloud is endpoint-gateway-create \
    --vpc VPC_ID \
    --target TARGET [--name NAME] [(--reserved-ip-id RESERVED_IP_ID1 --reserved-ip-id RESERVED_IP_ID2 ...) \
    | (--new-reserved-ip NEW_RESERVED_IP1 --new-reserved-ip NEW_RESERVED_IP2 ...)] \
    [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] \
    [--json]
   ```
   {: codeblock}

   Where:

   `--vpc`
   :   Indicates the ID of the VPC.

   `--target`
   :   Indicates the name, or CRN, of a provider cloud service instance.

1. Create an endpoint gateway by attaching the target type for a Private Path service (Private Path beta release only - requires a feature flag):

   ```sh
   ibmcloud is endpoint-gateway-create (--target-type private_path_service_gateway | provider_cloud_service | provider_infrastructure_service) --target TARGET [--vpc VPC] [--name NAME]    [--rip RIP --subnet SUBNET | (--new-reserved-ip NEW_RESERVED_IP1 --new-reserved-ip NEW_RESERVED_IP2 ...)] [--sg SG] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name        RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
   ```
   {: codeblock}

   Where:

   - **--target-type**: The type of target for this endpoint gateway, and one of the following: **private_path_service_gateway**, **provider_cloud_service**, **provider_infrastructure_service**.
   - **--vpc**: ID or name of the VPC.
   - **--target**: The name of the provider infrastructure service or the CRN for a provider cloud service instance. You can use the command `ibmcloud is endpoint-gateway-targets` to list the provider cloud and infrastructure services that are qualified to be set as the endpoint gateway target.
   - **--name**: New name of the endpoint gateway.

   You can use the command `ibmcloud is endpoint-gateway-targets` to list the provider cloud and infrastructure services that are qualified to be set as endpoint gateway target.
   {: tip}

   * **--name** is the new name of the endpoint gateway.
   * **--reserved-ip-id** is the ID of the reserved IP to be bound to the endpoint gateway.
   * **-new-reserved-ip: RESERVED_IP_JSON|@RESERVED_IP_JSON_FILE** is the new reserved IP configuration in JSON format or a JSON file.
   * **-security-group** is the ID of the security group.

       If you do not specify **-security-group**, the VPC default security group is bound to the endpoint gateway.
       {: note}

   * **--resource-group-id** is the ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
   * **--resource-group-name** is the name of the resource group. This option is mutually exclusive with **--resource-group-id**.
   * **--output** is the output format. Only JSON is supported.
   * **-q, --quiet** suppresses verbose output.

## Creating an endpoint gateway with the API
{: #vpe-ordering-api}
{: api}

To create an endpoint gateway with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Store the following values in variables to be used in the API command:

   * **ResourceGroupId** - First, get your resource group and then populate the variable:

      ```sh
      export ResourceGroupId=<your_resourcegroup_id>
      ```
      {: pre}

   * **VpcId** - Find by using the `list vpc` command (with the preceding variables) and then populate the variable based on the provided ID:

      ```sh
      export VpcId=<your_VPC_id>
      ```
      {: pre}

   * **TargetCrn** - The name, or CRN, of the instance of the IBM Cloud service where you want to set the endpoint gateway. You can use the command `ibmcloud is endpoint-gateway-targets` to list the IBM Cloud service instances that are qualified to be set as endpoint gateway targets.

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

      ```sh
      curl -X POST -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/endpoint_gateways?version=$api_version&generation=2" \
      -d {
      "name": endpoint-gateway-1",
      "vpc": {"id":"'$VpcId'"},
      "target": {
      "crn": "$TargetCrn",
      "resource_type": "provider_cloud_service"
      },
      "security_groups": [
      {"id": "'$sg'"}
      ]
      }'
      ```
      {: codeblock}

      When creating an endpoint gateway to connect to a Private Path service, make sure '"resource type" = "private_path_service_gateway"'.
      {: note}

      After you create an endpoint gateway for your service instance and assign a reserved IP address, you must bind the IP addresses from your VPC network to the endpoint gateway. See [Binding and unbinding a reserved IP address](/docs/vpc?topic=vpc-bind-unbind-reserved-ip) for details. These bound IP addresses become the VPE to access the service mapped to the endpoint gateway.
      {: important}

   * To create an endpoint gateway with an associated reserved IP address:

      ```sh
      curl -X POST -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/endpoint_gateways?version=$api_version&generation=2" \
      -d {
      "name": endpoint-gateway-1",
      "vpc": {"id":"'$VpcId'"},
      "target": {
      "crn": "$TargetCrn",
      "resource_type": "provider_cloud_service"
      },
      "security_groups": [
      {"id": "'$sg'"}
      ],
      "ips": [
      {"subnet": { "id": "$SubnetId" } }
      ],
      }'
      ```
      {: codeblock}

      When creating an endpoint gateway to connect to a Private Path service, make sure that '"resource type" is set to "private_path_service_gateway"'.
      {: note}

After an endpoint gateway is created for an {{site.data.keyword.cloud_notm}} service, it presents the endpoints associated with it. You can edit the endpoint gateway, such as binding/unbinding IPs, changing the resource group, and updating tags. If you need to change the service endpoints for any reason, you must delete and recreate the endpoint gateway.
{: important}

## Creating an endpoint gateway with Terraform
{: #creating-endpoint-gateway-terraform}
{: terraform}

You can use Terraform to create an endpoint gateway.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

The following example creates a VPE gateway:

```terraform
resource "ibm_is_virtual_endpoint_gateway" "example" {

  name = "example-endpoint-gateway"
  target {
    name          = "ibm-ntp-server"
    resource_type = "provider_infrastructure_service"
  }
  vpc            = ibm_is_vpc.example.id
  resource_group = data.ibm_resource_group.example.id
  security_groups = [ibm_is_security_group.example.id]
}

resource "ibm_is_virtual_endpoint_gateway" "example1" {
  name = "example-endpoint-gateway-1"
  target {
    name          = "ibm-ntp-server"
    resource_type = "provider_infrastructure_service"
  }
  vpc = ibm_is_vpc.example.id
  ips {
    subnet = ibm_is_subnet.example.id
    name   = "example-reserved-ip"
  }
  resource_group = data.ibm_resource_group.example.id
  security_groups = [ibm_is_security_group.example.id]
}

resource "ibm_is_virtual_endpoint_gateway" "example3" {
  name = "example-endpoint-gateway-2"
  target {
    name          = "ibm-ntp-server"
    resource_type = "provider_infrastructure_service"
  }
  vpc = ibm_is_vpc.example.id
  ips {
    subnet = ibm_is_subnet.example.id
    name   = "example-reserved-ip"
  }
  resource_group = data.ibm_resource_group.example.id
  security_groups = [ibm_is_security_group.example.id]
}

resource "ibm_is_virtual_endpoint_gateway" "example4" {
  name = "example-endpoint-gateway-3"
  target {
    crn           = "crn:v1:bluemix:public:cloud-object-storage:global:::endpoint:s3.direct.mil01.cloud-object-storage.appdomain.cloud"
    resource_type = "provider_cloud_service"
  }
  vpc            = ibm_is_vpc.example.id
  resource_group = data.ibm_resource_group.example.id
  security_groups = [ibm_is_security_group.example.id]
}
```
{: codeblock}

## After creating a VPE gateway
{: #after-create-vpe-gateway}

If you return to the virtual private endpoint gateways for VPC page, your endpoint gateway shows in the table.

* For IBM Cloud services, the status of your endpoint gateway changes from `Updating` to `Stable`. You can click the Actions menu ![Actions menu](images/overflow.png) to rename, reserve or bind an IP, unbind an IP, or delete an endpoint gateway.
* For non-IBM Cloud (third party) services, your endpoint gateway status is `Pending` until the service provider either permits or denies your connect request. The status changes to `Stable` when connectivity is established. You can click **Track** or expand the table row of the endpoint gateway to view the progress of your request.
* Optionally, there is a DNS resolution binding switch in the table row of the VPE gateway that allows you to enable or disable DNS sharing for this endpoint gateway. For more information, see [About DNS sharing for VPE gateways](/docs/vpc?topic=vpc-hub-spoke-model).
