---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-16"

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network, layer-7

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Managing access for application load balancers
{: #identity-and-access-management-iam-alb}

Access to {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Every user that accesses the load balancer service in your account must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. Some of the options include the following:

* Access across all instances of the service in your account
* Access to an individual service instance in your account   

After you define the scope of the access policy, you assign a role, which determines the user's level of access. Review the following tables that outline what actions each role allows within the Flow Logs service.

Table 1 details actions that are mapped to platform management roles. Platform management roles enable users to perform tasks on service resources at the platform level, for example, assign user access for the service and create or delete instances.

For more information about IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).

## Platform-access roles

{{site.data.keyword.cloud}} Application Load Balancer supports Administrator, Editor, and Viewer platform access roles.

| Role | Description of Actions | Example Actions |
|-------------|--------------|---------------------------|
| Administrator | Allows a user to assign load balancer access policies to other users. | <ul><li>Create load balancer</li><li>View load balancer</li><li>Edit load balancer</li><li>Delete load balancer</li></ul> |
| Editor | Performs all administrative actions. | <ul><li>Create load balancer</li><li>View load balancer</li><li>Edit load balancer</li><li>Delete load balancer</li></ul> |
| Viewer |Performs actions that don't change the state of resources. | <ul><li>View load balancer</li></ul> |
{: caption="Table 1. Platform-access roles and load balancer actions" caption-side="top"}

## Related links

* [Managing identity and access](/docs/account?topic=account-getstarted)
* [Managing users and access](/docs/account?topic=account-iamuserinv)
* [What is IAM](/docs/account?topic=account-iamoverview)
