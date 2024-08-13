---

copyright:
  years: 2018, 2024
lastupdated: "2024-08-13"

keywords: virtual server instances, VSI, compute, virtual machines, planning, best practices, instances, virtual servers, virtual server instance, Virtual servers for VPC, gen 2, generation 2, infrastructure, infrastructure as a service, IaaS

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding cloud maintenance operations
{: #about-cloud-maintenance}

## Types of maintenance operations that affect your virtual server instances
{: #types-of-maintenance}

### Host and dedicated host maintenance
{: #types-of-maintenance-host}

{{site.data.keyword.cloud}} performs periodic maintenance on the server hosts and dedicated hosts that run virtual servers. This maintenance upgrades the software on the underlying hypervisor, update the firmware on the systems, or other security and performance updates. In general, users don't experience any issues during these upgrades. Modifications that require host maintenance are applied with no or little impact to running services in most cases. Scenarios can occur where the user is involved during maintenance operations, which are discussed in the [Possible impacts to virtual server instances during maintenance operations](#maintenance-impacts) section.

Most updates are done transparently to the host and the virtual servers that run on those hosts do not see any disruption. Nondisruptive changes can occur multiple times per week or even daily if necessary, all without impacting the user experience.

### Data center maintenance
{: #types-of-maintenance-data-center}

{{site.data.keyword.cloud}} also performs periodic data center maintenance upgrades. Users don't generally experience any issues during data center maintenance. Examples of this maintenance can be updates to the network, power infrastructure, or server hardware in a data center. Most maintenance is performed without impact to the user’s workloads. Some infrequent scenarios can occur where the user might need to be involved during those operations, which are discussed in the following section.

## Possible impacts to virtual server instances during maintenance operations
{: #maintenance-impacts}

Some changes can require a secure live migration of a virtual server to update the hypervisor or host. These changes can be a firmware update, an event where the hypervisor kernel cannot be live patched, or load balancing. The regular live migration process is nondisruptive and the use of dedicated hosts and virtual servers is not interrupted.

When nondisruptive live migration occurs, the virtual server experiences a brief pause of around 10 seconds, and in some cases up to 30 seconds. You are not notified in advance of nondisruptive migration. The virtual server instance is not restarted as part of this process.

In cases where a disruptive migration is required, you are notified 30 days in advance of the scheduled migration. The virtual server instance is restarted as part of this process.

In limited cases, a virtual server restart might be required to complete the host or data center maintenance. This scenario can occur when specialized VMs are used or if the virtual server encounters a migration problem. In this case, a scheduled maintenance event occurs.

For more information about unscheduled migrations that result from an unexpected host failure, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=cli).
{: note}

## What to expect during a scheduled maintenance event
{: #about-scheduled-maintenance}

In scenarios where workloads cannot migrate automatically, a scheduled maintenance event occurs. When workloads can't migrate automatically, the account owner receives an email about the upcoming maintenance that requires their attention. The account owner has a maintenance period typically 30 days in length. Exceptional cases can necessitate different timelines as outlined in [Getting advanced notice for disruptive maintenance](/docs/get-support?topic=get-support-viewing-notifications#disruptive-maintenance).

To start the maintenance event, the account owner needs to power-off their virtual server and then power it back on. This power cycle can be initiated as soon as a maintenance notification is received. When the virtual server resumes, it starts on the updated infrastructure. By doing so, the customer can choose when to perform the maintenance anytime ahead of the scheduled window.

If the account owner takes no action before the maintenance window, the server restarts during the window that is specified in the notification email.

When the virtual server is started, customer data and configurations are restored. However, with specialty virtual server types such as instance storage, the ephemeral data isn't restored. Any data that must be restored on Instance Storage needs to be backed up to {{site.data.keyword.cos_full_notm}} or to a block volume.
