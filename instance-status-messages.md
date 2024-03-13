---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-10"

keywords: instance status message, instance status, VPC error message, error message, virtual server error

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Virtual server instance deployment error messages
{: #instance-status-messages}

If you receive an error message, you can use the following information to help resolve the problem.
{: shortdesc}

## Error: `cannot_start`
{: #cannot-start-instance}

**Error message**: _Can't start instance because provisioning failed_

Your virtual server instance didn't start because of an unexpected error. To address this error, try the follow possible resolutions:

* Stop and restart the instance.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For more information about managing your instance, see [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances).

## Error: `cannot_start_capacity`
{: #cannot-start-capacity}

**Error message**: _Can't start instance because resource capacity is unavailable._

Your virtual server instance didn't start because the instance doesn't have enough available resource capacity to start. To address this error, try the follow possible resolutions:

* Stop and restart the instance.
* Select a different availability zone.
* Provision a smaller instance profile that requires fewer resources.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For more information about managing your instance, see [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances).

## Error: `cannot_start_placement_group`
{: #cannot-start-placement-group}

**Error message**: _Can't start instance in the placement group_

Your virtual server instance didn't start because the instance doesn't have enough available resource capacity in the instance group to start. To address this error, try the follow possible resolutions:

* Stop and restart the instance.
* Resize to a smaller instance profile.
* Delete the failed instance and create an instance in another availability zone.
* Remove the instance from the placement group and restart the instance.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For more information about managing your virtual server instance and placement groups, see the following topics:
* [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances)
* [Managing placement groups](/docs/vpc?topic=vpc-managing-placement-group)

## Error: `cannot_start_compute`
{: #cannot-start-compute}

**Error message**: _Can't start instance because provisioning failed_

Your virtual server instance did not start because of an unexpected failure. To address this error, try the follow possible resolutions:

* Stop and restart the instance.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For more information, see [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances)

## Error: `cannot_start_network`
{: #cannot-start-network}

**Error message**: _Can't start instance because network resources can't be provisioned_

Your virtual server instance did not start because of a network resource failure. To address this error, try the follow possible resolutions:

* Stop and restart the instance.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For more information about managing network interfaces, see [Managing network interfaces](/docs/vpc?topic=vpc-using-instance-vnics)

## Error: `cannot_start_ip_address`
{: #cannot-start-ip-address}

**Error message**: _Can't start instance because the network interface IP address isn't available_

Your virtual server instance did not start because of an unexpected network interface IP address failure. To address this error, try the follow possible resolutions:

* Stop and restart the instance.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For more information about managing your instance and IP addresses, see the following topics:
* [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances)
* [Managing network interfaces](/docs/vpc?topic=vpc-using-instance-vnics)

## Error: `cannot_start_storage`
{: #cannot-start-storage}

**Error message**: _Can't start instance because storage wasn't provisioned_

Your virtual server instance did not start because of a storage volume or storage disk provisioning error. To address this error, try the follow possible resolutions:

* Stop and restart the instance.
* Delete the instance and try again.
* Check your storage resources and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For more information about storage, see the following topics:
* [About file storage](/docs/vpc?topic=vpc-file-storage-vpc-about)
* [About instance storage](/docs/vpc?topic=vpc-instance-storage)

## Error: `encryption_key_deleted`
{: #encryption-key-deleted}

**Error message**: _Instance can't start because the encryption key for the boot volume was deleted_

The virtual server instance is unusable because the encryption key for the boot volume was deleted.

To resolve this error, restore the encryption key and try again. For more information about restoring your encryption key, see [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys&interface=ui).

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

## Error: `stopped_for_image_creation`
{: #stopped-for-image-creation}

**Error message**: _Instance was stopped because a new image was created_

The virtual server instance can't start instance because the image is being created from boot volume.

To resolve this error, try to start the instance again after the image creation process is complete.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For more information about creating images, see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers) or [Creating virtual server instances by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli).

## Error: `stopped_by_host_failure`
{: #stopped-by-host-failure}

**Error message**: _Host not responding: instance stopped_

The virtual server instance stopped because the host the instance is on has failed. The host failure policy for automatic restart is disabled and the instance has remained stopped.

To address this error, try the follow possible resolutions:

Stop and restart the instance.
Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For morre information about unexpected host failures, see [Host Failure Recovery Policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=ui).
