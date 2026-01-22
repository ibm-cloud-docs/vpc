---

copyright:
  years: 2020, 2026
lastupdated: "2026-01-22"

keywords: virtual private endpoints, VPE, endpoint gateway

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an endpoint gateway
{: #ordering-endpoint-gateway}
{: help}
{: support}

You can create an endpoint gateway for an {{site.data.keyword.cloud}} or third-party service or application that you want to access on your private VPC network. You can use the console, CLI, API, or Terraform.
{: shortdesc}

To create a local-access endpoint gateway (currently supported only for Cloud Object Storage), follow the instructions in [Creating a local-access virtual private endpoint gateway](/docs/vpc?topic=vpc-create-local-access-vpe). To set up a cross-account endpoint gateway to access a supported IBM Cloud or third-party service or application, see [Creating a cross-account endpoint gateway](/docs/vpc?topic=vpc-ordering-cross-account-endpoint-gateway&interface=ui) for details.
{: attention}

## Before you begin
{: #vpe-before-you-begin}

Before creating an endpoint gateway, ensure that you review [Planning for virtual private endpoint gateways](/docs/vpc?topic=vpc-vpe-planning-considerations) and have met the following prerequisites:

* A VPC
* A subnet in at least one availability zone if you intend on binding an IP address at the same time you provision the endpoint gateway
* An instance of the service
* Appropriate [IAM permissions](/docs/vpc?topic=vpc-vpe-iam) to create an endpoint gateway, create or bind a reserved IP, and view or list the target service
* Verification that the service you are configuring is enabled for VPE
* If you plan to create a cross-account VPE, make sure that you have met the following requirements:

   * Make sure that you have the necessary IAM permissions and Context-Based Restriction (CBR) policies to authorize access.
   * Make sure that your target service supports cross-account VPEs. Check the service's documentation for details.
   * You must obtain the Cloud Resource Name (CRN) of the target service instance.
   * To authorize a VPE in your accessor account to use the target service instance that resides in a different IBM Cloud account, you must create a service-to-service authorization policy as described in the next section.

## Creating an endpoint gateway in the console
{: #vpe-creating-ui}
{: ui}

To create an endpoint gateway in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Virtual private endpoint gateways**. The Virtual private endpoint gateways for VPC page appears.
1. Click **Create** to go to the provisioning page.
1. In the Details section, enter values for the following fields:
   * **Name** - Type a unique name for your endpoint gateway.
   * **Resource group** - Select a resource group for the endpoint gateway. You can use the default group for this endpoint gateway, or choose from the list. For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).
   * **Tags** - Optionally, add tags to organize, track usage costs, or manage access to your resources.
   * **Access management tags** - You can also optionally add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Virtual private cloud** - Select the VPC where you need the VPE IP address. Based on the region of the VPC you selected, the system provides you with a list of IBM services that you can access to associate with the endpoint gateway. If the target service is not offered in the region of the VPC you selected, you can change the region to select the service in another region.

    If the intended service or service instance does not appear, revalidate your IAM permissions. You can also choose to allocate a reserved IP address to bind to the gateway, or specify a VPC subnet to allocate a reserved IP address for binding to the gateway.
    {: tip}

1. In the Security groups section, select the checkboxes of the security groups that you want to attach from the security group table. You can then manage these security groups and tighten their security rules for inbound traffic toward your endpoint gateways.

    When you create an endpoint gateway without specifying a security group, the VPC default security group is attached to the endpoint gateway. For more information, see [Configuring ACLs and security groups for use with endpoint gateways](/docs/vpc?topic=vpc-configure-acls-sgs-endpoint-gateways).

   Endpoint gateways created prior to the support for security groups do not have an attached security group. They also allow all inbound traffic. If you attach a security group to a VPE that does not have any attached security groups, you cannot revert that VPE back to a state where is has no security groups. You can revert to the previous "allow all inbound traffic" behavior by attaching a security group with rules for allowing all inbound traffic. However, this  rule is inherently less secure than having a more restrictive security group in place and is not recommended.
   {: important}

1. In the **Request connection to a service** section, select either an {{site.data.keyword.cloud_notm}} or non-{{site.data.keyword.cloud_notm}} service to access using this endpoint gateway.

   * For IBM Cloud services, select an available IBM Cloud service offering from the menu, then select its region. A region is pre-selected to optimize performance.
   * For non-IBM Cloud services, enter the cloud resource name (CRN) of the Private Path service (obtained from your service provider).

      Your connection request is sent to the service provider for review.

      The review might be automated based on the provider's chosen account policy, or it could take days if the provider has chosen to manually review their requests. If your request is permitted, you will receive notification and can then access the service. If your request is denied, contact the service provider.
      {: note}


1. In the Share DNS section, select one of the following DNS resolution binding modes:

   * **Primary** allows the VPE gateway's service endpoints in the DNS-shared VPC to be shared with the DNS hub VPC for name resolution.
   * **Disabled** does not allow the VPE gateway's service endpoints to be shared for name resolution.

   When the VPC is a DNS hub, DNS sharing is always set to **Primary** and the mode can't be changed.
   {: note}

1. In the Reserved IP section, select reserved IP addresses. You can choose:

   * **Select one for me** - Enter a name prefix for the IP addresses, or one is chosen for you. Select one or more subnets (one subnet per zone is recommended) and enable the Auto-release toggle if you want this reserved IP to be deleted automatically if the endpoint gateway is deleted.

   * **Select from existing IPs** - Choose from the list of existing IP addresses. If no IPs exist, click **Reserve IP** to reserve one.

      Only one reserved IP can be bound to a VPE gateway per AZ in a VPC. Ensure that no other bindings exist in the same AZ across all subnets within the VPC.
      {: note}

   * **Select one later** - Select IP addresses at some point after creating the endpoint gateway.

1. Review the **Order summary**, then click **Create virtual private endpoint gateway**. The endpoint gateway is requested for use.

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

1. Create an endpoint gateway by running the following command:<

   ```sh
   ibmcloud is endpoint-gateway-create --target TARGET [--target-type private_path_service_gateway | provider_cloud_service | provider_infrastructure_service] [--vpc VPC] [--name NAME] [--rip RIP --subnet SUBNET | (--new-reserved-ip NEW_RESERVED_IP1 --new-reserved-ip NEW_RESERVED_IP2 ...)] [--allow-dns-resolution-binding false | true] [--sg SG] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
   ```
   {: codeblock}

   Where:

   `--target-type`:
   :   Indicates the type of target for this endpoint gateway, and one of the following: `private_path_service_gateway`, `provider_cloud_service`, or `provider_infrastructure_service`.

   `--vpc`:
   :   Indicates ID or name of the VPC.

   `--target`:
   :   Indicates the name of the provider infrastructure service or the CRN for a provider cloud service instance. You can use the command `ibmcloud is endpoint-gateway-targets` to list the provider cloud and infrastructure services that are qualified to be set as the endpoint gateway target.

   `--name`:
   :   Indicates the new name of the endpoint gateway.

   `--rip`:
   :   Indicates the ID of the reserved IP to be bound to the endpoint gateway.

   `--subnet`:
   :   ID or name of the subnet. This name is required only if the supplied reserved IP is in name format.

   `--new-reserved-ip`:
   :   Indicates the new reserved IP configuration in JSON format or a JSON file, `RESERVED_IP_JSON|@RESERVED_IP_JSON_FILE`.

   `--sg`:
   :   Indicates the ID of the security group.

    If you do not specify `-sg`, the VPC default security group is bound to the endpoint gateway.
    {: note}

   `--resource-group-id`:
   :   Indicates the ID of the resource group. This option is mutually exclusive with `--resource-group-name`.

   `--resource-group-name`:
   :   Indicates the name of the resource group. This option is mutually exclusive with `--resource-group-id`.

   `--output`:
   :   Indicates the output format. Only JSON is supported.

   `-q, --quiet`:
   :   Suppresses verbose output.

   For more information and command examples, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli#endpoint-gateway-create).

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

   * **VpcId** - Use the `list vpc` command (with the preceding variables) and then populate the variable based on the provided ID:

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

1. When all variables are initiated, select one of the following actions:

   * Create an endpoint gateway for the specific VPC:

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

   * Create an endpoint gateway with an associated reserved IP address:

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

      When creating an endpoint gateway to connect to a Private Path service, make sure that '"resource type" is set to `private_path_service_gateway`.
      {: note}

After an endpoint gateway is created for an {{site.data.keyword.cloud_notm}} service, it presents the endpoints associated with it. You can edit the endpoint gateway in various ways, such as binding/unbinding IPs, changing the resource group, and updating tags. If you need to change the service endpoints for any reason, you must delete and recreate the endpoint gateway.
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

If you return to the Virtual private endpoint gateways for VPC page, your endpoint gateway shows in the table.

* For IBM Cloud services, the status of your endpoint gateway changes from `Updating` to `Stable`. You can click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") to rename, reserve or bind an IP, unbind an IP, or delete an endpoint gateway.
* For non-IBM Cloud (third party) services, your endpoint gateway status is `Pending` until the service provider either permits or denies your connect request. The status changes to `Stable` when connectivity is established. You can click **Track** or expand the table row of the endpoint gateway to view the progress of your request.
* Optionally, there is a DNS resolution binding switch in the table row of the VPE gateway that allows you to enable or disable DNS sharing for this endpoint gateway. For more information, see [About DNS sharing for VPE gateways](/docs/vpc?topic=vpc-vpe-dns-sharing).

## Related links
{: #vpe-create-related-links}

* [Creating a local-access endpoint gateway](/docs/vpc?topic=vpc-create-local-access-vpe)
* [Creating a cross-account endpoint gateway](/docs/vpc?topic=vpc-ordering-cross-account-endpoint-gateway&interface=terraform)
* [Planning for virtual private endpoint gateways](/docs/vpc?topic=vpc-vpe-planning-considerations)
