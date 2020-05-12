---

copyright:
  years: 2018, 2020
lastupdated: "2020-04-02"

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

# Identity and access management
{: #identity-and-access-management-iam}

You can configure access policies for an {{site.data.keyword.cloud}} Load Balancer for VPC instance. For more information about managing user access policies, see [Managing access to resources](/docs/iam?topic=iam-iammanidaccser#resourceaccess).

## Configuring resource group access policies for users
{: #configuring-resource-group-access-policies-for-users}

To create a load balancer, you need access to a resource group. The user who is creating a load balancer must have proper access to the resource group provided, or to the default resource group, if none is provided.

1. Go to **Manage > Account > Users** to see a list of the users with access to your IBM Cloud account.
2. Select the name of the user to whom you want to assign an access policy. If the user is not shown, click **Invite users** to add the user to your IBM Cloud account.
3. Select **Assign access**.
4. Select **Assign access within a resource group**.
5. From the **Resource group** list, select a resource group.
6. From the **Assign access to a resource group** list, select an access role.
7. Select **Assign** to assign the resource group access policy to the user.

## Configuring resource access policies for users
{: #configuring-resource-access-policies-for-users}

Table 1 provides information for configuring resource access policies for users.

| Platform Access Role | Load Balancer Action |
|-------------|-----|
| Administrator | Create/View/Edit/Delete load balancer |
| Editor | Create/View/Edit/Delete load balancer |
| Viewer | View load balancer |
{: caption="Table 1. Platform-access roles and load balancer actions" caption-side="top"}

1. Go to **Manage > Account > Users**.
2. Select the name of the user to whom you want to assign an access policy. If the user is not shown, click **Invite users** to add the user to your IBM Cloud account.
3. Select **Assign access**.
4. Select **Assign access to resources**.
5. From the **Services** list, select **Infrastructure Service**.
6. From the **Resource type** list, select **Load Balancer for VPC**.
7. From the **Load Balancer ID** list, select a Load Balancer instance ID, or use the default value, **All load balancers**.
8. Assign a platform access role to the user.
9. Select **Assign** to assign the access policy to the user.
