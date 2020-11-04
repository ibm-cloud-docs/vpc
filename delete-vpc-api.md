---

copyright:
  years: 2019, 2020
lastupdated: "2020-08-25"


keywords: delete, resources, api, 

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Deleting a VPC by using the REST APIs
{: #deleting-using-api}

Deleting an {{site.data.keyword.vpc_full}} by using the REST APIs follows the same general steps in the
[deleting](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) process as deletion by using the [CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli).

Here are the main steps in the process:

1. Find all subnets in the VPC you want to delete.
2. Delete each subnet in the VPC, which means:
    - Delete all VPN gateways in the subnet.
    - Delete all Load Balancers in the subnet.
    - Delete all network interfaces of the instances in the subnet.
    - Delete the subnet.
3. Delete all public gateways in the VPC.
4. Delete the VPC.

The following sections provide some example API calls that you can run to delete a VPC.

## Step 1: Find all subnets in the VPC you want to delete
{: #deleting-find-subnets-api}

Before a VPC can be deleted, each of its subnets must be deleted. Get the ID of the VPC you want to delete by running the following command and looking at the `id` value:

Add ` | json_pp ` or ` | jq ` after the curl command to get a readable JSON string.
{: tip}

```bash
curl -X GET "$vpc_api_endpoint/v1/vpcs?version=$version&generation=2" \
     -H "Authorization:$iam_token"
```
{: pre}

Save the ID of the VPC in a variable so you can use it later, for example:

```
vpc="0738-3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

To get the list of all subnets in your account, run the following command:

```bash
curl -X GET "$vpc_api_endpoint/v1/subnets?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

Look at the `vpc` value to determine the subnets that need to be deleted. Save the ID of the subnet in a variable for later use, for example:

```
subnet="0738-d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

If the VPC you want to delete has multiple subnets, repeat the steps to delete each subnet.
{: important}

## Step 2: Delete each subnet in the VPC
{: #deleting-subnet-resources-api}

### Delete all VPN gateways in the subnet, if any
{: #deleting-vpn-api}

To list all VPN gateways in your account, run the following command:

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=2" \
     -H "Authorization:$iam_token"
```
{: pre}

Run the following command for each VPN gateway in the subnet you want to delete, where `$vpnid` is the ID of the VPN gateway.

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

The status of the VPN gateway changes to `deleting` immediately, but it is still returned when you do a list query. The deletion of a VPN gateway can take up to 30 minutes. You can request other subnet resources to be deleted in parallel while you wait for the VPN gateway to be deleted, but the subnet can't be deleted until the VPN gateway and all other resources in the subnet no longer appear in the list queries.

### Delete all load balancers in the subnet, if any
{: #deleting-lbs-api}

To list all load balancers in your account, run the following command:

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

Run the following command for each load balancer in the subnet you want to delete, where `$lbid` is the ID of the load balancer.

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

The status of the load balancer changes to `deleting` immediately, but it is still returned when you do a list query. The deletion of a load balancer can take up to 30 minutes. You can request other subnet resources to be deleted in parallel while you wait for the load balancer to be deleted, but the subnet can't be deleted until the load balancer and all other resources in the subnet no longer appear in the list queries.

### Delete all network interfaces in the subnet, if any
{: #deleting-nics-api}

An instance can have multiple network interfaces, and those network interfaces can belong to multiple subnets of the VPC. Before a subnet can be deleted, any network interface in the subnet must be deleted first. The only way to delete a network interface in a subnet is to delete the instance.

To list all network interfaces of an instance, run the following command:

```bash
curl -X GET "$vpc_api_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

If a secondary network interface exists in the subnet you are trying to delete, you need to delete the instance. A network interface cannot be deleted from the instance without deleting the instance.

To list all instances in your account, run the following command:

```bash
curl -X GET "$vpc_api_endpoint/v1/instances?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

If you are deleting all instances in the VPC, you can run the following command for each instance in the VPC, where `$vsi` is the ID of the instance you want to delete. When the instance is deleted, all network interfaces in other subnets, if any, are deleted automatically.

Instances must be stopped before you can delete them. For more information, see [Create an instance action](/apidocs/vpc#create-instance-action) to use the `type` request and `stop` action.
{: tip}

```bash
curl -X DELETE "$vpc_api_endpoint/v1/instances/$vsi?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

The status of the instance changes to `deleting` immediately, but it still might be displayed in the results of a list query. The deletion of an instance can take up to 30 minutes. You can request other subnet resources to be deleted in parallel while you wait for the instance to be deleted, but the subnet can't be deleted until the instance and all other resources in the subnet no longer appear in the list queries.

### Delete the subnet
{: #deleting-subnet-api}

After all the resources inside the subnet are deleted and are not returned when you run a list query, run the following command to delete the subnet:

```bash
curl -X DELETE "$vpc_api_endpoint/v1/subnets/$subnet?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

The status of the subnet changes to `deleting` immediately, but it might take a few minutes for the subnet to be deleted and it no longer returns in the list queries.

## Step 3: Delete all public gateways in the VPC, if any
{: #deleting-pgs-api}

To list all public gateways in your account, run the following command:

```bash
curl -X GET "$vpc_api_endpoint/v1/public_gateways?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

For each public gateway in the VPC you want to delete, run the following command to delete the public gateway, where `$gateway` is the public gateway ID.

```bash
curl -X DELETE "$vpc_api_endpoint/v1/public_gateways/$gateway?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}


## Step 4: Delete the VPC
{: #deleting-single-vpc-api}

After all subnets and public gateways in the VPC are deleted, run the following command to delete the VPC, where `$vpc` is the ID of the VPC you are deleting.

```bash
curl -X DELETE "$vpc_api_endpoint/v1/vpcs/$vpc?version=$version&generation=2" \     
     -H "Authorization:$iam_token"
```
{: pre}

The status of the VPC changes to `deleting` immediately, but it might take a few minutes for the VPC to be deleted and it no longer appears in the list queries.
