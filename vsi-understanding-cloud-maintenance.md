---

copyright:
  years: 2018, 2021
lastupdated: "2021-09-27"

keywords: virtual server instances, VSI, compute, virtual machines, planning, best practices, instances, virtual servers, virtual server instance, Virtual servers for VPC, gen 2, generation 2, infrastructure, infrastructure as a service, IaaS

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}


# Understanding cloud maintenance operations
{: #about-cloud-maintenance}

## Types of maintenance operations that affect your virtual server instances
{: #types-of-maintenance}

### Host maintenance
{: #types-of-maintenance-host}

{{site.data.keyword.cloud}} performs periodic maintenance on the underlying server hosts that run virtual servers. This maintenance can be upgrades to the software on the underlying hypervisor, updates to the firmware on the systems or other security and performance updates. In general, users don't experience any issues during these upgrades. Modifications that require host maintenance are, in most cases, which are applied with no or little impact to running services. Some infrequent scenarios can occur where the user might need to be involved during those operations, which are discussed in the [possible impacts](#maintenance-impacts) section.

Most updates are done transparently to the host and the virtual servers that run on those hosts do not see any disruption. Non-disruptive changes can occur multiple times per week or even daily if necessary, all without impacting the user experience.

### Data center maintenance
{: #types-of-maintenance-data-center}

{{site.data.keyword.cloud}} also performs periodic data center maintenance upgrades. Users don't generally experience any issues during data center maintenance. Examples of this maintenance can be updates to the network, power infrastructure, or server hardware in a data center. Most maintenance is performed without impact to the user’s workloads. Some infrequent scenarios can occur where the user might need to be involved during those operations, which are discussed in the following section.

## Possible impacts to virtual server instances during maintenance operations
{: #maintenance-impacts}

Some of these changes might require that a virtual server needs to be live migrated to resolve an underlying issue on the hypervisor or host, such as a firmware update or a rare event where we can't live patch the hypervisor kernel. When live migration occurs, the virtual server experiences a brief pause of around 10 seconds, and in some cases up to 30 seconds. The virtual server instance is not restarted as part of this process.

In limited cases, a virtual server restart might be required to complete the host or data center maintenance. This scenario can occur when specialized VMs are used, such as virtual servers with Instance Storage, or if the virtual server encounters a migration problem. In this case, a scheduled maintenance event occurs.

## What to expect during a scheduled maintenance event
{: #about-scheduled-maintenance}

In scenarios where workloads can not migrate automatically, a scheduled maintenance event will occur.  When this happens, the account owner receives an email about upcoming maintenance that requires their attention. The account owner has a maintenance period typically 30 days in length. Exceptional cases can necessitate different timelines as outlined in [Getting advanced notice for disruptive maintenance](/docs/get-support?topic=get-support-viewing-notifications#disruptive-maintenance).

To start the maintenance event, the account owner needs to power-off their virtual server and then power it back on. This power cycle can be initiated as soon as a maintenance notification is received. When the virtual server resumes, it starts on the updated infrastructure. By doing so, the customer can chose when to perform the maintenance any time ahead of the scheduled window.

If the account owner takes no action before the maintenance window, the server restarts during the window specified in the notification e-mail.

When the virtual server is started, customer data and configurations are restored. However, with specialty virtual server types such as instance storage, the ephemeral data isn't restored. Any data that must be restored on Instance Storage should be backed up to Cloud Object Storage or to a block volume.
