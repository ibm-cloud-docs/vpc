---

copyright:
  years: 2020, 2023
lastupdated: "2023-08-25"

keywords: instance storage, local disk, storage, temporary storage, generation 2, gen 2

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
{:beta: .beta}
{:table: .aria-labeledby="caption"}

# About instance storage
{: #instance-storage}

Instance storage is a set of one or more solid-state drives (full or isolated partial storage spaces) attached to your virtual server instance when the instance is provisioned. Instance storage is close to the compute resources of the virtual server and on a high-speed communication channel independent from the network. An instance storage disk provides fast, affordable, temporary storage to improve the performance of cloud native workloads with scratch space, caching buffers, or a place for replicated data. The data that is stored on instance storage is ephemeral, which means that the data is tied directly to the instance lifecycle. The instance storage disk is automatically created and destroyed with the instance. Instance storage data is not lost when an instance is only rebooted.
{: shortdesc}

Instance storage is a complementary storage technology to the boot and block storage volumes for data offered with VPC. Some examples of use cases for instance storage disks are:

*	Distributed File Systems: Technologies such as Hadoop Distributed File System (HDFS), which do triplication of the data across multiple servers. These technologies can improve read bandwidth and retain reliability by maintaining multiple copies of the data. It is recommended that at least three copies of the data, ideally across availability zones, are maintained when using Instance Storage for these workloads.
*	Transactional jobs: Transaction processing usually creates a significant amount of temporary files. Instance Storage is a great place to temporarily store that data while the transactions are processed, with the result stored persistently on a data  volume.
*	Content Delivery Networks (CDNs): Improve performance for large geographically distributed applications with instance storage for fast, low cost, local data caching.

## Lifecycle of instance storage
{: #instance-storage-lifecycle}

The lifecycle of instance storage is critical to understand. Without a proper understanding of its temporary nature, workloads could experience data loss.
{: important}

The instance storage space is allocated and attached to the virtual server instance at the time of provisioning. It is an integral and inseparable element of the virtual server instance. If you reboot your instance (either from its operating system or through an IBM Cloud Management interface), the instance storage data is retained and the instance is reconnected to its original disks.

However, when an instance is stopped or shut down, the instance storage attached to the virtual server instance is cryptographically erased and the instance storage is no longer available. The instance's boot volume is not affected. For auditing purposes, when an instance that has one or more storage disks is shut down, an IBM Cloud Activity Tracker event is published for each disk indicating it was "wiped". The event action is `is.instance.disk.wipe`. Refer to the Compute Events table on the [Activity Tracker page](/docs/vpc?topic=vpc-at-events#events-compute) for listing of such events.

When a virtual server is powered off and then powered back on in order to complete a host or data center maintenance, the instance storage ephemeral data is not restored. However, advanced notice is given for maintenance windows. See [Understanding cloud maintenance operations](/docs/vpc?topic=vpc-about-cloud-maintenance) for more information.

In the event of a Host Failure, the data on the instance storage disk is lost. The instance is restarted on another host automatically unless `Host failure auto restart` is set to stop. The provisioned disks are now empty.

In rare cases, a physical disk error might occur. If a physical disk error occurs, the instance fails to read/write to the failed disk. The instance is not automatically restarted in this scenario. Contact your support representative to report the problem and identify next steps. You can stop and then start your instance to effectively place it on a different disk that is functional. If you stop and then start your instance in order to place it on a different disk, this scenario does erase your instance storage data.

Virtual servers that are deployed with multiple instance storage disks are spread across multiple physical disks. No two instance storage disks within a single virtual server instance lands on the same physical backing disk.

Block storage volumes and instance storage are complementary technologies. Volumes can be attached to instances that have instance storage. In fact, instances that use instance storage generally utilize block storage volumes for the persistent storage requirements of the workload.

## Encryption and isolation
{: #encryption-isolation}

Instance storage data is secured with on-disk encryption. The physical disks used for instance storage are self-encrypting with the strong AES-256 encryption standard. The data is automatically decrypted when your instance accesses the data. When your instance is shut down or deleted, the underlying storage space is erased and unrecoverable. At that point, the data is unrecoverable. 

Data is automatically encrypted on the physical media at the drive level. However, customer-managed keys are not supported for instance storage. For sensitive data, it is strongly recommended that users utilize software-based file system encryption such as LUKS for Linux&reg; or BitLocker for Windows&reg;. This technology allows end users to encrypt entirely within the instance, and can provide additional protection for sensitive data in-transit between the instances and the physical drive media. Some operating systems also provide FIPS certified encryption algorithms that may also be used. See [Encrypting block devices using LUKS](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/security_hardening/encrypting-block-devices-using-luks_security-hardening){: external} for an example of how to encrypt on Red Hat Enterprise Linux&reg; however, refer to the Operating System documentation or specific information on how to encrypt each device.

## Isolation
{: #isolation-instance-storage}

The instance storage disk, or disks attached to the virtual server instance, cannot be shared with any other virtual servers and cannot be accessed by any other virtual servers in the future. They are one-time use, single-attach, for the virtual server that requested the instance storage. There is also isolation from negative performance effects of different virtual servers doing I/O to different portions of the same physical device (as described in the next section). 

## Performance
{: #performance-instance-storage}

Instance storage is rate-limited. If the instance storage allocated to the virtual server instance is smaller than the entire physical Solid State Disk (SSD), then I/O bandwidth and IOPS limits are put in place proportional to the size of the attached disk. This limitation is in place to prevent a busy instance from consuming more than their fair share of the bandwidth of the device (noisy neighbor).

For [Gen 3 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles), write performance may be improved by first priming the instance storage disk. Priming involves performing a first write to the disk.

## Multiple instance storage disks
{: #multiple-instance-storage-disks}

When there are multiple instance disks on your virtual server instance, each disk is provisioned from a separate underlying physical device. This allows for better independent performance as well as software high-availability solutions to be implemented like software-based RAID.

## Billing
{: #billing-instance-storage}

You are billed for instance storage only while the virtual server instance is running.

## Restrictions
{: #restrictions-instance-storage}

Using instance storage comes with some restrictions. Keep the following in mind when you are considering instance storage:
*	You cannot attach or detach the instance storage disk from an existing virtual server instance.
*	The instance storage disk cannot be used as a boot disk.
*	Bring your own key (BYOK) is not supported for instance storage.

To prevent loss of critical data, review the data retention policy and ensure that critical data is stored on persistent storage. For more information, see [Lifecycle of instance storage](#instance-storage-lifecycle).
{: tip}

For information about provisioning a virtual server instance with instance storage, see [Managing instance storage](/docs/vpc?topic=vpc-instance-storage-provisioning).
