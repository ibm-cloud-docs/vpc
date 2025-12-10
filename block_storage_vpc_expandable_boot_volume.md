---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-10"

keywords: Block Storage, boot volume, data volume, volume, data storage, virtual server instance, instance, expandable volume

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Increasing capacity of a {{site.data.keyword.block_storage_is_short}} boot volume
{: #resize-boot-volumes}

You can increase the capacity in of your boot volumes up to 250 GB during and after instance provisioning in the console, from the CLI, with the API, or Terraform. For first-generation boot volumes, you can increase the capacity when the volumes are attached to a running virtual server instance. The capacity of second-generation boot volumes can be increased even if the volumes are not attached to a running instance. The steps for increasing the capacity are the same for all volume profiles.
{: shortdesc}

## Increase boot volume capacity in the console
{: #resize-vpc-boot-volumes-ui}
{: ui}

Increase boot volume capacity for new or existing instances in the console. For existing instances, you can increase the boot volume capacity by selecting a boot volume from the list of Block Storage volumes.

### Increase boot volume capacity during instance provisioning in the console
{: #resize-boot-vol-new-instance-ui}

When you create an instance from either a stock or custom image, you can increase the size of the boot volume. For example, a stock image would show 100 GB by default. You can modify the size up to 250 GB. For more information about creating virtual server instances, see [Creating virtual server instances in the console](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui).

You can also specify a larger boot volume capacity when you create an instance template. For more information, see [Creating an instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui#creating-instance-template).

### Increase boot volume capacity from the list of Block Storage volumes in the console
{: #resize-boot-vol-list-ui}

For an existing instance, you can increase its boot volume capacity by selecting it from the list of Block Storage volumes.

1. Go to the list of Block Storage volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.

2. Select a boot volume from the list of volumes. The attachment type is _boot_.

3. In the boot volume details, click the **Size** pencil icon. Alternatively, select **Expand volume** from the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions").

4. In the side panel, increase the boot volume size in the **Create size** field. The size must be more than the current size up to 250 GB.

5. Click **Expand boot volume size**.

## Increase boot volume capacity from the CLI
{: #expand-boot-vols-cli}
{: cli}

### Before you begin
{: #expand-boot-vols-cli-prereq}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

### Increase boot volume capacity when you create an instance from the CLI
{: #expand-new-boot-vols-cli}

Run the `ibmcloud is instance-create` command and specify a boot volume capacity in GBs.

The following example creates an instance with a boot volume of 190 GB.

```sh
ibmcloud is instance-create vsi-1 vpc-1 us-south-1 bx2-2x8  subnet-1 --image ibm-ubuntu-20-04-3-minimal-amd64-1 --boot-volume '{"name": "my-boot-vol-1", "volume": {"capacity": 190, "profile": {"name": "general-purpose"}}}'
```
{: pre}

```sh
Creating instance cli-vsi-1 under account Test Account as user test.user@ibm.com...

ID                                    0716_84f99419-554d-4c05-bea0-7034d1c40ed3
Name                                  vsi-1
CRN                                   crn:v1:bluemix:public:is:us-south-1:a/efe5afc483594adaa8325e2b4d1290df::instance:0716_84f99419-554d-4c05-bea0-7034d1c40ed3
Status                                pending
Availability policy on host failure   restart
Startable                             true
Profile                               bx2-2x8
Architecture                          amd64
vCPU Manufacturer                     Intel
vCPUs                                 2
Memory(GiB)                           8
Bandwidth(Mbps)                       4000
Volume bandwidth(Mbps)                1000
Network bandwidth(Mbps)               3000
Metadata service enabled              false
Image                                 ID                                          Name
                                      9f6b534b-6061-40f4-ac42-5aba4dd0da7f         ubuntu-20-04-3-minimal-amd64-1

VPC                                   ID                                          Name
                                      r006-42ebadb6-65f8-4b2f-923b-50b0e44670df   vpc-1

Zone                                  us-south-1
Resource group                        ID                                 Name
                                      11caaa983d9c4beb82690daab08717e9   Default

Created                               2022-02-24T16:43:47+05:30
Boot volume                           ID   Name           Attachment ID                               Attachment name
                                      -    PROVISIONING   0716-ee0ca315-7a21-42e2-99f7-b68377bbffe0   my-boot-vol1
```
{: screen}

You can also specify a larger boot volume capacity when you create an instance template from an image or snapshot. See the following example.

```sh
ibmcloud is instance template create tpl-1 vpc-1 us-south-1 bx2-2x8  cli-subnet-1 --image ubuntu-20-04-3-minimal-amd64-1 --boot-volume '{"name": "my-boot-vol1", "volume": {"capacity": 190, "profile": {"name": "general-purpose"}}}'
```
{: screen}

For more information about creating virtual server instances from the CLI, see [Creating virtual server instances from the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli). For more information about the commands that are used for increasing boot volume size, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli). 

### Increasing the capacity of an existing boot volume from the CLI
{: #expand-existing-boot-vol-cli}

From the CLI, locate the boot volume that you want to expand. You can use the `ibmcloud is volumes` command filter the results by specifying the resource group. Also, if you know the name or ID of the instance, you can view instance details and get information about the boot volume.

After you located the volume, use the `volume-update` command and provide the ID or name of the boot volume. Use the `--capacity` parameter to indicate the new size of the boot volume in GBs.

For example, this example increases the capacity of my-boot-vol1 to 200 GB. The existing capacity displays as the boot volume capacity is being expanded. Run the `ibmcloud is volume` command and specify the volume name to see the new capacity.

```sh
ibmcloud is volume update my-boot-vol-1 --capacity 200
```
{: pre}

```sh
Updating volume my-boot-vol1 under account VPC1 as user myuser@mycompany.com...

ID                                     9d60ba27-170c-4e2a-9bf6-6dbb11f95c38
Name                                   my-boot-vol1
CRN                                    crn:v1:bluemix:public:is:us-south-1:a/efe5afc483594adaa8325e2b4d1290df::volume:9d60ba27-170c-4e2a-9bf6-6dbb11f95c38
Status                                 updating
Capacity                               190
IOPS                                   3000
Bandwidth(Mbps)                        393
Profile                                general-purpose
Encryption key                         -
Encryption                             provider_managed
Resource group                         Default
Created                                2022-02-24T16:43:47+05:30
Zone                                   us-south-1
Volume Attachment Instance Reference   Attachment type   Instance ID                                 Instance name   Auto delete   Attachment ID                               Attachment name
                                       boot              0716_84f99419-554d-4c05-bea0-7034d1c40ed3   vsi-1           true          0716-ee0ca315-7a21-42e2-99f7-b68377bbffe0   boot-vol-name

Operating system                       ubuntu-20-04-amd64
Source image                           ID                                          Name
                                       9f6b534b-6061-40f4-ac42-5aba4dd0da7f        ubuntu-20-04-3-minimal-amd64-1

Active                                 true
Busy                                   false
Tags                                   -
```
{: screen}

## Increase boot volume capacity with the API
{: #increase-vpc-volumes-api}
{: api}

### Increase boot volume capacity when you create an instance with the API
{: #expand-new-boot-vol-api}

When you create an instance by making a `POST \instances` request, you can specify larger boot volume capacity for any of these contexts: when you create the instance from an image, a source boot volume, or an instance template. Specify a boot volume name and capacity in the `boot-volume-attachment` property. The capacity for the boot volume must be at least the image's minimum provisioned size, which is the default if you don't specify capacity.

The following example creates a virtual server instance from an image, with a boot volume that has 250 GB capacity.

```sh
curl -X POST "$vpc_api_endpoint/v1/instances?version=2022-02-01&generation=2"\
-H "Authorization: $iam_token"\
-d '{
      "boot_volume_attachment": {
         "volume": {
           "capacity": 250",
           "encryption_key": {
             "crn": "crn:[...]"
           },
           "name": "my-boot-volume",
           "profile": {"name": "general-purpose"}
         }
       },
      "image": {"id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366"},
       .
       .
       .
   }'
```
{: codeblock}

For more information, see [Create an instance](/apidocs/vpc#create-instance) in the VPC API reference.

### Increasing the capacity of an existing boot volume with the API
{: #expand-existing-boot-vol-api}

With the API, locate the boot volume that you want to expand by making a `GET \volumes` call. Then, make a `PATCH \volumes` call with the ID of the boot volume and specify a new value for capacity.

For example, this call increases the capacity of a boot volume to 250 GB.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/volumes/$volume_id/?version=2022-02-12&generation=2"\
-H "Authorization: $iam_token" \
-d '{
      "capacity": 250,
   }'
```
{: codeblock}

## Increasing the capacity of an existing boot volume with Terraform
{: #expand-existing-boot-vol-terraform}
{: terraform}

To increase the capacity of a boot volume, use the `ibm_is_volume` resource. When it's applied, the following example updates the capacity of the volume to 250 GB.

```terraform
resource "ibm_is_volume" "boot-volume-example" {
  name    = "my-boot-volume"
  size    = 250
  profile = "general-purpose"
  zone    = "us-south-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

## Modifying OS to use the increased capacity
{: #modifying-os-expanded-boot-volume}

After you expand the boot volume capacity, you have to make your OS recognize the capacity increase. You must independently grow the disk partition, and then increase the file system into the partition.

For more information about expanding the file system, see your OS Documentation. For example,
- [RHEL 9 - Modifying Logical Volume](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_logical_volumes/basic-logical-volume-management_configuring-and-managing-logical-volumes#resizing-logical-volumes_basic-logical-volume-management){: external}
- [Microsoft&reg; - Extend a basic volume](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume){: external}.

### Modifying the expanded boot volume in Linux
{: #modifying-the-linux-os-expanded-boot-volume}

The following example is based on CentOS Linux 7. Instructions for other Linux distributions can vary. After you increased the volume capacity from 100 GB to 250 GB, you can log in to the virtual server instance to validate the increase. Then, increase the partition and then expand the file system on the volume.

Extending a file system is a moderately risky operation. Consider taking a snapshot of the volume to prevent data loss.
{: tip} 

1. Establish an SSH connection to your virtual server instance by using the floating IP address that is assigned to the instance. For more information, see [Connecting to Linux instances](/docs/vpc?topic=vpc-vsi_is_connecting_linux).
1. Run the `lsblk` command to see the list of attached storage volumes. In the following example, `vda` is the expanded boot volume, and `vdc` is the attached {{site.data.keyword.block_storage_is_short}} data volume. The `vdb` disk is an instance storage volume. You can see that the partitions on the `vda` disk remained unchanged, although the overall size is increased to 250G.
   ```sh
   [root@docs-demo-instance ~]# lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   vda    253:0    0  250G  0 disk
   ├─vda1 253:1    0  200M  0 part /boot/efi
   └─vda2 253:2    0 99.8G  0 part /
   vdb    253:16   0 69.9G  0 disk
   vdc    253:32   0  1.2T  0 disk /myvolumedir
   vdd    253:48   0  370K  0 disk
   vde    253:64   0   44K  0 disk
   ```
   {: screen}

1. Issue the `growpart` command to grow the partition size to cover the maximum available space. 
   1. By using the `--dry-run` option, you can preview the changes before you perform the partition update.
      ```sh
      [root@docs-demo-instance ~]# growpart /dev/vda 1 --dry-run
       NOCHANGE: partition 1 is size 409600. it cannot be grown
      [root@docs-demo-instance ~]# growpart /dev/vda 2 --dry-run
       CHANGE: partition=2 start=411648 old: size=209303552 end=209715200 new: size=523876319 end=524287967
       # === old sfdisk -d ===
       # partition table of /dev/vda
        unit: sectors
       /dev/vda1 : start=     2048, size=   409600, Id=ef
       /dev/vda2 : start=   411648, size=209303552, Id=83, bootable
       /dev/vda3 : start=        0, size=        0, Id= 0
       /dev/vda4 : start=        0, size=        0, Id= 0
       # === new sfdisk -d ===
       # partition table of /dev/vda
       unit: sectors
       /dev/vda1 : start=     2048, size=   409600, Id=ef
       /dev/vda2 : start=   411648, size=523876319, Id=83, bootable
       /dev/vda3 : start=        0, size=        0, Id= 0
       /dev/vda4 : start=        0, size=        0, Id= 0
      ```
      {: screen}

   1. Update the partition size of the boot volume as shown in the following example.
      ```sh
      [root@docs-demo-instance ~]# growpart /dev/vda 2 
      CHANGED: partition=2 start=411648 old: size=209303552 end=209715200 new: size=523876319 end=524287967
      ```
      {: screen}

1. Issue the command `lsblk` to verify that the partition is resized. The following example shows that the `vda2` partition is successfully increased in size.
   ```sh
   [root@docs-demo-instance ~]# lsblk
   NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
   vda    253:0    0   250G  0 disk
   ├─vda1 253:1    0   200M  0 part /boot/efi
   └─vda2 253:2    0 249.8G  0 part /
   vdb    253:16   0  69.9G  0 disk
   vdc    253:32   0   1.2T  0 disk /myvolumedir
   vdd    253:48   0   370K  0 disk
   vde    253:64   0    44K  0 disk
   ```
   {: screen}

   However, the file system still sees the `vda2` partition as 99G instead of 249G.

   ```sh
   [root@docs-demo-instance ~]# df -kh
   Filesystem      Size  Used Avail Use% Mounted on
   devtmpfs        3.9G     0  3.9G   0% /dev
   tmpfs           3.9G     0  3.9G   0% /dev/shm
   tmpfs           3.9G  385M  3.5G  10% /run
   tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
   /dev/vda2        99G  1.3G   92G   2% /
   /dev/vda1       200M   12M  189M   6% /boot/efi
   /dev/vdc        1.2T   71M  1.2T   1% /myvolumedir
   tmpfs           783M     0  783M   0% /run/user/0
   ```
   {: screen}

1. Resize the file system on the partition with the `resize2fs` command.
   ```sh
   [root@docs-demo-instance ~]# resize2fs /dev/vda2
   resize2fs 1.42.9 (28-Dec-2013)
   Filesystem at /dev/vda2 is mounted on /; on-line resizing required
   old_desc_blocks = 13, new_desc_blocks = 32
   The filesystem on /dev/vda2 is now 65484539 blocks long.
   ```
   {: screen}

1. Verify that the file system is expanded. In the example, you can see that the size of `vda2` increased.
   
   ```sh
   [root@docs-demo-instance ~]# df -kh
   Filesystem      Size  Used Avail Use% Mounted on
   devtmpfs        3.9G     0  3.9G   0% /dev
   tmpfs           3.9G     0  3.9G   0% /dev/shm
   tmpfs           3.9G  385M  3.5G  10% /run
   tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
   /dev/vda2       246G  1.3G  234G   1% /
   /dev/vda1       200M   12M  189M   6% /boot/efi
   /dev/vdc        1.2T   71M  1.2T   1% /myvolumedir
   tmpfs           783M     0  783M   0% /run/user/0
   ```
   {: screen}

### Modifying the expanded boot volume in Windows
{: #modifying-the-windows-os-expanded-boot-volume}

You can extend your boot partition with the DiskPart utility from the Command Prompt or PowerShell as an administrator.

If you used a stock image of Windows Server 2022 Standard Edition or Windows Server 2025 Standard Edition to create your virtual server, you might find that the Recovery partition (D: drive) is preventing you from extending the C: drive. To resolve this issue, see the troubleshooting topic: [Why can't I expand my C: drive on Windows Server?](/docs/vpc?topic=vpc-troubleshooting-unexpandable-c-drive)


## Next steps
{: #next-steps-resize-boot-vols}

Create more volumes or manage your existing Block Storage volumes.

* [Creating Block Storage volumes](/docs/vpc?topic=vpc-creating-block-storage).
* [Managing Block Storage volumes](/docs/vpc?topic=vpc-managing-block-storage).

Optionally, [increase the capacity of your data volumes](/docs/vpc?topic=vpc-expanding-block-storage-volumes) that are attached to a virtual server instance.
