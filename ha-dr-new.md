---

copyright:
  years: 2026
lastupdated: "2026-01-26"

keywords: HA for VPC, DR for VPC, VPC recovery time objective, VPC recovery point objective

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability and disaster recovery for {{site.data.keyword.vpc_short}}
{: #ha-dr-vpc}

[High availability](#x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures. [Disaster recovery](#x2113280){: term} is the process of recovering the service instance to a working state.
{: shortdesc}

{{site.data.keyword.vpc_full}} is a highly available service that is designed to meet the [Service Level Objectives (SLO)](/docs/resiliency?topic=resiliency-slo). It is composed of both zonal and regional services.

For more information about the available region and data center locations, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

## High availability architecture
{: #vpc-ha-architecture}



VPC resources are divided into control plane and data plane services to enable customers to build highly available applications on top of VPC. The control plane exists to provision and manage VPC resources (create, update, delete) and to provide control functions. The data plane is the collection of provisioned VPC resources like virtual server instances, floating IP addresses, security groups, block storage, and more.

The control plane is hosted on redundant hardware across zones, which provides resiliency for hardware and zonal failures. The control plane and data plane are on different failure domains. For example, a control plane outage does not impact the availability of the data plane. All existing customer resources continue to run without any impact. For increased resiliency, users can build applications from redundant data plane resources.

VPC resources are categorized into zonal resources and regional resources based on their scope. Some resources, such as VPC, span across multiple zones and are considered regional resources. Most resources are zonally scoped and are available in a specific zone. For example, subnets, access control lists, security groups, routing tables, public gateways, and Virtual Private Endpoint gateways exists in the zone that they are created.

For more information about data plane protection from control plane faults, zonal service independence, and regional service redundancy, see [IBM Cloud service architecture for high availability and resiliency](/docs/resiliency?topic=resiliency-ha-redundancy#services-architecture).

### Zone failure
{: #zone-failure}

If a complete zone failure occurs, both the control plane and data plane are impacted on the zone. Control functions in the affected zones are not available, and all zonal resources are down. For example, virtual server instances in the affected zone are unavailable and are not moved to another healthy zone. Any changes that are made to the regional resources do not take effect on the failed zone until the zone recovers.

The data plane in other zones is unaffected, and all zonal resources in the unaffected zones continue to function without any disruption. Regional resources like VPC continue running on the healthy zones. The control plane is highly available and enables services to manage the resources in the other unaffected zones.

Customers should develop mechanisms to manage the high availability of their applications by spreading resources across zones (failure domains), and plan for disaster recovery.

### Regional failure
{: #regional-failure}

In the unusual event of a regional disaster, any underlying problems are resolved and the VPC control plane is restored with a focus on reducing data loss for resources. The data plane is also restored by recovering the state of the customer data from storage with an objective of meeting the recovery point objective (RPO) and recovery time objective (RTO).

On a single-campus multizone region (SC-MZR), a data center disaster could impact the entire region because zones are more tightly related. Services should employ backup and recovery strategies to another MZR to avoid data loss.

### Hardware failures
{: #hardware-failures}

Resources are served from reliable and often redundant hardware, but an unforeseen hardware failure might take down these resources. For example, a virtual server instance might fail when the underlying hardware fails. In this situation the [host failure recovery policy](/docs/vpc?topic=vpc-host-failure-recovery-policies) determines how the virtual server is recovered. If the failure occurs and the host failure recovery policy is set to the default setting, `restart`, the control plane detects the hardware failure and migrates the virtual server to available hardware in the same zone and restarts the virtual server. Ephemeral disk storage is not restored to the boot volume. Data volumes are available, but they might be missing application or operating system cache writes that are not saved at the time of failure.

Block volumes are backed by redundant hardware with advanced replication techniques to improve resiliency. However, a zonal disaster or multi-hardware failure might result in block volume failure. Backup and restore are a suitable approach for mitigating data loss or corruption. This approach can also be used to mitigate against a regional disaster by replicating data to other {{site.data.keyword.cloud_notm}} regions. The snapshot capability can be used to support backup and restore. Customers can also distribute their applications across other zones to avoid any disruptions and improve RPO/RTO.

A strong failure correlation can exist between volume storage and associated server application failure.
Be sure to examine and test workloads to determine application behavior in the presence of a failing storage device.

For information about bare metal servers and their associated storage, see [Storage overview for Bare Metal Servers for VPC](/docs/vpc?topic=vpc-bare-metal-servers-storage). Snapshots are not available for the local disks on bare metal servers. Customers must manage the high availability and disaster recovery for these devices.

### Building HA applications
{: #ha-applications}

You can use a VPC load balancer to distribute incoming requests to multiple virtual servers and bare metal servers. Virtual servers and bare metal servers that become unavailable stop responding to health checks; the load balancer then balances the load on the available resources.
You can use an Application Load Balancer (ALB) to distribute the traffic of a workload to virtual servers in multiple zones and to create workloads that are available even when an entire zone becomes unavailable.

When the ALB itself is configured on subnets across zones, it is resilient from a single zone failure. The Network Load Balancer is a zonal service that is distributed on multiple underlying virtual servers and resilient to a single virtual server failure.

The basic strategy for improving availability for workloads that are constructed from VPC resources is to distribute the workload over multiple resources. It is possible to distribute resources within a zone, across multiple zones in a multizone region (MZR), or across multiple regions. For more information, see [Deploy isolated workloads across multiple locations and zones](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region); this strategy uses IBM Cloud Internet Services (CIS) and a Global Load Balancer.

### High availability features
{: #vpc-ha-features}

{{site.data.keyword.vpc_short}} supports the following high availability features:



| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| [Application Load Balancer](/docs/vpc?topic=vpc-load-balancers-about) | An ALB distributes load across zones to IP addresses. | The workload must be scalable. |
| [Network Load Balancer](/docs/vpc?topic=vpc-network-load-balancers) | An NLB distributes load to IP addresses within a zone. | The workload must be scalable. |
| [Auto scale for VPC](/docs/vpc?topic=vpc-creating-auto-scale-instance-group) | Improve performance and costs by dynamically creating virtual server instances to meet the demands of your environment. | The workload must be scalable. |
| [Load balancer and instance group](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui#creating-instance-group) | Scalable workloads that are fronted by a load balancer distribute load to multiple instances across zones. | The workload must be scalable. |
{: caption="HA features for {{site.data.keyword.vpc_short}}" caption-side="bottom"}

As a customer, you can create and support the HA:

| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Scalable workload | Create a virtual server or bare metal server-based workload that can be scaled horizontally with more servers. | Not all workloads can be scaled horizontally. |
{: caption="Customer HA features for {{site.data.keyword.vpc_short}}" caption-side="bottom"}

#### Scalable workload
{: #vpc-ha-feature-scalable-workload}

Scalable workloads can handle increased demand by adding more servers that are running the same image. The scalable workload can be implemented by using load balancers and Auto Scale for VPC.



## Disaster recovery architecture
{: #vpc-disaster-recovery-intro}



The strategy for disaster recovery is to provide scripting automation to restore a VPC workload in a recovery location. For example, when a region becomes unavailable it is the customer's responsibility to migrate the workload and associated data to an available region. IBM has support for the Terraform infrastructure as code system that can be used to define workloads with parameterized locations and performance. The VPC API, SDK, and CLI can be used by customers to create scripts to recover resources in an available location during a disaster. For more information, see [Planning for disaster recovery](/docs/resiliency?topic=resiliency-PlanningforDR).

You can learn more about using IBM Cloud Object Storage, IBM Cloud Schematics that provide Terraform-as-a-Service, and deployable architectures in [Using IBM Cloud services in your disaster recovery](/docs/resiliency?topic=resiliency-KeyServicesTitle).

### Disaster recovery features
{: #vpc-dr-features}

{{site.data.keyword.vpc_short}} supports the following disaster recovery features:



| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| [Single volume snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) | A snapshot is a point-in-time copy of your boot or data volume. Block Storage snapshots are stored in the regional Cloud Object Storage instance. | A snapshot is independent from its source volume. If the source volume's zone is unavailable, the snapshot can be used to create a new volume in a different zone in the region. Snapshots can be created on-demand in the console, from the CLI, with the API or Terraform. They can also be scheduled by using the Backup for VPC service. |
| [Cross-regional snapshot copies](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots_vpc_crossregion_copy) | You can use cross-regional snapshot copies that are independent from their source snapshots to create new volumes. | Snapshots can be copied to another region manually in the console, from the CLI, or programmatically with the API or Terraform. You can also include the creation of a copy in other region in your backup policy.|
| [Consistency group snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#multi-volume-snapshots) |A snapshot consistency group contains snapshots of multiple Block Storage volumes that are attached to the same virtual server instance. | When you request a snapshot of a consistency group, the system generates snapshots of all the tagged Block Storage volumes that are attached to the virtual server instance at the same time. You can include or exclude boot volumes. Instance storage is not included.|
| [Fast restore snapshots](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-fr) | Fast restore snapshots are snapshots that are cached in the zone with the parent block volume. When you create a virtual server with a bootable fast restore snapshot, the server becomes fully operational faster compared to when you provision its boot volume from a normal snapshot. |
| [File share replication](/docs/vpc?topic=vpc-file-storage-replication) | If the source share becomes unavailable, you can initiate a [replication failover](/docs/vpc?topic=vpc-file-storage-failover&interface=ui) to the replica share. | You can create a replica share in another zone of the same region. You can also create a replica in another region in the same geography. You can replicate your data every 15 minutes. |
{: caption="DR features for {{site.data.keyword.vpc_short}}" caption-side="bottom"}

Restoring a volume or a share from a snapshot is a manual operation that takes time. If you require a higher level of service for disaster recovery, see IBM Cloud's [Backup and recovery services](/docs/backup-recovery).
{: tip}



As a customer, you can create and support additional disaster recovery options:

| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| External source of truth for VPC configuration | Construction of VPC, network, and servers captured in customer-managed configuration files like Terraform scripts, shell scripts, or programs. | Customer must create the script and persist the configuration where it can be used during a potential disaster. |
| Customer-created scripts for file storage backup and copy | Copy the contents of a file share to make it available in another location. | Customer must create the script or use customer-managed continuous backup and restore. |
| Customer-managed continuous backup and restore for block volumes and file storage | Customers can install 3rd-party agents and OS drivers on servers that integrate with backup and recovery systems such as [Veeam](/docs/vpc?topic=vpc-about-veeam). | Customer must install and manage the 3rd party backup and recovery solution. |
{: caption="Customer DR features for {{site.data.keyword.vpc_short}}" caption-side="bottom"}

### Planning for disaster recovery
{: #vpc-features-for-disaster-recovery}

The disaster recovery steps must be practiced regularly. As you build your plan, consider the following failure scenarios and resolutions.

| Failure | Resolution |
| -------------- | -------------- |
| Boot volume failure | You can create a new volume from a [custom image that you created from your volume](/docs/vpc?topic=vpc-image-from-volume-vpc). Or you can [restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore) in the console, from the CLI, with the API, Terraform, or by using a customer-managed continuous backup and restore solution. When you restore a boot volume from a snapshot, expect some performance degradation while the data is copied to the boot volume from the snapshot. By using [fast restore snapshots](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-use-fast-restore), you can achieve [recovery time objectives](#x3167918){: term} (RTO) quicker than by restoring from a regular snapshot, as all data is available and performance is not impacted. |
| Data volume failure or data corruption | [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore) in the console, from the CLI, with the API, Terraform, or by using a customer-managed continuous backup and restore solution. [Fast restore snapshots](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-use-fast-restore) are also available for data volumes, too.|
| File share data corruption | You can create file share snapshots to preserve data on your file share at a specific point in time. Then, you can [restore data from a file share snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore) if the contents of the file share get accidentally deleted or overwritten. |
| File share failure | Mitigated by initiating a failover to an existing replica in another zone. Test the failover process to see how long it takes. |
| Virtual server failure | Mitigated by using the high availability features of a load balancer and Auto Scale for VPC, creating a scalable workload. Virtual server restart might be required. Block volume failure resolution might be required. |
| Bare metal server failure | Customer-managed backup and restore solution. |
| Zone failure | Mitigated by using the high availability features of a load balancer and Auto Scale for VPC, creating a scalable workload. Use an external source of truth for the VPC zone configuration to create resources in an available zone by using server failure resolution and block volume failure resolution. |
| Regional failure | Use an external source of truth for VPC region configuration to create resources in an available region. Restore volumes and file storage to previous values by using server failure resolution. |
{: caption="DR scenarios for {{site.data.keyword.vpc_short}}" caption-side="bottom"}

## Your responsibilities for high availability and disaster recovery
{: #vpc-feature-responsibilities}



See [understanding your responsibilities when using Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc) for background. It is your responsibility to continuously test your plan for HA and DR. For more information, see [Disaster recovery testing](/docs/resiliency?topic=resiliency-dr-testing).

Interruptions in network connectivity and short periods of unavailability of a service might occur. It is your responsibility to make sure that application source code includes [client availability retry logic](/docs/resiliency?topic=resiliency-high-availability-design#client-retry-logic-for-ha) to maintain high availability of the application.
{: note}

You can use the following checklists to help you create and practice your plan.

- Block volume snapshot
   - [ ] Verify that the block volume snapshot is available in the restore location.
   - [ ] Consider including [Cross-regional snapshot copies](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots_vpc_crossregion_copy) as part of the DR strategy.
   - [ ] Consider creating a [backup plan](/docs/vpc?topic=vpc-backup-service-about) that can schedule the creation of the snapshots and remote copies.
   - [ ] Consider caching fast restore snapshots. For more information, see [Planning Block Storage for VPC snapshots](/docs/vpc?topic=vpc-snapshots-vpc-planning).

- File storage replication
   - [ ] Consider [cross-regional file storage replication](/docs/vpc?topic=vpc-file-storage-replication) as part of the DR strategy
   - [ ] Verify the configuration of your source and replica file shares for a failover scenario.

- External source of truth for VPC configuration
   - [ ] Verify that the source of truth is available in the restore location.
   - [ ] Verify that the VPC resources such as bare metal instances, virtual server instances, block storage volumes, and file storage are available in the restore location. For more information, see [Profile details](/docs/vpc?topic=vpc-accelerated-profile-family) for your virtual server profile family and [x86-64 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile).
   - [ ] Consider pre-provisioning resources to ensure availability.

## Change management
{: #vpc-change-management}



Change management includes tasks such as upgrades, configuration changes, and deletion.

Grant users and processes the IAM roles and actions with the least privilege that is required for their work. For more information, see [How can I prevent accidental deletion of services?](/docs/resiliency?topic=resiliency-dr-faq#prevent-accidental-deletion).
{: tip}



Consider creating a manual backup before making changes to infrastructure configurations.

## How {{site.data.keyword.IBM}} helps support disaster recovery planning
{: #vpc-ibm-disaster-recovery}

{{site.data.keyword.IBM}} takes specific recovery actions for {{site.data.keyword.vpc_short}} if a disaster occurs.

If a single host fails unexpectedly, virtual servers on the failed host can be automatically restarted on a healthy host. For more information about how IBM monitors infrastructure and responds to host failures, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies).

### How {{site.data.keyword.IBM_notm}} recovers from zone failures
{: #vpc-ibm-zone-failure}



Zone failures can result from natural disasters, infrastructure problems such as a power outage, accidental or malicious actions that delete information, or
software updates that contain a bug or error. In the event of a zone failure, IBM works to recover facilities and data centers, physical network and devices,
physical storage, physical servers and memory, and hypervisors. For more information, see [Shared responsibilities for using IBM Cloud products](/docs/overview?topic=overview-shared-responsibilities).


### How {{site.data.keyword.IBM_notm}} recovers from regional failures
{: #vpc-ibm-regional-failure}



In the event that an entire region experiences a failure, IBM again works to recover facilities and data centers, physical network and devices, physical storage,
physical servers and memory, and hypervisors. For more information, see [Shared responsibilities for using IBM Cloud products](/docs/overview?topic=overview-shared-responsibilities)
and [FAQs for disaster recovery](/docs/resiliency?topic=resiliency-dr-faq).



If {{site.data.keyword.IBM_notm}} can’t restore the service instance, you must restore the service as described in the [Disaster recovery architecture](#vpc-disaster-recovery-intro).

## How {{site.data.keyword.IBM_notm}} maintains services
{: #vpc-ibm-service-maintenance}



When routine maintenance is performed on virtual servers, hosts, and data centers, routine protocols are followed. For more information, see [Understanding cloud maintenance operations](/docs/vpc?topic=vpc-about-cloud-maintenance).

All upgrades follow {{site.data.keyword.IBM_notm}} service best practices, including recovery plans and rollback processes. Regular maintenance might cause short interruptions, mitigated by [client availability retry logic](/docs/resiliency?topic=resiliency-high-availability-design#client-retry-logic-for-ha). Changes are rolled out sequentially, region by region, and zone by zone within a region. {{site.data.keyword.IBM_notm}} reverts updates at the first sign of a defect.



Complex changes are enabled and disabled with feature flags to control exposure.



Changes that impact customer workloads are detailed in {{site.data.keyword.cloud_notm}} notifications. For more information about planned maintenance, announcements, and release notes that impact this service, see [Monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status).
