---

copyright:
  years: 2024
lastupdated: "2024-10-15"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing details of a cluster network
{: #view-details-cluster-network}

Contact your IBM Support representative if you are interested in getting early access to this offering. It is currently available for early access users of the `gx3d-160x1792x8h100` [virtual server instance profiles](/docs/vpc?topic=vpc-profiles#gpu) in the `us-east` region.
{: beta}

View details of your cluster network, such as its name, creation date, and CRN. You can also manage and create subnets and interfaces within the cluster network. For example, you can specify the subnet CIDR and define reserved IP addresses.
{: shortdesc}

## Before you begin
{: #view-details-cluster-network-prerequisites}

Review [Planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [Known issues and limitations](/docs/vpc?topic=vpc-limitations-cluster-network&interface=ui).

You can view details of a cluster network with the UI, CLI, API, or Terraform.

## Viewing details of a cluster network in the UI
{: #view-details-cluster-network-ui}
{: ui}

To view details of a cluster network interface in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg)  > **Cluster networks**.
1. In the Cluster networks for VPC table, locate the cluster network for which you want to view details, then click the name of that cluster network.
1. View the details of your cluster network in the **Cluster network details** section.

## Viewing details of a cluster network in the CLI
{: #view-details-cluster-network-cli}
{: cli}

To view the details of a cluster network in the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your account with the CLI. After you enter the password, the system prompts for the account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Enable the following feature flag:

   ```sh
   export IBMCLOUD_IS_FEATURE_CLUSTER_NETWORK=true
   ```
   {: pre}

1. To view details of a cluster network, enter the following command:

   ```bash
   ibmcloud is cluster-network CLUSTER_NETWORK [--output JSON] [-q, --quiet]
   ```
   {: pre}

   Where:

   `CLUSTER_NETWORK`
   :   ID or name of the cluster network.

   `-output`
   :    Specify output format, only JSON is supported. One of: `JSON`.

   `-q, --quiet`
   :    Suppress verbose output.

### Command examples
{: #command-examples-cluster-network-view}

To view cluster network `my-cl-net-2` by name:

```sh
ibmcloud is cl-net my-cl-net-2
```
{: codeblock}

To view cluster network `my-cl-net-2` by ID:

```sh
ibmcloud is cl-net 7208-353ec740-c1b1-4778-b7a1-8c77a365e435
```
{: codeblock}

### Related commands
{: #related-commands-cluster-network-interface-details}

* [Creating a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=cli)
* [Create a cluster network subnet](/docs/vpc?topic=vpc-create-cluster-network-subnet&interface=cli)
* [Creating a cluster network interface](/docs/vpc?topic=vpc-create-cluster-network-interface&interface=cli)
* [Delete a cluster network](/docs/vpc?topic=vpc-delete-cluster-network&interface=cli)
* [Attaching cluster network interfaces to an instance](/docs/vpc?topic=vpc-attach-interfaces-cluster-network&interface=cli)

## Viewing details of a cluster network with the API
{: #view-details-cluster-network-api}
{: api}

To view details of a cluster network with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, run the following command to view details of the cluster network:

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/cluster_networks/$cluster_network_id?version=$tomorrow&generation=2&maturity=development" -H "Authorization: Bearer $iam_token"
   ```
   {: codeblock}

To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc-scoped?code=go#list-cluster-network-profiles).

## Viewing details of a cluster network with Terraform
{: #view-details-cluster-network-terraform}
{: terraform}

Terraform will support this feature after it reaches General Availability (GA) and is officially released.
{: note}
