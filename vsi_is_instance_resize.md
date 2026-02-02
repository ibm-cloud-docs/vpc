---

copyright:
  years: 2021, 2026
lastupdated: "2026-02-02"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Resizing a virtual server instance
{: #resizing-an-instance}

You can resize your virtual server instance and vertically scale to any supported profile size in minutes. You can increase or decrease the amount of available vCPU and RAM for greater flexibility in workload management to address resource requirement changes and optimize cost or workload performance.
{: shortdesc}

Virtual servers are configured by using profiles, or a combination of instance attributes, such as the number of vCPUs, amount of RAM, network bandwidth, and more that define the size and capabilities of the virtual server instance.
When you upgrade or downgrade an existing server, you choose another profile that has the pre-defined specifications that you need. You cannot customize the configuration of a virtual server. The virtual server profile that you select determines the valid cores, RAM, bandwidth, and disk sizes on the resized instance. For more information about profiles, see [Instance Profiles](/docs/vpc?topic=vpc-profiles).

When you resize an instance, keep the following information in mind:

* You need to stop, update, and start the instance that you want to resize.
* Data isn't deleted from the primary volume or the data volume.
* RAM is wiped from the resized instance.
* All network configurations are maintained, such as private IPs, floating IPs, vNICs, and security groups.
* The instance name doesn't change.
* The location doesn't change. The location includes the location geography, region, and zone that were used when you create the virtual server instance.
* You must select a secure execution-enabled profile when you want to resize an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance. Selecting a profile that is not secure execution-enabled causes the provisioning to fail.
* After the instance is resized, you are billed the hourly rate of the new instance profile.
* You can track the instance resize in {{site.data.keyword.atracker_full_notm}} and {{site.data.keyword.logs_full_notm}} for troubleshooting and audit purposes.

## Resizing virtual servers on dedicated hosts
{: #resizing-dedicated-virtual-servers}

Virtual servers that run on dedicated hosts can be resized only to profiles that are supported by the dedicated host that the instance is hosted on. For example, a virtual server that is provisioned with a profile from the Memory family can resize to other profiles that belong to the Memory family.

 Resizing virtual servers on dedicated hosts is not supported for LinuxONE (s390x processor architecture).
{: note}

## Resizing virtual servers with instance storage
{: #resizing-with-instance-storage}

When you stop a virtual server instance with an instance storage profile, that storage is temporary storage and is available only while your virtual server is running. Data on the drive is unrecovered after the instance stops.

## Resizing virtual servers with data volumes
{: #resizing-with-data-volumes}

Attached data volumes remain intact and are attached in the resized instance.

When you resize an instance to a smaller profile (one with fewer vCPUs), it might be necessary to adjust the instance’s storage bandwidth allocation. To successfully resize, the instance’s storage bandwidth must be at least 500 MBps less than the preallocated bandwidth in the target profile.

For example,

   * Current profile: mx2-8x64 (total bandwidth: 16000 MBps, 12000 MBps network, 4000 MBps storage)
    (Assume that the instance is using default network and storage bandwidth allocation.)
   * Target profile: bx2-2x8 (total bandwidth: 4000 MBps, 3000 MBps network, 1000 MBps storage)

   The resize operation fails because the current amount of storage bandwidth (4000 MBps) is not at least 500 MBps less than the target profile’s total bandwidth (4000 MBps). To successfully resize, you must adjust the instance’s storage bandwidth amount to 3500 MBps or less before you attempt the resize operation.

## Resizing instances that are associated with instance templates and instance groups
{: #resizing-instance-templates-groups}

When you resize an instance that was provisioned from an instance template or that was provisioned as part of an instance group, the following rules apply.

* An instance that is provisioned from an instance template can be resized with a new instance profile.
* Instance templates are not editable, except for the name. You cannot update an instance profile within an instance template. To choose a different profile for an instance template, you must create a new template.
* Resizing an instance that is part of an instance group removes it from the instance group. An instance must be stopped to resize it. When the instance is stopped, the instance group replaces it with a new instance with the same profile as the instance template describes.

## Resizing instances between Gen 2 and Gen 3 profiles
{: #resizing-instance-generations}

You can resize a 2nd generation profile to a 3rd generation profile. A 3rd generation profile can be resized to a 2nd generation profile. Before you resize between profile generations, review the following information.

* Before you resize an instance with a 2nd generation profile to a 3rd generation profile, take a [snapshot](/docs/vpc?topic=vpc-snapshots-vpc-create) of the boot volume that is attached to your virtual server instance. You can refer to the snapshot if needed.
* When you resize an instance to a 3rd generation profile, the virtual firmware defaults to Open Virtual Machine Firmware (OVMF) if your image supports UEFI. If the instance that was previously booted with SeaBIOS, the system attempts to preserve the firmware setting, even when if you move to the latest generation. If the virtual firmware changes from SeaBIOS to OVMF during the migration to the new profile, the device names might appear differently in the guest.
* If you resize from a 3rd generation profile to a 2nd generation profile, any changes that are made to your virtual server instance while it runs with the 3rd generation profile are preserved. If you have the snapshot available that you took before you migrated to the new profile, you can restore from that snapshot if something goes wrong.

When you deploy a new Windows virtual server instance with a 3rd generation profile, avoid resizing to a 2nd generation profile. The new Windows instance uses OVMF virtual firmware and cannot be resized to a 2nd generation profile because the instance can't boot. If the Windows virtual server instance was originally provisioned with a 2nd generation profile, then resized to a 3rd generation profile, it can be resized back to a 2nd generation profile successfully.
{: note}

## Resizing a virtual server instance by using the UI
{: #resizing-a-virtual-server-UI}
{: ui}

Complete the following steps to resize an existing virtual server instance.

1. From the **IBM Cloud Console** menu, select **Virtual server instances**.
2. From the **Virtual server instances for VPC** list, find the virtual server that you want to resize and verify that its status is Stopped or Stopping.
3. Select the vertical ellipsis, and select **Resize**.
4. From the list of available profiles, select the profile that you want to use.
    * If you are resizing a virtual server that is running on a dedicated host, you see only profiles that the dedicated host supports.
    * If you are resizing a {{site.data.keyword.hpvs}} for VPC instance, make sure that you select a secure execution-enabled profile. Similarly, don't select a secure execution-enabled profile for an instance that doesn't support secure execution.
5. Review and check the Terms and Conditions.
6. Select **Resize virtual server instance**.
7. Start the virtual server instance.

## Resizing a virtual server by using the CLI
{: #resizing-a-virtual-server-CLI}
{: cli}

Use the `instance-update` command to resize a virtual server.

```sh
ibmcloud is instance-update <instance> --profile <profile>
```
{: pre}

Where:
* `instance` is the ID or Name of the instance that you want to resize
* `profile` is the Name of the profile that you want to use

As an example, if you want to resize an instance to the _bx2-16x64_ profile, the command would look similar to the following example.

```sh
ibmcloud is instance-update 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --profile bx2-16x64
```
{: pre}

## Resizing a virtual server by using the API
{: #resizing-a-virtual-server-API}
{: api}

Use the `instance-update` command to resize a virtual server.

1. Run the following command to find the name of the profile that you want to use:
   ```sh
   curl  -s -X GET "<api_endpoint>/v1/instance/profiles?generation=2&version=2021-02-01" -H "Authorization: Bearer <IAM token>"
   ```
   {: pre}

2. Select a compatible profile for your instance.
    * For a virtual server that is running on a dedicated host, choose a profile that the dedicated host supports.
    * If you use instance storage, choose a profile that has instance storage.
    * For data volumes, choose a profile that has data volumes.

3. Run the following command:
   ```sh
   curl -k -sS -X PATCH "<api_endpoint>/v1/instances/<instance id>?generation=2&version=2021-02-01" \
       -H "Authorization: Bearer <IAM token>" \
       -d '
   {
       "profile": {
          "name": "<new profile>"
       }
   } '
   ```
   {: pre}

   Where:
      * `instance-id` is the ID of the instance that you want to resize
      * `profile-id` is the ID of the profile that you want to use

## Resize a virtual server and reservations
{: #resize-virtual-server-reservation}

Keep the following information in mind when you resize a virtual server that's in a reservation. For more information about reservations, see [About Reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc).

If the instance that you want to resize is attached to a reservation, the instance profile can't update if the profile doesn't match the profile of its associated reservation.

If the instance that you want to resize is a new profile, you need to first detach the instance from the reservation. The instance profile can then be updated and then a new reservation attached to the instance with a matching profile.
