---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a cluster network interface
{: #create-cluster-network-interface}

A cluster network interface is a virtual NIC that connects your instance to a cluster network subnet. You can create a cluster network interface either during the creation of a cluster network, or after the network is provisioned.
{: shortdesc}

## Before you begin
{: #create-cluster-prerequisites}

* If you don't have a cluster network, review [planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [known issues](/docs/vpc?topic=vpc-known-issues-cluster-networks).
* See [creating a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=ui).

You can create a cluster network interface with the console, CLI, API, or Terraform.

## Creating a cluster network interface in the console
{: #create-cluster-network-interface-ui}
{: ui}

To create a cluster network interface in the console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![menu icon](../../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Cluster networks**.
1. On the Cluster networks for VPC page, click the link of the cluster network name where you want to create an interface. The Overview page is displayed.
1. Click the Interfaces tab to show the Cluster network interfaces table, then click **Create**.
1. In the Create interface side panel, complete the following information:

   * Enter an interface name.
   * Select the cluster network subnet.

      A primary IP address is selected for you and is available to view after creation.
      {: note}

1. Click **Create**. The cluster network interface is requested for use.

## Creating a cluster network interface from the CLI
{: #create-cluster-network-interface-cli}
{: cli}

To create a cluster network interface from the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account with the CLI. After you enter the password, the system prompts for the account and region:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. To create a cluster network interface, enter the following command:

   
   ```sh
   ibmcloud is cluster-network-interface-create CLUSTER_NETWORK (--rip RIP | (--rip-address RIP_ADDRESS --rip-auto-delete true | false --rip-name RIP_NAME)) [--subnet SUBNET] [--vpc VPC] [--name NAME] [--output JSON] [-q, --quiet]
   ```
   {: codeblock}

   

   

   

   Where:

   `CLUSTER_NETWORK`
   :    ID or name for the cluster network.

   `--rip`
   :    ID or name of the reserved IP to bind the primary IP address to the cluster network interface.

   `--rip-address`
   :    The IP address of the reserved IP to bind to the cluster network interface, which must not already be reserved on the subnet.

   `--rip-auto-delete`
   :    Indicates whether this cluster network subnet reserved IP member is automatically deleted when either target is deleted, or the cluster network subnet reserved IP is unbound. One of: `true`, `false` (default: `true`).

   `--rip-name`
   :    The name of this cluster network subnet reserved IP.

   `--subnet`
   :    ID or name for the subnet.

   `--vpc`
   :    ID or name of the VPC. It is only required to specify the unique resource by name inside this VPC.

   `--name`
   :    Name of the cluster network interface.

   `--output`
   :    Specify output format, only JSON is supported. One of: `JSON`.

   `-q`, `--quiet`
   :    Suppress verbose output.
   

   

### Command examples
{: #command-examples-create-cluster-network-interface}

* To create a cluster network interface with reserved IP:
   
   ```sh
   ibmcloud is cluster-network-interface-create my-cluster-network --name my-cluster-network-interface --subnet my-cluster-network-subnet --rip-name my-cluster-network-interface-reserved-ip
   ```
   {: pre}

   

   


* To create a cluster network interface by using a reserved IP from an existing cluster network subnet:
   
   ```sh
   ibmcloud is cluster-network-interface-create my-cluster-network --name my-cluster-network-interface --rip my-cluster-network-interface-reserved-ip
   ```
   {: pre}

   
   

## Creating a cluster network interface with the API
{: #create-cluster-network-interface-api}
{: api}

To create a cluster network interface with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands. For example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, run the following command to create a cluster network interface:

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/cluster_networks/$cluster_network_id/interfaces?version=$today&generation=2" -H "Authorization: Bearer $iam_token" -d '{
         "name": "my-cluster-network-interface",
         "subnet": {
             "id": "0767-7931845c-65c4-4b0a-80cd-7d9c1a6d7930"
         }
       }'
   ```
   {: codeblock}

To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc/latest#list-cluster-network-profiles).

## Creating a cluster network interface with Terraform
{: #create-cluster-network-interface-terraform}
{: terraform}

The following example provisions a cluster network interface by using Terraform:

```terraform
resource "ibm_is_cluster_network_interface" "is_cluster_network_interface_instance" {
  cluster_network_id            = var.is_cluster_network_interface_cluster_network_id
  name                          = var.is_cluster_network_interface_name
  primary_ip {
    address = "10.1.0.6"
    name    = "my-cluster-network-subnet-reserved-ip"
  }
  subnet = "0767-7931845c-65c4-4b0a-80cd-7d9c1a6d7930"
}
```
{: codeblock}

## Next steps
{: #next-steps-create-interface}

To create virtual server instances and attach them to interfaces in your cluster network, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers) and [Attaching a cluster network interface to an instance](/docs/vpc?topic=vpc-attach-interfaces-cluster-network).
