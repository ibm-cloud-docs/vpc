---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-07-29"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, control

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Assigning role-based access to VPC resources
{: #assigning-role-based-access-to-vpc-resources}

Account administrators can use authorization policies to control access to resources based on an individual user's role. Policies also can be applied for:

(1) the authorization of a group of users, called an _access group_,

(2) on the assigned characteristics of a single _resource_, or

(3) on a collection of resources called a _resource group_.

The authorizations for resources and the authorizations for users can be assigned independently of each other.
{:shortdesc}

For more information about creating users, user access groups, resource groups, and policies, see [About resources](/docs/vpc?topic=vpc-about-vpc-infrastructure-resources).

## IAM-based access control
{: #iam-based-access-control}

In general, the {{site.data.keyword.vpc_full}} authorization and resource management practices coordinate with the IBM Cloud Identity and Access Management (IAM) services. For more information about IAM, resource groups, and access groups in general, refer to these IBM Cloud documents:

* [IBM Cloud IAM](/docs/iam?topic=iam-getstarted)
* [Resource Groups](/docs/overview?topic=overview-whatis-rgs)
* [Access Groups](/docs/overview?topic=overview-cloudaccess)

You can assign IAM-based authorizations based on:

* individual users
* access groups (groups of users)
* specific types of resources
* resource groups

## Assigning user permissions
{: #assigning-user-permissions}

For users, access is controlled by assigning system-defined IAM roles:

* Administrator
* Editor
* Operator
* Viewer

Here's a table that shows the correspondence between each user role and the type of control it allows for VPCs in the early access release:

| Role Name | Type of Access Allowed |
|-----------|-------------------------|
| Viewer | View VPC, List VPCs  |
| Editor | View VPC, List VPCs |
| Operator  | View VPC, List VPCs |
| Administrator |View VPC, List VPCs, Create VPCs, Delete VPCs, Update VPCs, Assign policies to other users |


## Next steps
{: #permissions-next-steps}

For step by step instructions on granting role-based permissions to users for certain tasks, see [Managing user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).
