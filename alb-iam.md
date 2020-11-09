---

copyright:
  years:  2017, 2020
lastupdated: "2020-04-15"

keywords: IAM access for _servicename_, permissions for _servicename_, identity and access management for _servicename_, roles for _servicename_, actions for _servicename_, assigning access for _servicename_

subcollection: _your-subcollection_

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Managing access for _servicename_
{: #alb-iam-docs-template}

Access to `service name` service instances for users in your account is controlled by {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Every user that accesses the `service name` service in your account must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.Bluemix_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. Some of the options include the following:

* Access across all instances of the service in your account
* Access to an individual service instance in your account <!-- if this applies -->
* Access to a specific resource within an instance <!-- if this applies list what resoureceType attributes are supported -->


After you define the scope of the access policy, you assign a role, which determines the user's level of access. Review the following tables that outline what actions each role allows within the `service name` service.

The following table details actions that are mapped to platform management roles. Platform management roles enable users to perform tasks on service resources at the platform level, for example, assign user access for the service, create or delete instances, and bind instances to applications.

| Platform management role | Description of actions |
|--------------------------|------------------------|
| Viewer                   | Description            |
| Editor                   | Description            |
| Operator                 | Description            |
| Administrator            | Description            |
{: caption="Table 1. IAM user roles and actions" caption-side="top"}


The following table details actions that are mapped to service access roles. Service access roles enable users access to `service name` and the ability to call the `service name's` API.

| Service access role | Description of actions |
|---------------------|------------------------|
| Reader              | Description            |
| Writer              | Description            |
| Manager             | Description            |
{: caption="Table 2. IAM service access roles and actions" caption-side="top"}


For information about assigning user roles in the console, see [Managing access to resources](/docs/account?topic=account-assign-access-resources).

<!-- You can add an extra column to each table if you want to provide the specific action name in dot notation as it is used in the service's registration with IAM. For example: key-protect.keys.create, key-protect.keys.delete) -->
