---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-21"

keywords: resource attribute, iam access policy, terraform, cli

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:external: target="_blank" .external}


# VPC resource attributes
{: #resource-attributes}

When using Terraform or {{site.data.keyword.cloud}} Command Line Interface (CLI) to create, update, or delete {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) access policies, you can specify the target VPC resource by using resource attributes.
{:shortdesc}

Resource attributes are in the form of  `name=value,name=value...`.

You can select a resource object by entering the id of the object. Or, you can enter the wildcard `*` in `value` to denote all applicable objects. For example, the attribute `vpcId:*` set the access policy to be applicable to all the VPCs in the account. You can also specify which resource group the policy is applied to in the command.

The following example CLI command gives the user `name@example.com` `Viewer` role for all the VPCs in the current account:

```
ibmcloud iam user-policy-create name@example.com --roles Viewer --service-name is --attributes "vpcId=*"
```
{:pre} 

For information about using CLI to create and modify IAM access policy, see [ibmcloud iam user-policy-create](/docs/account?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_user_policy_create).

For information about using Terraform to create IAM access policy, see [Access Group Policy using attributes](/docs/terraform?topic=terraform-iam-resources#access-group-policy-using-attributes).

See Table 1 for the full list of VPC resource attributes.

|   Resource     | Resource Attribute |
| ------- | ------ |
| Auto Scale for VPC | `instanceGroupId:<instance-group-id>` | 
| Block Storage for VPC | `volumeId: <volume-id>` |  
| Floating IP for VPC | `floatingIpId: <fip-id>` |
| Flow Logs for VPC | `flowLogCollectorId: <flc-id>` |
| Image Service for VPC | `imageId:<image-id>` |
| Load Balancer for VPC | `loadBalancerId: <load-balancer-id>` |
| Network ACL | `networkAclId: <nacl-id>` |
| Placement Group for VPC | `placementGroupId: <placement-group-id>` |
| Public Gateway for VPC | `publicGatewayId: <pgw-id>` |
| Security Group for VPC | `securityGroupId: <default-sec-grp-id>` |
| SSH Key for VPC | `keyId:<key-id>` |
| Subnet | `subnetId: <subnet-id>` |
| Virtual Private Cloud |  `vpcId: <vpc-id>`  |   
| Virtual Server for VPC | `instanceId: <instance-id>` |   
| VPN for VPC | `vpnId:<vpn-id>` |
{: caption="Table 1. VPC resource attributes" caption-side="top"}

<!--| Dedicated Host for VPC | `dedicatedHostId:<dedicated-host-id>` | -->
<!--| Virtual Private Endpoint for VPC | `endpointGatewayId:<endpoint-gateway-id>` |--> 
<!--Exclude "Snapshot snapshotId" and "Share shareId" as they are neither in production nor staging. Don't push those labeled with "staging" to production-->

