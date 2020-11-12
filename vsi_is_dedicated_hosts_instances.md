---

copyright:
  years: 2020 
lastupdated: "2020-11-12"

keywords: vpc, vsi, dedicated host, dedicated instance, virtual server instance, creating, UI, console

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:beta: .beta}
{:table: .aria-labeledby="caption"}

# Creating dedicated hosts and groups (Beta)
{: #creating-dedicated-hosts-instances}

You can create one or more dedicated groups with dedicated hosts in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console.
{:shortdesc}

Dedicated hosts are only available to accounts with special approval to preview this beta feature. 
{:beta}

## Dedicated hosts
{: #dedicated-hosts}

You can create a *dedicated host* to carve out a single-tenant compute node, free from users outside of your organization. Within that dedicated space, you can create virtual server instances according to your needs. Additionally, you can create dedicated groups that contain dedicated hosts for a specific purpose. Because a dedicated host is a single-tenant space, only users within your account that have the required permissions can create instances on the host. 

When you create a dedicated host, you are billed by the usage of the host on an hourly basis. 
You are not billed for instances that are running on the host. 

## Dedicated groups
{: #dedicated-groups}

A *dedicated group* is a collection of dedicated hosts in a single region and zone. When you create a dedicated host, you must 
assign it to a dedicated group. A dedicated host can be a member of only one group. You assign a resource 
group to the dedicated group that contains the resources and users that you want to access the dedicated group. <!--A dedicated group can provide redundancy. If a host within the group happens to fail, 
instances running on that host can be migrated to another host in the same group.-->

You can create dedicated groups that contain dedicated hosts for a distinct function. For example, if you have multiple business 
units within your organization, you might want to separate the physical compute infrastructure that is used by each business. 
If you want to assign compute resources to be used by only one business group within your organization, you can create a dedicated group 
with dedicated hosts for that unique goal. 

When you provision an instance, you can provision it to either a dedicated host or to a dedicated group.

## Creating dedicated groups 
{: #creating-dedicated-groups}

You can create one or more dedicated groups in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console.

Before you can create a dedicated group or a dedicated host, you need to [create an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{:important}

To create a dedicated group:
1. In the [{{site.data.keyword.cloud_notm}} console]](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Dedicated hosts**. 
2. Click the **Dedicated groups** tab.
3. Click **New dedicated group** and enter the information in Table 1.
3. Click **Create dedicated group** when the information is complete.

| Field | Value |
|-------|-------|
| Dedicated group name  | A unique resource name within the region is required for your dedicated group. If you don't specify a name, a name is generated and assigned to your dedicated group. |
| Zone | Select the specific fault-tolerant data center within a region where you want to create the group. |
| Resource group | Select the resource group that contains the account resources and users that you want to be able to access the dedicated hosts within this dedicated group. For more information about resource groups, see [Managing resource groups](/docs/account?topic=account-rgs).  |
{: caption="Table 1. Dedicated group creation selections" caption-side="top"}

## Creating dedicated hosts 
{: #creating-dedicated-host}

You can create one or more dedicated hosts in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console.

To create a dedicated host:
1. In the [{{site.data.keyword.cloud_notm}} console]](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Dedicated hosts**. 
2. Click **New dedicated host** and enter the information in Table 2.
3. Click **Create dedicated host** when you are ready to provision.

| Field | Value |
|-------|-------|
| Dedicated host name  | A unique resource name within the region is required for your dedicated host. If you don't specify a name, a name is generated and assigned to your dedicated host. |
| Dedicated group | Select the dedicated group where you want this dedicated host created. Or you can create a new dedicated group. |
| Instance placement | Select whether you want to enable the placement of instances on this dedicated host. The default value of instance placement is set to *On*. If you set the instance placement value to *Off*, no instances can be created on this dedicated host until the value is changed to *On*.|
| Profile | Select a profile to define the vCPU and memory for the dedicated host.  |
{: caption="Table 2. Dedicated host provisioning selections" caption-side="top"}

## Creating instances on dedicated hosts
{: #creating-dedicated-instance}

When you have a dedicated host created within a dedicated group, you can start provisioning virtual server instances on the dedicated host. 

To create an instance on a dedicated host:
1. In the [{{site.data.keyword.cloud_notm}} console]](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Dedicated hosts**. 
2. If you want to create an instance on a specific dedicated hosts, on the **Dedicated hosts** tab click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the host where you want the instance created and select **New instance**.
3. If you want to create an instance on any dedicated host within a dedicated group, on the **Dedicated groups** tab click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the dedicated group where you want the instance created and select **New instance**.

