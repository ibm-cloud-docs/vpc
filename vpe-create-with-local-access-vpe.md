---

copyright:
  years: 2026
lastupdated: "2026-01-09"

keywords: virtual private endpoints, VPE, endpoint gateway

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a local-access endpoint gateway
{: #create-local-access-vpe}
{: help}
{: support} 

In addition to the hub-based DNS sharing model, you can create local-access virtual private endpoint gateways in DNS-shared VPCs. Local-access VPEs provide private, local connectivity to supported IBM Cloud services without routing traffic through the hub VPC.
{: shortdesc}

Currently, this capability is supported only for IBM Cloud Object Storage and is available to select allowlisted customers.
{: attention}

## Before you begin
{: #local-access-vpe-before-you-begin}

* Local-access endpoint gateways use the [DNS sharing model](/docs/vpc?topic=vpc-vpe-dns-sharing&interface=ui), which is based on a hub-and-spoke architecture. You must configure DNS sharing before you create a local-access endpoint gateway. If you already have DNS sharing configured, no changes or migration are required to begin using a local-access endpoint gateway.
* Review [general planning considerations](/docs/vpc?topic=vpc-vpe-planning-considerations) before creating a VPE, including creation limits, security groups, cross-account access, and supported features.
* For prerequisites specific to local-access VPEs in a DNS hub and shared VPC topology, see [DNS sharing planning considerations](/docs/vpc?topic=vpc-vpe-dns-sharing-planning-considerations&interface=ui).

## Creating a local-access endpoint gateway in the console
{: #local-access-vpe-creating-ui}
{: ui}

To create a local-access endpoint gateway in the {{site.data.keyword.cloud_notm}} console, follow these steps:

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

1. In the **Request connection to a service** section, select {{site.data.keyword.cloud_notm}}, then select **cloud-object-storage** from the menu. 

   * Select the region for your service. A region is pre-selected to optimize performance.

   * Keep in mind that all service endpoint types except for configuration service endpoints (**config** endpoint type) support resource bindings, which allow VPE bindings to Object Storage buckets or resources.
      
1. In the Share DNS section, select one of the following DNS resolution binding modes:

   * **Primary** allows the VPE gateway's service endpoint to be shared with the hub VPC's DNS resolver for name resolution. 
   * **Per-resource binding** allows the VPE gateway to have resource bindings to **cloud-object-storage** buckets and share the service resource endpoint with hub VPC's DNS resolver for name resolution.
   * **Disabled** does not allow the VPE gateway's service endpoints to be shared for name resolution.

   When the VPC is a DNS hub, DNS sharing is always set to **Primary** and the mode can't be changed.
   {: note}
      
   If you select **Per-resource binding** or **Disabled**, you can create optional resource bindings, which allow you to enable granular access from the selected VPC to specific Object Storage buckets. For more information, see [Creating resource bindings](/docs/vpc?topic=vpc-vpe-create-resource-binding&interface=ui). 

   During provisioning, you can bind a maximum of 10 buckets, with the option to add more later.
   {: note}

1. In the Reserved IP section, select reserved IP addresses. You can choose:

   * **Select one for me** - Enter a name prefix for the IP addresses, or one is chosen for you. Select one or more subnets (one subnet per zone is recommended) and enable the Auto-release toggle if you want this reserved IP to be deleted automatically if the endpoint gateway is deleted. 

   * **Select from existing IPs** - Choose from the list of existing IP addresses. If no IPs exist, click **Reserve IP** to reserve one.

      Only one reserved IP can be bound to a VPE gateway per AZ in a VPC. Ensure that no other bindings exist in the same AZ across all subnets within the VPC.
      {: note}
      
   * **Select one later** - Select IP addresses at some point after creating the endpoint gateway.

1. Review the **Order summary**, then click **Create virtual private endpoint gateway**. The endpoint gateway is requested for use.

## Creating a local-access endpoint gateway from the CLI
{: #local-access-vpe-ordering-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a local-access endpoint gateway from the CLI, follow these steps:

1. List the IBM Cloud service instances that are qualified to be set as endpoint gateway targets:

   ```sh
   ibmcloud is endpoint-gateway-targets
   ```
   {: pre} 

1. Create an endpoint gateway by running the following command:

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

## Creating a local-access endpoint gateway with the API
{: #local-access-vpe-ordering-api}
{: api}

To create a local-access endpoint gateway with the API, follow these steps:

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

1. When all variables are initiated, choose one of the following options to do:

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

## Creating a local-access endpoint gateway with Terraform
{: #create-local-access-vpe-terraform}
{: terraform}
 
You can use Terraform to create an endpoint gateway.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

The following example creates a VPE gateway:

```terraform
resource "ibm_is_virtual_endpoint_gateway" "vpe_iam" {
  name = "${var.name}-iam-vpe"
  vpc  = ibm_is_vpc.testacc_vpc.id

  target {
    resource_type = var.vpe_target_type
    crn           = var.vpe_target_crn
  }
  dns_resolution_binding_mode = "disabled"
}
resource "ibm_is_virtual_endpoint_gateway_resource_binding" "vpe_rb" {
  name = "${var.name}-rb-vpe"
  endpoint_gateway_id  = ibm_is_virtual_endpoint_gateway.vpe_iam.id

  target {
    crn           = var.vpe_rb_target
  }
}
data "ibm_is_virtual_endpoint_gateway" "vpe_iam" {
  name = ibm_is_virtual_endpoint_gateway.vpe_iam.name
}
data "ibm_is_virtual_endpoint_gateways" "vpe_iam" {
}
data ibm_is_virtual_endpoint_gateway_resource_bindings bindings {
  endpoint_gateway_id = ibm_is_virtual_endpoint_gateway.vpe_iam.id
}
data ibm_is_virtual_endpoint_gateway_resource_binding binding {
  endpoint_gateway_id = ibm_is_virtual_endpoint_gateway.vpe_iam.id
  endpoint_gateway_resource_binding_id = ibm_is_virtual_endpoint_gateway_resource_binding.vpe_rb.endpoint_gateway_resource_binding_id
}
```
{: codeblock} 

## After creating a local-access endpoint gateway
{: #after-create-vpe-gateway}

If you return to the Virtual private endpoint gateways for VPC page, your endpoint gateway shows in the table. For IBM Cloud services, the status of your endpoint gateway changes from `Updating` to `Stable`. You can click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") to rename, reserve or bind an IP, unbind an IP, or delete an endpoint gateway.

## Related links
{: #vpe-create-local-access-vpe-related-links} 

* [Creating resource bindings](/docs/vpc?topic=vpc-vpe-create-resource-binding&interface=ui)
* [DNS sharing planning considerations](/docs/vpc?topic=vpc-vpe-dns-sharing-planning-considerations) 
