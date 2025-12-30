---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a cluster network
{: #create-cluster-network}

Cluster networks help you interconnect and define sets of performance criteria for multiple virtual networks. These networks are designed to support tasks that require high-speed data transfer and low latency, such as high-performance computing (HPC) and large-scale data processing.
{: shortdesc}

## Before you begin
{: #create-interface-prerequisites}

Before you create a cluster network, review the following prerequisites:

* Review [planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [known issues](/docs/vpc?topic=vpc-known-issues-cluster-networks).
* Make sure that you have an existing VPC in a region that has capacity for cluster network profiles with clustering support. For more information, see [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started) and [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region&interface=cli).

You can create a cluster network with the console, API, CLI, and Terraform.

## Creating a cluster network in the console
{: #cn-ui-create}
{: ui}

To create a cluster network in the console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../../icons/icon_hamburger.svg), then click  **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Cluster networks**.
1. On the Cluster networks for VPC page, click **Create**.
1. In the Location section, edit the following fields, if necessary.
    * **Geography**: The geographic area for the cluster network.
    * **Region**: The specific region within the geography.
    * **Zone**: The availability zone within the selected region.

    You must create the cluster network and the virtual server instances in the same region as the VPC. For more information, see [supported regions and zones](/docs/vpc?topic=vpc-planning-cluster-network#cn-supported-regions) for cluster networks and instances.
    {: note}

1. In the Details section, complete the following information:
   * **Name**: Enter a unique name for the virtual network interface, such as `my-virtual-network-interface`.
   * **Resource group**: Select a resource group for the virtual network interface.
   * **Tags**: (Optional) Add tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Virtual private cloud**: Choose the VPC to associate the cluster network with.
1. Choose the required profile for your cluster network. Available profile is Hopper 1 (`hopper-1`).

1. To add cluster subnets to your cluster network, enable the **Cluster network subnets** section. You can add cluster subnets now, or after you create your cluster network.
   * For Hopper 1, select the total number of cluster subnets that you want to create (8, 16, or 32).
   * Add a subnet prefix to your cluster subnets.

   Certain scenarios might use a larger number of subnets.
   {: note}

1. Review the information in the Summary panel, and click **Create cluster network**.

## Creating a cluster network from the CLI
{: #create-cluster-network-cli}
{: cli}

To create a cluster network from the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account with the CLI. After you enter the password, the system prompts for the account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. To create a cluster network, enter the following command:



```sh
   ibmcloud is cluster-network-create --vpc VPC --zone ZONE --profile PROFILE [--name NAME] [--subnet-prefixes-cidr SUBNET_PREFIXES_CIDR] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
   ```
   {: codeblock}

   Where:

   `-vpc`
    :    The ID or name of the VPC.

   `--zone`
   :    The zone this cluster network resides in. The zone must be listed as supported on the specified cluster network profile. For more information, see [Cluster network supported regions and zones](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui#cn-supported-regions).

   `--profile`
   :    Name of the profile to use for this cluster network. Available profiles are `hopper-1` for Hopper 1 cluster network.

   `--name`
   :    Name for the cluster network.

   `--subnet-prefixes-cidr`
   :    The IPv4 range of the cluster network's subnet prefix expressed in CIDR format.

   `--resource-group-id value`
   :    ID of the resource group. This ID is mutually exclusive with `--resource-group-name`.

   `--resource-group-name value`
   :    Name of the resource group. This name is mutually exclusive with `--resource-group-id`.

   `--output value`
   :    Specify output format, only JSON is supported. One of: JSON.

   `-q, --quiet`
   :    Suppress verbose output.





### Command examples
{: #cli-command-examples-cluster-network-create}

* Create a new cluster network in the specified VPC and zone with a specific profile:


   ```sh
   ibmcloud is cluster-network-create --vpc my-vpc --zone us-south-1 --profile profile-name
   ```
 {: pre}




* Create a cluster network in the specified VPC and zone with a specific profile and custom name (`my-cl-net`):


   ```sh
   ibmcloud is cluster-network-create --vpc my-vpc --zone us-south-1 --profile profile-name --name my-cl-net
   ```
 {: pre}





* Create a cluster network with a custom name and a specified CIDR block for the subnet prefix. The output is formatted in JSON:


   ```sh
   ibmcloud is cluster-network-create --vpc my-vpc --zone us-south-1 --profile profile-name --name my-cl-net --subnet-prefixes-cidr 10.0.0.24/24 --output JSON
   ```
 {: pre}







## Creating a cluster network with the API
{: #cn-api-create}
{: api}

To create a cluster network with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands. For example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, run the following command to create the cluster network:

   ```sh
   curl -X POST   "$vpc_api_endpoint/v1/cluster_networks?version=$api_version&generation=2" -H "Authorization: Bearer $iam_token" -d '{
      "name": "my-cluster-network",
      "profile": {
          "name": "hopper-1"
      },
      "vpc": {
          "id": "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
      },
      "zone": {
          "name": "us-east-3"
      }
    }'
   ```
   {: codeblock}

   Available cluster network profiles name is `hopper-1`.
   {: note}

To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc/latest#list-cluster-network-profiles).

## Creating a cluster network interface with Terraform
{: #create-cluster-network-terraform}
{: terraform}

The following example provisions a cluster network instance by using Terraform:

```terraform
resource "ibm_is_cluster_network" "is_cluster_network_instance" {
  name            = var.is_cluster_network_name  // change to update
  profile         = "hopper-1"
  resource_group  = "fee82deba12e4c0fb69c3b09d1f12345"
  subnet_prefixes {
    cidr = "10.0.0.0/24"
  }
  vpc             = "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
  zone            = "us-east-3"
}
```
{: codeblock}

## Next steps
{: #next-steps-create-cluster-network}

If you didn't create cluster network subnets when you created your cluster network, see [Creating a cluster network subnet](/docs/vpc?topic=vpc-create-cluster-network-subnet).
