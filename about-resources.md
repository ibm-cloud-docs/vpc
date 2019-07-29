---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-07-29"

keywords: resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# About VPC Infrastructure resources
{: #about-vpc-infrastructure-resources}

The term _resources_ refers to the component parts of a VPC deployment. Each VPC may contain resources of the following types, which can be grouped together as needed:
{:shortdesc}

* **vpc**
* **instance**
* **key**
* **image**
* **subnet**
* **volume**
* **security group**
* **floating IP**
* **vnic**
* **gateway**

## Resource policies
{: #resource-policies}

Generally, access to resources in a VPC is controlled by _policies_. If you want to customize access to resources for your VPC, you can create customized policies. A resource policy can target:

* All resources
* All resources of a type
* All resources in a group
* An individual resource

## Resources and resource groups
{: #resources-and-resource-groups}

A _resource group_ is a collection of resources, such as an entire VPC or a single subnet, that are associated for the purpose of establishing authorization and usage. You could think of a resource group as a collection of infrastructure that might be used by a project, a department, or a team.

Large enterprises might divide a VPC into resource groups. Smaller companies might not need to divide their VPC into resource groups because all of their team would be using the entire VPC. If you are familiar with _OpenStack_, a resource group is similar in concept to a _Project_ in _OpenStack Keystone_.

Assignment of a resource to a resource group can be done only when the resource is created. Resources cannot change resource groups after they are created.
{: .note}

If you want to use resource groups, it’s good to have a plan for how you want to assign the resources and the users in your organization to each resource group.
{: tip}

## Resource authorization for VPC
{: #resource-authorizations-vpc}
Some VPC infrastructure resources inherit their authorization policies, which govern their usage, from the account while others have individual policies.

### VPC resource authorization by resource type
{: #vpc-resource-authorization-by-resource-type}

The table summarizes how VPC resources are authorized by default.

| Type of resource | Where it gets its default authorization |
|-----------|------------|
| VPC | Authorization check when created |
| Subnet | Authorization check when created |
| Public gateway | Authorization check when created |
| Instance | Authorization check when created |
| Security group | Authorization check when created |
| Key | Authorization check when created |
| Image | Account |
| Floating IP | Account |
| Volume | Authorization check when created |

Floating IPs can be created with scoping at the account level, if unassigned. However, as soon as a floating IP is assigned to an instance, the resource becomes subject to the VPC's authorization scope.
{: note}

### VPC coverage of roles and authorized actions on resources
{: #roles-and-authorized-actions}

In a scenario with 2 VPCs, here are some examples of what happens when any user with certain roles attempts to perform certain actions:

* User _admin_, with **Administrator** permission on all resources
  * Creates `vpc1`: ok

* User _editor_, with **Editor** permission on all resource
  * Creates `vpc2`: ok

* User _operator_, with **Operator** permission on all resources
  * Creates `vpc`: denied

* User _viewer_, with **Viewer** permission on all resources
  * Lists VPCs: sees [vpc1][ vpc2]

* User _noRole_, with no policies
  * Lists VPCs: sees []

### VPC coverage summary table
{: #vpc-coverage-summary}

The following table lists the VPC access permissions granted per role in the early access release (X=denied, o=OK). In this release, only Create and List operations are supported.

 Role:      | None | Viewer | Operator | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Create      | X    | X      | X     | o      | o
List        | X    | o      | X     | o      | o
Read        | X    | o      | X     | X      | o
Update      | X    | X      | X     | X      | o
Delete      | X    | X      | X     | X      | o

## Resource authorization for Security Groups for VPC
{: #resource-authorizations-of-security-groups-for-vpc}

Resource authorization for **Security Groups for VPC** usage is set up separately from other resource authorizations in your VPC, but in a similar manner.

### Security Group for VPC coverage of roles and authorized actions on resources
Set the resource authorization for **Security Groups for VPC** separately from other resource authorizations in your VPC.

* As an administrator you can define roles and take any available actions on Security Groups.
* As a viewer you can take actions that don't change the state of the resources.

### Security Group coverage summary table
{: #security-group-coverage-summary-table}

The following table lists the Security Groups for VPC access permissions granted per role in the early access release (X=denied, o=OK). In this release, only Create and List operations are supported.

Role:       | None | Viewer | Operator | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Create      | X    | X      | X     | o      | o
List        | X    | o      | o     | o      | o
Read        | X    | o      | o     | o      | o
Update      | X    | X      | X     | x      | o
Delete      | X    | X      | X     | x      | o

When you create a security group, you must also have Viewer access for the VPC.
{: tip}


## Resource authorization for Virtual Server for VPC
{: #virtual-servers-for-vpc-permissions}

Set the resource authorization for **Virtual Server for VPC** separately from other resource authorizations in your VPC.

* As an administrator, you can define roles and take any available actions on {{site.data.keyword.vsi_is_short}}.
* As an operator or viewer, you can take actions that don't change the state of resources.

### Virtual server coverage summary table
{: #vsi-coverage-summary-table}

The following table lists the Virtual Server for VPC access permissions granted per role in the early access release (X=denied, o=OK). In this release, only Create and List operations are supported.

Role:       | None | Viewer | Operator | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Create      | X    | X      | X     | o      | o
List        | X    | o      | o     | o      | o
Read        | X    | o      | o     | o      | o
Update      | X    | X      | X     | x      | o
Delete      | X    | X      | X     | x      | o