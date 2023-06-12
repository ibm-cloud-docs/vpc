---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-01"

keywords: resource attribute, iam access policy, terraform, cli

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC resource attributes
{: #resource-attributes}

When you use Terraform or the {{site.data.keyword.cloud}} Command Line Interface (CLI) to create, update, or delete {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) access policies, you can specify the target VPC resource by using resource attributes.
{: shortdesc}

Resource attributes are in the form of  `name=value,name=value...`.

You can select a resource object by entering the ID of the object. Or, you can enter the wildcard `*` in `value` to denote all applicable objects. For example, the attribute `vpcId:*` set the access policy to be applicable to all the VPCs in the account. You can also specify which resource group the policy is applied to in the command.

The following example CLI command gives the user `name@example.com` `Viewer` role for all the VPCs in the current account:

```sh
ibmcloud iam user-policy-create name@example.com --roles Viewer --service-name is --attributes "vpcId=*"
```
{: pre}

For more information about using the CLI to create and modify IAM access policy, see [ibmcloud iam user-policy-create](/docs/account?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_user_policy_create).

For more information about using Terraform to create IAM access policy, see the `resources.attributes` input parameter for the following IAM policies:

* [`ibm_iam-access_group_policy`](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-iam-resources#iam-access-group-policy)
* [`ibm_iam_service_policy`](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-iam-resources#iam-service-policy)
* [`ibm_iam_user_policy`](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-iam-resources#iam-user-policy)
* [`ibm_iam_user_invite`](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-iam-resources#iam-user-invite) (`iam_policy.resource.attributes`)

See Table 1 for the full list of VPC resource attributes.


|   Resource     | Resource Attribute |
| ------- | ------ |
| Auto Scale for VPC | `instanceGroupId:<instance-group-id>` |
| Backup service | `backupPolicyId: <backup-policy-id>`|
| Block Storage for VPC | `volumeId: <volume-id>` |
| Bare metal server | `bareMetalServerId: <bare-metal-server-id>` |
| Dedicated Host for VPC | `dedicatedHostId:<dedicated-host-id>` <!--(staging)--> |
| File Storage | `shareId: <share-id>` | 
| Floating IP for VPC | `floatingIpId: <fip-id>` |
| Flow Logs for VPC | `flowLogCollectorId: <flc-id>` |
| Image Service for VPC | `imageId:<image-id>` |
| Load Balancer for VPC | `loadBalancerId: <load-balancer-id>` |
| Network ACL | `networkAclId: <nacl-id>` |
| Placement Group for VPC | `placementGroupId: <placement-group-id>` |
| Public Gateway for VPC | `publicGatewayId: <pgw-id>` |
| Security Group for VPC | `securityGroupId: <default-sec-grp-id>` |
| Snapshots | `snapshotId: <snapshot-id>`|
| SSH Key for VPC | `keyId:<key-id>` |
| Subnet | `subnetId: <subnet-id>` |
| Virtual Private Endpoint for VPC | `endpointGatewayId:<endpoint-gateway-id>`<!--(staging)--> |
| Virtual Private Cloud |  `vpcId: <vpc-id>`  |   
| Virtual Server for VPC | `instanceId: <instance-id>` |   
| {{site.data.keyword.vpn_vpc_short}} | `vpnGatewayID: <vpn-gateway-id>` |
{: caption="Table 1. VPC resource attributes" caption-side="bottom"}

