---

copyright:
  years: 2019
lastupdated: "2019-07-29"


keywords: vpc, delete, resources, cli, infrastructure, command line interface

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Deleting a VPC using the IBM Cloud CLI
{: #deleting-using-cli}

This topic provides examples of how to delete resources from your {{site.data.keyword.cloud}} virtual private cloud, for every VPC resource, in the suggested order.
{:shortdesc}

Here are some key facts to remember about deletion:

* A VPC cannot be deleted until it is empty. 
* All containing resources must be deleted successfully before the parent resource can be deleted. 
* If the resource is pending a deletion, it shows a `deleting` status when viewed in list queries. 
* Most delete requests are _asynchronous_, which means that the resource may show a **deleting** status in list queries when it has not yet been deleted completely. 
* You must poll to make sure the child resource is no longer in the list view before attempting to delete the parent resource. 

If the status of the resource changes from `deleting` to `failed`, you can try to delete the resource again. If you cannot delete a resource in `failed` status, [Contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).
{: tip}

## Step 1: Find all subnets within the VPC you want to delete
{: #deleting-find-subnets-cli}

Before a VPC can be deleted, each of its subnets must be deleted. Get the ID of the VPC you want to delete by running the following command and looking at the `ID` value:

```
ibmcloud is vpcs
```
{: pre}

Save the ID of the VPC in a variable so we can use it later, for example:

```
vpc="0738-3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

To get the list of all subnets in your account, run the following command:

```
ibmcloud is subnets
```
{: pre}

Look at the `VPC` value to determine the subnets that need to be deleted. Save the ID of the subnet in a variable so we can use it later, for example:

```
subnet="0738-d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

If the VPC you want to delete has multiple subnets, repeat these steps to delete each subnet.
{: important}

## Step 2: Delete each subnet in the VPC
{: #deleting-subnet-resources-cli}


### Delete all network interfaces in the subnet, if any
{: #deleting-nics-cli}

An instance can have multiple network interfaces, and those network interfaces can belong to multiple subnets of the VPC. Before a subnet can be deleted, any network interface in the subnet must first be deleted. 

The only way to delete a network interface in a subnet is to delete the instance.
{: note}

To list all instances in your account, run the following command:

```
ibmcloud is instances
```
{: pre}

If you are deleting all instances in the VPC, you can run the following command for each instance in the VPC, where `$vsi` is the ID of the instance you want to delete. When the instance is deleted, all network interfaces in other subnets, if any, will be deleted automatically.

```
ibmcloud is instance-delete $vsi
```
{: pre}

The status of the instance should change to `deleting` immediately, but it will still appear in a list query result. The deletion of an instance can take up to 30 minutes.
{: note}

You can request other subnet resources to be deleted in parallel while waiting for the instance to be deleted. However, the subnet cannot be deleted until the instance and all other resources in the subnet no longer appear in the list query results.

To list all network interfaces of an instance, run the following command:

```
ibmcloud is instance-network-interfaces $vsi
```
{: pre}

If a secondary network interface exists in the subnet you are trying to delete, you must delete the instance. A network interface cannot be deleted from the instance without deleting the instance.

### Delete the subnet
{: #deleting-subnet-cli}

Once all the resources inside the subnet have been deleted, which means that they are not returned in a list query result, run the following command to delete the subnet:

```
ibmcloud is subnet-delete $subnet
```
{: pre}

The status of the subnet should change to `deleting` immediately, but it may take a few minutes until the subnet is actually deleted and no longer appears in the list query results.

## Step 3: Delete all public gateways in the VPC, if any
{: #deleting-pgs-cli}

To list all public gateways in your account, run the following command:

```
ibmcloud is public-gateways
```
{: pre}

For each public gateway in the VPC you want to delete, run the following command to delete the public gateway, where `$gateway` is the public gateway ID.

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


## Step 4: Delete the VPC
{: #deleting-single-vpc-cli}

Once all subnets and public gateways in the VPC have been deleted, run the following command to delete the VPC, where `$vpc` is the ID of the VPC you are deleting.

```
ibmcloud is vpc-delete $vpc
```
{: pre}

The status of the VPC should change to `deleting` immediately, but it may take a few minutes until the VPC is actually deleted and no longer appears in the list query results.
