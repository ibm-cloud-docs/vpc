---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing details of a cluster network
{: #view-details-cluster-network}

View details of your cluster network, such as its name, creation date, and CRN. You can also manage and create subnets and interfaces within the cluster network. For example, you can specify the subnet CIDR and define reserved IP addresses.
{: shortdesc}

## Before you begin
{: #view-details-cluster-network-prerequisites}

* Review [planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [known issues and limitations](/docs/vpc?topic=vpc-known-issues-cluster-networks&interface=ui).
* Make sure that a cluster network exists. For more information, see [Creating a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=ui).

You can view details of a cluster network with the console, CLI, API, or Terraform.

## Viewing details of a cluster network in the console
{: #view-details-cluster-network-ui}
{: ui}

To view details of a cluster network interface in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Cluster networks**.
1. In the Cluster networks for VPC table, locate the cluster network for which you want to view details, then click its name.
1. View the details of your cluster network in the **Cluster network details** section.

## Viewing details of a cluster network from the CLI
{: #view-details-cluster-network-cli}
{: cli}

To view the details of a cluster network from the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account from the CLI. After you enter the password, the system prompts for the account and region:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. To view details of a cluster network, enter the following command:



   ```sh
   ibmcloud is cluster-network CLUSTER_NETWORK [--output JSON] [-q, --quiet]
   ```
   {: codeblock}





   Where:

   `CLUSTER_NETWORK`
   :   ID or name of the cluster network.

   `-output`
   :    Specify output format, only JSON is supported. One of: `JSON`.

   `-q, --quiet`
   :    Suppress verbose output.





### Command examples
{: #command-examples-cluster-network-view}

* To view the cluster network `my-cl-net-2` by name:



   ```sh
   ibmcloud is cl-net my-cl-net-2
   ```
   {: pre}




* To view the cluster network `my-cl-net-2` by ID:



   ```sh
   ibmcloud is cl-net 7208-353ec740-c1b1-4778-b7a1-8c77a365e435
   ```
   {: pre}




## Viewing details of a cluster network with the API
{: #view-details-cluster-network-api}
{: api}

To view details of a cluster network with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands. For example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, run the following command to view details of the cluster network:

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/cluster_networks/$cluster_network_id?version=$api_version&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: codeblock}

To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc/latest#list-cluster-network-profiles).
