---

copyright:
  years: 2021
lastupdated: "2021-09-22"

keywords: instance status message, instance status, VPC error message, error message

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

#  Virtual server deployment error messages
{: #instance-status-messages}

If you receive an error message, you can use the following information to help resolve the problem.
{: shortdesc}

## Error: `cannot_start`
{: #cannot-start-instance}

**Error message**: _Can't start instance because provisioning failed_

Your virtual server didn't start because the provisioning process failed. To address this error, try the follow possible resolutions: 

* Start the instance again.
* Stop and restart the instance.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help).

## Error: `cannot_start_capacity`
{: #cannot-start-capacity}

**Error message**: _Can't start instance because resource capacity is unavailable._

Your virtual server didn't start because the instance doesn't have enough available resource capacity to start. To address this error, try the follow possible resolutions: 

* Start the instance again.
* Stop and restart the instance.
* Provision a smaller instance profile with fewer available resources.
* Try a different availability zone or location.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help).

## Error: `cannot_start_placement_group`
{: #cannot-start-placement-group}

**Error message**: _Can't start instance in the placement group_

Your virtual server didn't start because the instance doesn't have enough available resource capacity in the instance group to start. To address this error, try the follow possible resolutions: 

* Deploy the placement group again.
* Distribute the instances across availability zones.
* Provision a smaller instance profile.
* Provision again with fewer instances.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help).

## Error: `cannot_start_compute`
{: #cannot-start-compute}

**Error message**: _Can't start instance because provisioning failed_

Your virtual server did not start because of an unexpected failure. To address this error, try the follow possible resolutions: 

* Start the instance again.
* Stop and restart the instance.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help).

## Error: `cannot_start_network`
{: #cannot-start-network}

**Error message**: _Can't start instance because network resources can't be provisioned_

Your virtual server did not start because of a network resource failure. To address this error, try the follow possible resolutions: 

* Start the instance again.
* Stop and restart the instance.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help).

## Error: `cannot_start_ip_address`
{: #cannot-start-ip-address}

**Error message**: _Can't start instance because the network interface IP address isn't available_

Your virtual server did not start because of an unexpected network interface IP address failure. To address this error, try the follow possible resolutions: 

* Start the instance again.
* Stop and restart the instance.
* Delete the instance and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help).

## Error: `cannot_start_storage`
{: #cannot-start-storage}

**Error message**: _Can't start instance because storage wasn't provisioned_

Your virtual server did not start because of a storage volume or storage disk provisioning error. To address this error, try the follow possible resolutions: 

* Start the instance again.
* Stop and restart the instance.
* Delete the instance and try again.
* Check your storage resources and try again.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help).

## Error: `encryption_key_deleted`
{: #encryption-key-deleted}

**Error message**: _Instance can't start because the encryption key for the boot volume was deleted_

The virtual server instance is unusable because the encryption key for the boot volume was deleted.

To resolve this error, restore the encryption key and try again. For more information about restoring your encryption key, see [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys&interface=ui).

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help).

## Error: `stopped_for_image_creation`
{: #stopped-for-image-creation}

**Error message**: _Instance was stopped because a new image was created_

The virtual server instance can't start instance because the image is being created from boot volume. 

To resolve this error, try to start the instance again after the image creation process is complete.

If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help).
