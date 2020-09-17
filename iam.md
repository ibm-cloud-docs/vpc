---

copyright:
  years: 2019, 2020

lastupdated: "2020-06-02"

keywords: resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Getting Started with IAM 
{: #iam-getting-started}

Access to {{site.data.keyword.vpc_full}} (VPC) resources for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Every user that accesses infrastructure services resources in your account must be assigned one or more access policies that define their IAM roles. The policies determine what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Each user must also have access to the resource group that contains the infrastructure resources. A _resource group_ organizes account resources in customizable groupings so that you can quickly assign access to more than one resource at a time. Every resource that is managed by IAM belongs to a resource group within your account.

Policies enable access to be granted at different levels, for example:

* Access to all VPC infrastructure resources in your account
* Access to resources in a specific resource group
* Access to a specific resource

After you define the scope of the access policy, you
assign a role that determines the user's level of access.

## IAM roles and actions
{: #iam-roles}

The following table details what actions are mapped to platform management roles within infrastructure services. Platform management roles enable users to perform tasks on resources at the platform level, for example assign user access for the service, create or delete resources, and bind instances to applications.

| Platform management role | Description of actions | Example actions                                                 |
|--------------------------|------------------------|-----------------------------------------------------------------|
| Administrator            | All actions, including managing accounts and assigning access policies to other users            |<ul><li>Add and remove users</li><li>Assign roles for each user</li></ul>  |
| Editor                   | All actions that can modify the state of the resource (such as create, delete, and edit) as well as create and delete subresources  |<ul><li>Create, delete, and edit VPCs</li><li>Attach and detach volumes</li></ul>                    |
| Operator                 | All Viewer actions, plus actions that change the association of the resource to other resources.| <ul><li>Associate a floating IP with a virtual server instance (if you have Editor access to the instance)</li><li>Create an instance in your VPC (if you have Editor access to instances)</li></ul> |
| Viewer                   | Actions that don't change the state of resources            | <ul><li>View and list subnets</li><li>View monitoring and log data</li></ul>                     |
{: caption="Table 1. IAM user roles and actions" caption-side="top"}

### Tips
{: #iam-tips}

- Access to a container resource doesn't automatically grant access to its subresources. For example, granting access to a VPC doesn't grant access to subnets in that VPC.
- Similarly, access to a subresource does not grant access to its container resource. For example, granting access to a subnet doesn't grant access to that subnet's VPC.
- In general, to change the relationship between multiple resources, the user must have access to each resource. For example, to attach a network interface to a security group, the user must have access to both the network interface and the security group. For more information, see [Required permissions for VPC resources](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls).

For more information about assigning user roles in the UI, see [Managing user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).
 
## Resources and resource groups
{: #resources-and-resource-groups}

A _resource group_ is a collection of resources, such as an entire VPC or a single subnet, that are associated for the purpose of establishing authorization and usage. You can think of a resource group as a collection of infrastructure resources that might be used by a project, a department, or a team.

Large enterprises might divide a VPC into various resource groups, whereas smaller companies might need only one resource group because all team members have access to the entire VPC. If you are familiar with _OpenStack_, a resource group is similar in concept to a _Project_ in _OpenStack Keystone_.

Assignment of a resource to a resource group can be done only when the resource is created. Resources can't change resource groups after they are created. 

If you want to use multiple resource groups, it’s good to have a plan for how you want to assign the resources and the users in your organization to each resource group.
{: tip}

For more information about IAM, resource groups, and access groups in general, refer to these {{site.data.keyword.cloud_notm}} topics:

* [{{site.data.keyword.cloud_notm}} IAM](/docs/account?topic=account-getstarted)
* [Resource Groups](/docs/resources?topic=resources-rgs)
* [Access Groups](/docs/account?topic=account-cloudaccess)