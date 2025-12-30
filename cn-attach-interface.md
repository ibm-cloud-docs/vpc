---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching a cluster network interface to an instance
{: #attach-interfaces-cluster-network}

After you create an instance, you can attach it to an existing cluster network instance subnet. Alternatively, you can plan out your network by creating the cluster network instances first, before you create subnet instances to attach.
{: shortdesc}

## Before you begin
{: #attach-interfaces-prerequisites}

* Review [planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [known issues](/docs/vpc?topic=vpc-known-issues-cluster-networks).
* Make sure that you complete the following steps:
   * Check whether a cluster network exists. For more information, see [creating a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=ui).
   * [Create a cluster network subnet](/docs/vpc?topic=vpc-create-cluster-network-subnet&interface=ui).
   * [Create a virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui).
   * [Create a cluster network interface](/docs/vpc?topic=vpc-create-cluster-network-interface&interface=ui).

You can attach cluster network interfaces to an instance with the CLI, API, or Terraform.

## Attaching a cluster network interface to an instance from the CLI
{: #attach-interfaces-cluster-network-cli}
{: cli}

To attach an interface to an instance from the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account from the CLI. After you enter the password, the system prompts for the account and region:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. To attach an interface to an instance, enter the following command:

   
   ```sh
   ibmcloud is instance-cluster-network-attachments INSTANCE --cnac CLUSTER_NETWORK_ATTACHMENT
   ```
   {: codeblock}

   
   

   

   
   Where:

   `INSTANCE`
   :    ID or name of the instance.

   `--output`
   :    Specify output format, only JSON is supported. One of: `JSON`.

   `-q`, `quiet`
   :    Suppress verbose output.
   

   

### Command examples
{: #command-examples-cluster-network-attach-interfaces}

* To create instance `my-ins-1` with cluster network atttachments:

   
   ```sh
   ibmcloud is instance-create my-ins-1 cli-vpc us-south-2 fx3d-160c1792x9h100 cli-subnet --image ibm-ubuntu-20-04-6-minimal-amd64-5 --cluster-network-attachments '[{"name":"instance-cnac-1","cluster_network_interface":{"id":"7208-18204195-be40-4f12-aaaa-2649e19acb91"}},{"name":"instance-cnac-2","cluster_network_interface":{"id":"7208-cf6023a3-86c7-459f-84b8-536b4f812541"}},{"name":"instance-cnac-3","cluster_network_interface":{"id":"7208-2d55d27c-835c-4566-acbc-da36bcf49da7"}},{"name":"instance-cnac-4","cluster_network_interface":{"id":"7208-161e2919-c505-4e6d-bd49-88e7e0c0f1f3"}},{"name":"instance-cnac-5","cluster_network_interface":{"id":"7208-d700a40e-af61-45ee-b71a-a09203db76bd"}},{"name":"instance-cnac-6","cluster_network_interface":{"id":"7208-c7dbc8b9-6b47-4b83-8649-512e4e8f0a81"}},{"name":"instance-cnac-7","cluster_network_interface":{"id":"7208-f4773e40-49b5-4d44-8c68-7a75513bbf16"}},{"name":"instance-cnac-8","cluster_network_interface":{"id":"7208-46e097b5-c1ea-4669-aafd-7a4cc82d0e02"}}]'
   ```
   {: pre}

   

   

* To create instance `my-ins-2` with a cluster network attachments JSON file:

   
   ```sh
   ibmcloud is instance-create my-ins-2 deceiving-strode-dimly-undesirable us-south-2 hx3d-160x1002x8h100 test-subnet --image ibm-ubuntu-20-04-6-minimal-amd64-5 --cluster-network-attachments @~/cnac.json
   ```
   {: pre}

   

   

## Attaching a cluster network interface to an instance with the API
{: #attach-interfaces-cluster-network-api}
{: api}

To attach a cluster network interface to an instance by using the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands. For example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, enter the following command to attach the cluster network interface to the instance:

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/instances/$instance_id/cluster_network_attachments?version=$today&generation=2" -H "Authorization: Bearer $iam_token" -d '{
     "name": "my-cluster-network-attachment",
     "cluster_network_interface": {
       "id": "$cluster_network_interface_id"
     }
   }'
   ```
   {: codeblock}

To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc/latest#list-cluster-network-profiles).

## Attaching a cluster network interface to an instance with Terraform
{: #attach-interfaces-cluster-network-terraform}
{: terraform}

The following example provisions an instance cluster network attachment by using Terraform:

```terraform
resource "ibm_is_instance_cluster_network_attachment" "is_instance_cluster_network_attachment_instance" {
  instance = var.is_instance_cluster_network_attachment_instance_id
  before {
    # href = "https://us-south.iaas.cloud.ibm.com/v1/instances/5dd61d72-acaa-47c2-a336-3d849660d010/cluster_network_attachments/0767-fb880975-db45-4459-8548-64e3995ac213" // conflicts with id
    id = "0767-fb880975-db45-4459-8548-64e3995ac213"
  }
  cluster_network_interface {
    # id              = var.is_cluster_network_interface_id // conflicts with other properties
    auto_delete     = var.is_cluster_auto_delete
    name            = var.is_cluster_name
    subnet          = var.is_cluster_subnet_id
    primary_ip {
      address = "192.168.3.4"
      name    = "my-cluster-subnet-reserved-ip-1"
    }
  }
  name = var.is_instance_cluster_network_attachment_name
}
```
{: codeblock}
