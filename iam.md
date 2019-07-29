---

copyright:
  years: 2019

lastupdated: "2019-07-29"

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

## Identity and Access Management roles and actions
{: #iam-roles}

Access to {{site.data.keyword.vpc_full}} (VPC) resources for users in your account is controlled by {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Every user that accesses VPC Infrastructure resources in your account must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.Bluemix_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. Some of the options include the following: 

* Access across all instances of the service in your account
* Access to an individual service instance in your account
* Access to a specific resource within an instance

After you define the scope of the access policy, you 
assign a role that determines the user's level of access. 

The following table details what actions are mapped to platform management roles within the VPC Infrastructure service. Platform management roles enable users to perform tasks on service resources at the platform level, for example assign user access for the service, create or delete instances, and bind instances to applications.

| Platform management role | Description of actions | Example actions                                                 |
|--------------------------|------------------------|-----------------------------------------------------------------|
| Viewer                   | Actions that don't change the state of resources            | <ul><li>View and list subnets</li><li>View monitoring and log data</li></ul>                     |
| Operator                 | Actions required to operate virtual server instances  | <ul><li>Start, stop, and reboot virtual server instances</li></ul> |
| Editor                   | All actions that can modify the state of the resource (such as create, delete, and edit) as well as create and delete subresources  |<ul><li>Create, delete, and edit VPCs</li><li>Attach and detach volumes</li></ul>                    |
| Administrator            | All actions, including managing accounts and assigning access policies to other users            |<ul><li>Add and remove users</li><li>Assign roles for each user</li></ul>  |
{: caption="Table 1. IAM user roles and actions" caption-side="top"}


**Tips:**
- Access to a container resource doesn't automatically grant access to its subresources. For example, granting access to a VPC doesn't grant access to subnets in that VPC.
- Similarly, access to a subresource does not grant access to its container resource. For example, granting access to a subnet doesn't grant access to that subnet's VPC.

- In general, to change the relationship between multiple resources, the user must have access to each resource. For example, to attach a network interface to a security group, the user must have access to both the network interface and the security group. For more information, see [Required authorizations for VPC resources](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls).

For more information about assigning user roles in the UI, see [Managing user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).
 
## Resources and resource groups
{: #resources-and-resource-groups}

A _resource group_ is a collection of resources, such as an entire VPC or a single subnet, that are associated for the purpose of establishing authorization and usage. You could think of a resource group as a collection of infrastructure that might be used by a project, a department, or a team.

Large enterprises might divide a VPC into resource groups. Smaller companies might not need to divide their VPC into resource groups because all of their team would be using the entire VPC. If you are familiar with _OpenStack_, a resource group is similar in concept to a _Project_ in _OpenStack Keystone_.

Assignment of a resource to a resource group can be done only when the resource is created. Resources can't change resource groups after they are created.

If you want to use resource groups, it’s good to have a plan for how you want to assign the resources and the users in your organization to each resource group.
{: tip}

For more information about IAM, resource groups, and access groups in general, refer to these IBM Cloud topics:

* [IBM Cloud IAM](/docs/iam?topic=iam-getstarted)
* [Resource Groups](/docs/overview?topic=overview-whatis-rgs)
* [Access Groups](/docs/overview?topic=overview-cloudaccess)