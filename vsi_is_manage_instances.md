---

copyright:
  years: 2019,2021
lastupdated: "2021-08-17"

keywords: view instance details, restart, stop, instance details, delete

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}

# Managing virtual server instances
{: #managing-virtual-server-instances}
[comment]: # (linked help topic)

You can view and manage your {{site.data.keyword.vsi_is_full}} instances from the *Virtual server instances* page in {{site.data.keyword.cloud_notm}} console. Perform tasks such as start, stop, restart, and delete virtual server instances. 
{:shortdesc}

To manage your instances, complete the following steps.
1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. From here, you can manage tasks for a specific instance, or click an instance to view and edit its properties.

## Reboot

The reboot action immediately powers off a running instance and then powers it back on again.

## Stop and start

The stop and start action remotely turns an instance off or on. If the instance is stopped, the instance remains in the 
stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for 
some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped. If the instance is 
started, normal interaction and billing continues.

## Delete

To delete an instance, the instance must have a powered off status. If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The delete action permanently removes an instance and its connected vNIC, boot volume, and data from your account. If the instance has one or more attached data volumes, those volumes are preserved unless the default setting is changed to auto-delete. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30  minutes, but when the process is complete, the instance no longer appears on the virtual server instances page. 

## Viewing instance details
You can interact with instances by viewing the summary of all instances on the *Virtual server instances* page, or by clicking an individual instance name to view details and make changes. From the instance details page, you can also view the associated network interface, access its subnet, and reserve or delete a floating IP address.

## Retrieving the virtual server instance identifier
{: #retrieve-VSI-instance-identifer}

When a virtual server instance is created, that instance is automatically assigned an instance identifier (ID), which includes the SMBIOS system-uuid as a portion of the ID. The ID can be up to 64 bytes in length and it consists of digits, lower-case letters, underscores, and dashes. 

IDs are immutable, globally unique, and never reused, so the ID uniquely identifies a particular instantiation of a virtual server instance across all of IBM Cloud. The ID, including the SMBIOS system-uuid portion, is static and persists for the lifecycle of the virtual server instance until that virtual server instance is deleted. 

From within your virtual server, you can retrieve the instance identifier in one of the following ways:

Linux

```
dmidecode -s system-family
```
{: pre}

Windows

```
Get-WmiObject Win32_ComputerSystem | Select-Object -ExpandProperty SystemFamily
```
{: pre}

From within your virtual server, you can retrieve the SMBIOS system-uuid in one of the following ways:

Linux

```
dmidecode -s system-uuid
```
{: pre}

Windows

```
 Get-WmiObject Win32_ComputerSystemProduct | Select-Object -ExpandProperty UUID
 ```
{: pre}

