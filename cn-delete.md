---

copyright:
  years: 2024
lastupdated: "2024-11-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a cluster network
{: #delete-cluster-network}

Cluster Networks for VPC is available for select customers only. Contact IBM Support if you are interested in using this functionality.
{: preview}

You can delete a cluster network interface after all instances attached to it are deleted.
{: shortdesc}

## Before you begin
{: #delete-cluster-network-prerequisites}

Review [Planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [Known issues and limitations](/docs/vpc?topic=vpc-limitations-cluster-network&interface=ui).

You can delete a cluster network with the UI, CLI, API, or Terraform.

## Deleting a cluster network in the UI
{: #delete-cluster-network-ui}
{: ui}

To delete a cluster network in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click  **Infrastructure** ![VPC icon](../../icons/vpc.svg)  > **Cluster networks**.
1. In the Cluster networks for VPC table, locate the cluster network interface that you want to delete, then click **Delete** in the Actions menu ![Actions menu](images/overflow.png).

   Alternatively, you can **Delete** a cluster network from the Actions menu on the cluster network's details page.

## Deleting a cluster network in the CLI
{: #delete-cluster-network-cli}
{: cli}

To delete a cluster network in the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Enable the following feature flag:

   ```sh
   export IBMCLOUD_IS_FEATURE_CLUSTER_NETWORK=true
   ```
   {: pre}
   
1. Log in to your account with the CLI. After you enter the password, the system prompts for the account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. To delete one or more cluster networks, enter the following command:

   ```bash
   ibmcloud is cluster-network-delete (CLUSTER_NETWORK1 CLUSTER_NETWORK2...) [--output JSON]  [-f, --force] [-q, --quiet]
   ```
   {: pre}

   Where:

   `CLUSTER_NETWORK1 CLUSTER_NETWORK2`
   :    IDs or names of the cluster networks, separated by a space.

   `-output`
   :    Specify output format, only JSON is supported. One of: `JSON`.

   `-force, -f`
   :    Force the operation without confirmation.

   `-q, --quiet`
   :    Suppress verbose output.

### Command example
{: #command-examples-cluster-network-delete}

To delete cluster network `cli-cn-1`. You are prompted for confirmation. This action cannot be undone.

```sh
ibmcloud is cluster-network-delete cli-cn-1
```
{: codeblock}

### Related commands
{: #related-commands-cluster-network-interface-delete}

* [Creating a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=cli)
* [Create a cluster network subnet](/docs/vpc?topic=vpc-create-cluster-network-subnet&interface=cli)
* [Creating a cluster network interface](/docs/vpc?topic=vpc-create-cluster-network-interface&interface=cli)
* [View details of a cluster network](/docs/vpc?topic=vpc-view-details-cluster-network&interface=cli)
* [Attaching cluster network interfaces to an instance](/docs/vpc?topic=vpc-attach-interfaces-cluster-network&interface=cli)

## Deleting a cluster network with the API
{: #delete-cluster-network-api}
{: api}

To delete a cluster network with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, run the following command delete the cluster network:

   ```sh
   curl -X DELETE   "$vpc_api_endpoint/v1/cluster_networks/$cluster_network_id?version=$tomorrow&generation=2&maturity=development" -H "Authorization: Bearer $iam_token"
   ```
   {: codeblock}

To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc-scoped?code=go#list-cluster-network-profiles).
