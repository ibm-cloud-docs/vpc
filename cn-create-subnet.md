---

copyright:
  years: 2024, 2025
lastupdated: "2025-05-26"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a cluster network subnet
{: #create-cluster-network-subnet}

A cluster network subnet is a subnet within a cluster network. While it resembles a VPC subnet, it offers fewer features. However, it does let you define the subnet CIDR and configure reserved IPs for the cluster network subnet. These reserved IPs include addresses you specify and come with an auto-delete function similar to that of VPC subnet reserved IPs.
{: shortdesc}

## Before you begin
{: #create-subnet-prerequisites}

Review [Planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [Known issues and limitations](/docs/vpc?topic=vpc-limitations-cluster-network&interface=ui).

You can create a cluster network subnet with the console, CLI, API, or Terraform.

## Creating a cluster network subnet in the console
{: #create-cluster-network-subnet-ui}
{: ui}

To create a subnet within a network cluster in the console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![menu icon](../../icons/icon_hamburger.svg), then click  **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Cluster networks**.
1. On the Cluster networks for VPC page, click the link of the cluster network name where you want to create a subnet. The Overview page displays.
1. Click the Subnets tab to show the Cluster network subnets table, then click **Create +**.
1. Complete the information in the Create subnet side panel, then click **Create**.

   * Enter a subnet name.
   * Review the address prefix. Change as needed.
   * Select the number of total IP addresses (default is `256`).
   * Review the IP range stated. Change as needed.
   * Review the specified address space. Adjust the IP range as needed.

The cluster network subnet is requested for use.

## Creating a cluster network subnet in the CLI
{: #create-cluster-network-subnet-cli}
{: cli}

To create a cluster network subnet in the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account with the CLI. After you enter the password, the system prompts for the account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. To create a cluster network subnet, enter the following command:

   ```bash
   ibmcloud is cluster-network-subnet-create CLUSTER_NETWORK (--total-ipv4-address-count TOTAL_IPV4_ADDRESS_COUNT | --ipv4-cidr-block IPV4_CIDR_BLOCK) [--name NAME] [--ip-version IP_VERSION] [--output JSON] [-q, --quiet]
   ```
   {: pre}

   Where:

   `CLUSTER_NETWORK`
   :    ID or name for the cluster network.

   `--total-ipv4-address-count`
   :    The total number of IPv4 addresses required. Must be a power of 2.

   `--ipv4-cidr-block`
   :    The IPv4 range of the cluster network subnet, expressed in CIDR format.

   `--name`
   :    The name for this cluster network subnet.

   `--output`
   :    Specify output format, only JSON is supported. One of: `JSON`.

   `-q`, `--quiet`
   :    Suppress verbose output.

### Command example
{: #command-example-create-cluster-network-subnets}

To create a cluster network subnet with name `cli-cn-sub-1` for cluster network `cli-cn-1`:

```sh
ibmcloud is cluster-network-subnet-create cli-cn-1 --name cli-cn-sub-1 --total-ipv4-address-count 32
```
{: codeblock}

## Creating a cluster network subnet with the API
{: #create-cluster-network-subnet-api}
{: api}

To create a cluster network subnet with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, run the following command to create the cluster network subnet:

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/cluster_networks/$cluster_network_id/subnets?version=$today&generation=2" -H "Authorization: Bearer $iam_token" -d '{
         "name": "my-cluster-network-subnet",
         "total_ipv4_address_count": 2048
       }'
   ```
   {: codeblock}

To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc/latest#list-cluster-network-profiles).

## Creating a cluster network subnet with Terraform
{: #create-cluster-network-subnet-terraform}
{: terraform}

The following example provisions a cluster network subnet by using Terraform:

```terraform
resource "ibm_is_cluster_network_subnet" "is_cluster_network_subnet_instance" {
  cluster_network_id        = var.is_cluster_network_subnet_cluster_network_id
  ip_version                = var.is_cluster_network_subnet_ip_version
  name                      = var.is_cluster_network_subnet_name
  total_ipv4_address_count  = var.is_cluster_network_subnet_total_ipv4_address_count
  // ipv4_cidr_block = var.is_cluster_network_subnet_ipv4_cidr_block #conflicts with total_ipv4_address_count
}
```
{: codeblock}
