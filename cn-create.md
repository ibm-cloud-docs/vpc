---

copyright:
  years: 2024, 2025
lastupdated: "2025-04-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a cluster network
{: #create-cluster-network}

Cluster networks allow you to interconnect and define sets of performance criteria for multiple virtual networks. These networks are designed to support tasks that require high-speed data transfer and low latency, such as high-performance computing (HPC) and large-scale data processing.
{: shortdesc}

## Before you begin
{: #create-interface-prerequisites}

* Review [Planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [Known issues and limitations](/docs/vpc?topic=vpc-limitations-cluster-network&interface=ui).
* Ensure that you have an existing VPC in a region that has capacity for NVIDIA H100 or H200 profiles with clustering support.

   Currently, the only supported zone is `us-east-wdc07-a`. For more information about zones, see [zone mapping](/docs/overview?topic=overview-locations#zone-mapping).
   {: note}
   
You can create a cluster network with the UI, API, CLI, and Terraform.

## Creating a cluster network in the UI
{: #cn-ui-create}
{: ui}

To create a cluster network in the UI, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click  **Infrastructure** ![VPC icon](../../icons/vpc.svg)  > **Cluster networks**.
1. On the Cluster networks for VPC page, click **Create +**.
1. In the Location section, edit the following fields, if necessary.
    * **Geography**: Indicates the geography where you want the cluster network created.
    * **Region**: Indicates the region where you want the cluster network created.
    * **Zone**: Indicates the region where you want the cluster network created.
1. In the Details section, complete the following information:
   * **Name**: Enter a unique name for the virtual network interface, such as `my-virtual-network-interface`.
   * **Resource group**: Select a resource group for the virtual network interface.
   * **Tags**: (optional) Add tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Virtual private cloud**: Choose the VPC to associate the cluster network with.
1. Choose the profile for your cluster network. 

   Currently, cluster networking supports both H100 and Hopper-1 profiles for NVIDIA Hopper HGX instances. However, the H100 cluster network profile will be deprecated and replaced by the Hopper-1 cluster network profile, as it supports both NVIDIA H100 and H200 instance profiles.
   {: note} 

1. If you want to add cluster subnets to your cluster network, ensure that the Cluster network subnets section is switched On. Then:
   * Select the total number of cluster subnets you want to create (8, 16, or 32).
   * Add a subnet prefix to your cluster subnets.
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

   ```bash
   ibmcloud is cluster-network-create --vpc VPC --zone ZONE --profile PROFILE [--name NAME] [--subnet-prefixes-cidr SUBNET_PREFIXES_CIDR] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
   ```
   {: pre}

   Where:

   `-vpc`
    :    The ID or name of the VPC.

   `--zone`
   :    The zone this cluster network will reside in. The zone must be listed as supported on the specified cluster network profile.

   `--profile`
   :    Name of the profile to use for this cluster network. Currently, cluster networking supports both H100 and Hopper-1 profiles for NVIDIA Hopper HGX instances. However, the H100 cluster network profile will be deprecated and replaced by the Hopper-1 cluster network profile, as it supports both NVIDIA H100 and H200 instance profiles. 

   `--name`
   :    Name for the cluster network.

   `--subnet-prefixes-cidr`
   :    The IPv4 range of the cluster network's subnet prefix, expressed in CIDR format.

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

* `ibmcloud is cluster-network-create --vpc my-vpc --zone us-south-1 --profile profile-name`
* `ibmcloud is cluster-network-create --vpc my-vpc --zone us-south-1 --profile profile-name --name my-cl-net`
* `ibmcloud is cluster-network-create --vpc my-vpc --zone us-south-1 --profile profile-name --name my-cl-net --subnet-prefixes-cidr 10.0.0.24/24`
* `ibmcloud is cluster-network-create --vpc my-vpc --zone us-south-1 --profile profile-name`
* `ibmcloud is cluster-network-create --vpc my-vpc --zone us-south-1 --profile profile-name --name my-cl-net`
* `ibmcloud is cluster-network-create --vpc my-vpc --zone us-south-1 --profile profile-name --name my-cl-net --subnet-prefixes-cidr 10.0.0.24/24 --output JSON`
 
## Creating a cluster network with the API
{: #cn-api-create}
{: api}

To create a cluster network with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, run the following command to create the cluster network:

   ```sh
   curl -X POST   "$vpc_api_endpoint/v1/cluster_networks?version=$tomorrow&generation=2&maturity=development" -H "Authorization: Bearer $iam_token" -d '{
      "name": "my-cluster-network",
      "profile": {
          "name": "h100"
      },
      "vpc": {
          "id": "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
      },
      "zone": {
          "name": "us-south-1"
      }
    }'
   ```
   {: codeblock}

   To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc/latest#list-cluster-network-profiles).

## Creating a cluster network interface with Terraform
{: #ccreate-cluster-network-terraform}
{: terraform}

The following example provisions a cluster network instance by using Terraform:

```terraform
resource "ibm_is_cluster_network" "is_cluster_network_instance" {
  name            = var.is_cluster_network_name  // change to update
  profile         = "h100"
  resource_group  = "fee82deba12e4c0fb69c3b09d1f12345"
  subnet_prefixes {
    cidr = "10.0.0.0/24"
  }
  vpc             = "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
  zone            = "us-south-1"
}
```
{: codeblock}
