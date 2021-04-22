---

copyright:
  years: 2018, 2021
lastupdated: "2021-04-02"

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


# Understanding Cloud Maintenance Operations
{: #about-cloud-maintenance}

## Types of maintenance operations that could affect your virtual server instances
{: #types-of-maintenance}

### Host Maintenance
{: #types-of-maintenance-host}

IBM performs periodic maintenance on the underlying server hosts that are running customer’s virtual servers. This could be upgrades to the software on the underlying hypervisor, updates to the firmware on the systems or other security and performance updates. In general, users will not experience any issues during these upgrades. Modifications that require host maintenance will, in most cases, be applied with no or little impact to running services. There are some infrequent scenarios where the user might need to be involved during those operations, which are discussed in possible impacts below.

### Data Center Maintenance
{: #types-of-maintenance-data-center}

IBM also performs periodic data center maintenance upgrades. Users will not generally experience any issues during data center maintenance.  Examples of this can be updates to the network, power infrastructure or server hardware in a data center.  Most can be performed without impact to the user’s workloads.  There are some infrequent scenarios where the user might need to be involved during those operations, which are discussed in possible impacts below.

## Possible impacts to virtual server instances during maintenance operations
{: #maintenance-impacts}

Most updates are done transparently to the host and the virtual servers that are running on those hosts do not see any disruption. Non-disruptive changes can occur multiple times per week or even daily if necessary, all without impacting the end user experience.

Some of these changes might require that a virtual server be live migrated to resolve an underlying issue on the hypervisor or host, such as a firmware update or a rare event where we cannot live patch the hypervisor kernel. When live migration occurs, the virtual server will experience a brief pause of around 10 seconds, and in some cases up to 30 seconds. The virtual server instance is not rebooted as part of this process.

In limited cases a virtual server restart might be required to complete the host or data center maintenance. This scenario can occur when specialized VMs are used, such as virtual servers with Instance Storage, or if the virtual server encounters a problem when migrating. In this case, a scheduled maintenance event will occur.

## What to expect during a Scheduled Maintenance Event
{: #about-scheduled-maintenance}

In this rare case, the account owner will be emailed about the upcoming maintenance that requires their attention. They are given a maintenance period during which they will need to power off their virtual server and then power it back on. This causes the virtual machine to then be started on updated infrastructure.  

When virtual servers are restarted, customer data and configuration is restored.  However, in the case of specialty virtual server types such as instance storage, the ephemeral data will not be restored. 

Advanced notice is given for the maintenance window.  Review [IBM Cloud Notifications](/docs/get-support?topic=get-support-viewing-notifications) for details on advanced notice for disruptive maintenance. If the user takes no action before the maintenance window, the virtual server is restarted during the maintenance window on their behalf.
