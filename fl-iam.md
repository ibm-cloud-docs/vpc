---

copyright:
  years: 2020, 2024
lastupdated: "2024-06-20"

keywords: flow logs, IAM

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Managing access for flow logs
{: #fl-iam}

Access to {{site.data.keyword.cloud}} Flow Logs service instances for users in your account is controlled by {{site.data.keyword.iamlong}}. Every user that accesses the Flow Logs service in your account must be assigned an access policy with an IAM role defined. The policy determines the actions that a user can take within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be run on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. The following are some of the included options:

* Access across all instances of the service in your account
* Access to an individual service instance in your account

After you define the scope of the access policy, you assign a role, which determines the user's level of access. Review the following tables that outline the actions that each role allows within the Flow Logs service.

The following table details actions that are mapped to platform management roles. Platform management roles enable users to complete tasks on service resources at the platform level, for example, assign user access for the service and create or delete instances.

For more information about IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).

| Platform management role | Description of actions |
|--------------------------|--------------------------|
| Administrator | Read, operate, update, create, delete, and list flow log collectors |
| Editor | Read, operate, update, create, delete, and list flow log collectors |
| Operator | Operate and list flow log collectors |
| Viewer | Read flow log collectors |
{: caption="Table 1. IAM user roles and actions" caption-side="bottom"}

For more information about assigning user roles in the console, see [Managing access to resources](/docs/account?topic=account-assign-access-resources).

Only one operator role is needed, as determined by the scope of your flow log collector.
{: important}

In addition, you also require the following actions and operations that are not specific to {{site.data.keyword.cloud_notm}} Flow Logs.

| Role                | Description of actions    |
| ---------------------------- | --------------------------- |
| Writer on {{site.data.keyword.cos_short}} bucket | Create flow log collector |
| Operator on Subnet     | Create flow log collector with Subnet scope    |
| Operator on VPC    | Create flow log collector with VPC scope    |
| Operator on virtual server instance | Create flow log collector with Instance or Interface scope  |
{: caption="Table 2. Additional IAM user roles and actions" caption-side="bottom"}

Operator roles in the following table are required only if the target scope is being changed.
{: important}

| Role                   | When needed                 |
| ---------------------------- | --------------------------- |
| Writer on {{site.data.keyword.cos_short}} bucket | (Change {{site.data.keyword.cos_short}} bucket) |
| Operator on Subnet     | (To Subnet scope)           |
| Operator on VPC        | (To VPC scope)              |
| Operator on virtual server instance | (To Instance or Interface scope) |
{: caption="Table 3. IAM roles only if the target scope is being changed" caption-side="bottom"}

Each aggregator creates a separate stream of data to {{site.data.keyword.cos_short}}. Since you can create a flow log collector that associates data that is captured from multiple interface IDs with a single {{site.data.keyword.cos_short}} bucket, each bucket needs a folder structure for holding data.
{: note}
