---

copyright:
  years: 2020 
lastupdated: "2020-04-10"

keywords: manage dedicated host, manage dedicated group, delete dedicated host, disable placement

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Managing dedicated hosts and groups (Beta)
{: #manage-dedicated-hosts-groups}

With a dedicated group and dedicated host created, you can manage your resources by creating additional dedicated hosts, 
provisioning new virtual server instances, or deleting resources as needed. You can also disable placement of new instances on 
a dedicated host. Disabling placement of new instances is required if you need to delete a dedicated host. 
{:shortdesc}

## Managing dedicated hosts
{: #manage-dedicated-hosts}

When you have a dedicated host created, you can create new virtual server instances on the host or disable placement of 
instances on the host. If you no longer need a dedicated host or dedicated group, you can delete the resources. 

To complete management tasks on your dedicated hosts, complete the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) >
VPC Infrastructure > Dedicated hosts**.
2. On the Dedicated hosts tab of the Dedicated hosts for VPC page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the dedicated host that you want to manage. You can select 
from the following actions:

| Action | Description |
|--------|-------------|
| New virtual server | Create a virtual server instance on this dedicated host. |
| Disable placement | Temporarily prevent new virtual server instances from being provisioned on this host. For example, you must disable placement before you can delete a dedicated host.|
| Delete | If you no longer need the dedicated host, you can delete it to permanently remove it from your account. Before you can delete the dedicated host, you must first delete any virtual server instances on the host. (Click the name of the dedicated host to access the Host details page which displays instances on the host.) Then, you must disable the placement of any new virtual server instances on the dedicated host. This prevents any new instances from being created before you can delete the dedicated host. |
{: caption="Table 1. Actions available for dedicated hosts" caption-side="top"}

## Managing dedicated groups
{: #manage-dedicated-groups}

When you have a dedicated group created, you can create new dedicated hosts or virtual server instances within the group. If 
you no longer need a dedicated group, you can delete it. 

To complete management tasks on your dedicated groups, complete the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) >
VPC Infrastructure > Dedicated hosts**.
2. Click the **Dedicated groups** tab on the Dedicated hosts for VPC page. Then click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the dedicated host that you want to manage. You can select 
from the following actions:

| Action | Description |
|--------|-------------|
| New virtual server | Create a virtual server instance in this dedicated group. The virtual server instance is provisioned on any dedicated host in the group that has space available. |
| New dedicated host | Create a dedicated host within this dedicated group.|
| Delete | If you no longer need the dedicated group, you can delete it to permanently remove it from your account. Before you can delete the dedicated group, you must first delete all dedicated hosts within the group. |
{: caption="Table 2. Actions available for dedicated groups" caption-side="top"}
