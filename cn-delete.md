---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a cluster network
{: #delete-cluster-network}

You can delete a cluster network interface after all attached instances are deleted.
{: shortdesc}

## Before you begin
{: #delete-cluster-network-prerequisites}

Make sure that a cluster network exists. For more information, see [Creating a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=ui).

You can delete a cluster network with the console, CLI, API, or Terraform.

## Deleting a cluster network in the console
{: #delete-cluster-network-ui}
{: ui}

To delete a cluster network in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../../icons/icon_hamburger.svg), then click  **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Cluster networks**.
1. In the Cluster networks for VPC table, locate the cluster network interface that you want to delete, then click **Delete** in the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions").

Alternatively, you can **Delete** a cluster network from the Actions menu on the cluster network's details page.

## Deleting a cluster network from the CLI
{: #delete-cluster-network-cli}
{: cli}

To delete a cluster network from the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account with the CLI. After you enter the password, the system prompts for the account and region:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. To delete one or more cluster networks, enter the following command:



   ```sh
   ibmcloud is cluster-network-delete (CLUSTER_NETWORK1 CLUSTER_NETWORK2...) [--output JSON]  [-f, --force] [-q, --quiet]
   ```
   {: codeblock}




   Where:

   `CLUSTER_NETWORK1 CLUSTER_NETWORK2`
   :    IDs or names of the cluster networks, which are separated by a space.

   `-output`
   :    Specify the output format. Only JSON is supported. One of: `JSON`.

   `-force, -f`
   :    Force the operation without confirmation.

   `-q, --quiet`
   :    Suppress verbose output.



### Command example
{: #command-examples-cluster-network-delete}

To delete the cluster network `cli-cn-1`. You are prompted for confirmation. This action can't be undone.


```sh
ibmcloud is cluster-network-delete cli-cn-1
```
{: codeblock}




## Deleting a cluster network with the API
{: #delete-cluster-network-api}
{: api}

To delete a cluster network with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands. For example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, run the following command delete the cluster network:

   ```sh
   curl -X DELETE   "$vpc_api_endpoint/v1/cluster_networks/$cluster_network_id?version=$api_version&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: codeblock}

To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc/latest#list-cluster-network-profiles).

## Deleting a cluster network with Terraform
{: #delete-cluster-network-terraform}
{: terraform}

Use the `terraform destroy` command to delete a cluster network. The following example deletes `ibm_is_cluster_network`.

```terraform
terraform destroy --target ibm_is_cluster_network
```
{: codeblock}

You can delete all resources that were created by using a .tf file. For example, if you created a cluster with the following .tf file (`terraform apply -auto-approve`), the command `terraform destroy auto-approve` deletes all resources that are defined in your .tf file.

```terraform
// Provision is_cluster_network resource instance
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
