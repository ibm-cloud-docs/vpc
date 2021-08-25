---

copyright:
  years: 2019, 2021
lastupdated: "2021-08-25"

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

## Adjusting bandwidth allocation using the UI
{: #adjusting-bandwidth-allocation-ui}
{: ui}

You can adjust the allocation of your instance's total bandwidth between network bandwidth and storage bandwidth using the UI.

To adjust the bandwidth of an instance:

1. Navigate to a virtual service instance.
2. Select Bandwidth allocation.
3. On the Edit bandwidth allocation screen, adjust the value for Storage bandwidth. You can increase the bandwidth allocated for your block storage boot and attached data volumes. For information about how storage bandwidth is allocated, see [Bandwidth allocation for block storage volumes](/docs/vpc?topic=vpc-block-storage-bandwith).
After you set storage bandwidth, network bandwidth is automatically adjusted so the total bandwidth of the instance equals the displayed Total bandwidth value. The value for Network bandwidth or Storage bandwidth cannot be set less than 500 Mbps.

To view the new bandwidth allocation, you must either stop and start the instance, or detach and reattach secondary volumes.
{: note}

## Adjusting bandwidth allocation using the CLI
{: #adjusting-bandwidth-allocation-cli}
{: cli}

You can adjust the allocation of your instance's total bandwidth between network bandwidth and storage bandwidth using the CLI.

To reallocate instance bandwidth by using the CLI, run the `instance-update {id}` command and specify the total storage bandwidth in megabits per second (Mbps) for the `total-volume-bandwidth` parameter. Use this syntax:
```
ibmcloud is instance-update {id} --total-volume-bandwidth VALUE
```
{: pre}

## Adjusting total storage bandwidth allocation from the API
{: #adjusting-bandwidth-allocation-api}
{: api}

You can adjust total storage bandwidth for an existing instance. Make a `PATCH /instances` call and specify `total_volume_ bandwidth`. Total storage bandwidth (in megabits per second) is the total bandwidth allocated for primary boot and secondary attached data volumes. Increasing total storage bandwidth results in a corresponding decrease in network bandwidth. The minimum network bandwidth you can have is 500 mbps, so adjust total storage bandwidth accordingly. For example,

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-22&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
      "total_volume_bandwidth": 500
      }'
```
{: pre}

Bandwidth allocation is also realized when you add a secondary volume using `POST / volume_attachments` or delete a volume using `DELETE volume_attachments`.

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

