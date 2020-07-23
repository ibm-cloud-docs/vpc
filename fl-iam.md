---

copyright:
  years: 2020
lastupdated: "2020-07-23"

keywords: flow logs, iam

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Managing access for flow logs
{: #fl-iam}

Access to {{site.data.keyword.cloud}} Flow Logs service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Every user that accesses the Flow Logs service in your account must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. Some of the options include the following:

* Access across all instances of the service in your account
* Access to an individual service instance in your account   

After you define the scope of the access policy, you assign a role, which determines the user's level of access. Review the following tables that outline what actions each role allows within the Flow Logs service.

The following table details actions that are mapped to platform management roles. Platform management roles enable users to perform tasks on service resources at the platform level, for example, assign user access for the service and create or delete instances.

For more information about IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).

| Platform management role | Description of actions |
|--------------------------|--------------------------|
| Administrator | Read, operate, update, create, delete, and list flow log collectors |
| Editor | Read, operate, update, create, delete, and list flow log collectors |
| Operator | Operate and list flow log collectors |
| Viewer | Read flow log collectors |
{: caption="Table 1. IAM user roles and actions" caption-side="top"}

For information about assigning user roles in the console, see [Managing access to resources](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

Only one operator role is needed, as determined by the scope of your flow log collector.
{: important}

In addition, you also require the following actions and operations that are not specific to {{site.data.keyword.cloud_notm}} Flow Logs.

| Additional role                | Description of actions    |
| ---------------------------- | --------------------------- |  
| Write on COS bucket  | Create flow log collector |
| Operator on Subnet     | Create flow log collector with Subnet scope    |
| Operator on VPC    | Create flow log collector with VPC scope    |
| Operator on VSI | Create flow log collector with Instance or Interface scope  |
{: caption="Table 2. Additional IAM user roles and actions" caption-side="top"}

Operator roles in the following table are required only if the target scope is being changed.
{: important}

| Role                | When needed                 |  
| ---------------------------- | --------------------------- |  
| Write on COS bucket            | (Change COS bucket)         |  
| Operator on Subnet     | (to Subnet scope)           |  
| Operator on VPC           | (to VPC scope)              |  
| Operator on VSI | (to Instance or Interface scope) |
{: caption="Table 3. IAM roles only if the target scope is being changed" caption-side="top"}

Each aggregator creates a separate stream of data to COS. Since you can create a flow log collector that associates data that is captured from multiple interface IDs with a single COS bucket, each bucket needs a folder structure for holding data.
{: note}
