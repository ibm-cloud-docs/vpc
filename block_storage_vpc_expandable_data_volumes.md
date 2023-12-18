---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-18"

keywords: Block Storage, boot volume, data volume, volume, data storage, virtual server instance, instance, expandable volume

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Increasing capacity of a {{site.data.keyword.block_storage_is_short}} data volume
{: #expanding-block-storage-volumes}

After you provisioned a {{site.data.keyword.block_storage_is_short}} data volume and attached it to a virtual server instance, you can increase its volume size in the console, from the CLI, with the API or Terraform. 
{: shortdesc}

You can't change the volume to a smaller size after you expand its capacity. However, if your requirements change, you can expand the same volume again up to the maximum capacity that's available for its profile.

## Expand Block Storage volumes in the UI
{: #expand-vpc-volumes-ui}
{: ui}

Follow these steps to expand volume capacity:

1. Go to the list of Block Storage volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Storage > Block Storage volumes**. By default, Block Storage volumes display for all resource groups in your region.

2. In the list of all **Block Storage for VPC volumes**, click the name of the volume you want to expand to see the volume details.

   The volume that you select must be attached to a virtual server instance. In the list of volumes, its attachment type is _data_.
   {: note}

3. Alternatively, go to a virtual server instance with an attached volume that you want to expand and select it from the list of attached volumes.

4. On the volume details page, locate **Size**. The current IOPS profile and volume size are displayed.

5. Click the pencil icon. Alternatively, click the **Actions** menu and select **Expand Block Storage volume**.

6. In the panel, increase the volume size in GB up to 16,000 GB.

   The maximum size that you can expand to is based on the selected IOPS profile or custom volume settings. The UI indicates the maximum capacity for the selected profile. For a custom profile, you can expand the volume based on [sizing limits](#expandable-volume-limitations). When you increase the size of the volume, max IOPS and throughput are calculated for the expanded volume.

7. Review the estimated monthly order summary for your geography and new pricing.

8. If you're satisfied, click **Save and continue**. Your new Block Storage allocation is available in a few minutes.

## Expand Block Storage volumes from the CLI
{: #expand-vpc-volumes-cli}
{: cli}

### Before you begin
{: #expand-vpc-volumes-cli-prereq}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

2. Select the current generation of VPC. 
   ```sh
   ibmcloud is target --gen 2
   ```
   {: pre}

### Expand volume capacity from the CLI
{: #expand-vol-capacity-cli}

From the CLI, use the `ibmcloud is volume-update` command with the `--capacity` option to indicate the new size of the volume in GBs.

```sh
ibmcloud is volume-update VOLUME_ID --capacity CAPACITY_GB
```
{: pre}

The following example expands the capacity of a `general-purpose` volume to 8,000 MB.

```sh
$ ibmcloud is volume-update demo-volume-update --capacity 8000
Updating volume demo-volume-update under account Test Account as user test.user@ibm.com...

ID                                     r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac
Name                                   demo-volume-update   
CRN                                    crn:v1:bluemix:public:is:us-east-1:a/a123456::volume:r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac   
Status                                 updating   
Attachment state                       attached   
Capacity                               100
IOPS                                   3000
Bandwidth(Mbps)                        3145
Profile                                general-purpose
Encryption key                         -
Encryption                             provider_managed
Resource group                         defaults
Created                                2023-06-29T16:14:59+00:00
Zone                                   us-east-1
Health State                           ok
Volume Attachment Instance Reference   Attachment type   Instance ID                                 Instance name   Auto delete   Attachment ID                               Attachment name      
                                       data              0757_11f5db7f-35a1-4678-bcbd-c85204e09507   kj-test-ro      false         0757-4dfc4384-c4b5-497e-bab3-6415f9c4d44b   otp      

Active                                 true
Busy                                   false
Tags                                   -
```
{: screen}

When the update operation completes, run the `ibmcloud is volume` command to see the updated properties of the volume.

```sh
$ ibmcloud is volume demo-volume-update
Getting volume demo-volume-update under account Test Account as user test.user@ibm.com...
                                          
ID                                     r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac
Name                                   demo-volume-update
CRN                                    crn:v1:bluemix:public:is:us-east-1:a/a123456::volume:r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac
Status                                 available
Attachment state                       attached
Capacity                               8000   
IOPS                                   24000
Bandwidth(Mbps)                        3145
Profile                                general-purpose
Encryption key                         -
Encryption                             provider_managed
Resource group                         defaults
Created                                2023-06-29T16:14:59+00:00
Zone                                   us-east-1
Health State                           ok
Volume Attachment Instance Reference   Attachment type   Instance ID                                 Instance name   Auto delete   Attachment ID                               Attachment name      
                                       data              0757_11f5db7f-35a1-4678-bcbd-c85204e09507   kj-test-ro      false         0757-4dfc4384-c4b5-497e-bab3-6415f9c4d44b   otp      
                                          
Active                                 true
Busy                                   false
Tags                                   -
```
{: codeblock}

For more information about available command options, see [`ibmcloud is volume-update`](/docs/cli?topic=cli-vpc-reference#volume-update).

## Expand Block Storage volumes with the API
{: #expand-vpc-volumes-api}
{: api}

You can expand existing data volumes by calling the Virtual Private Cloud (VPC) API. Make a `PATCH /volumes` request to increase the capacity of a volume that is attached to an instance.

You can't update the name of the volume and expand capacity in the same `PATCH /volumes` request. Make two separate `PATCH/volumes` requests.
{: note}

This example call expands a volume with a capacity of 50 GB to 250 GB.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-02-25&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "capacity": 250
    }'
```
{: codeblock}

The volume status shows `updating` while the volume is being expanded. The current capacity is shown.

```json
{
	"capacity": 50,
	"created_at": "2022-02-25T09:46:43.000Z",
	"crn": "crn:v1:bluemix:public:is:us-south-1:a/<Acc id>::volume:<Volume ID>",
    .
    .
    .
	"status": "updating",
    .
    .
    .
}
```
{: screen}

When the volume expansion completes, the new value displays, and the volume status is `available`.

```json
{
	"capacity": 250,
	"created_at": "2022-02-25T09:46:43.000Z",
	"crn": "crn:[...]",
	"encryption": "provider_managed",
	"href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
	"id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
	"IOPS": 100,
	"name": "my-volume-1",
	"profile": {
		"href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
		"name": "general-purpose"
	},
	"resource_group": {
		"href": "https://resource-manager.bluemix.net/v1/resource_groups/83daf012-5920-4ba9-9689-cc0d2d2281fb",
		"id": "83daf012-5920-4ba9-9689-cc0d2d2281fb",
		"name": "Default"
	},
	"status": "available",
	"volume_attachments": [{
		"delete_volume_on_instance_delete": true,
		"device": {
			"id": "4cbb38bc-57d5-4121-a796-d5b10cf0810a"
		},
		"href": "https://us-south.iaas.cloud.ibm.com/v1/instances/8f06378c-ed0e-481e-b98c-9a6dfbee1ed5/volume_attachments/4cbb38bc-57d5-4121-a796-d5b10cf0810a",
		"id": "<4cbb38bc-57d5-4121-a796-d5b10cf0810aAttachment ID>",
		"instance": {
			"crn": "crn:[...]",
			"href": "https://us-south.iaas.cloud.ibm.com/v1/instances/8f06378c-ed0e-481e-b98c-9a6dfbee1ed5",
			"id": "8f06378c-ed0e-481e-b98c-9a6dfbee1ed5",
			"name": "my-instance-1"
		},
		"name": "my-volume-attachment-1",
		"type": "data"
	}],
	"zone": {
		"href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
		"name": "us-south-2"
	}
}
```
{: screen}

## Expand Block Storage volumes with Terraform
{: #expand-vpc-volumes-terraform}
{: terraform}

To increase the capacity of a volume, use the `ibm_is_volume` resource. When applied, the following example updates the capacity to 8000 GB.

```terraform
resource "ibm_is_volume" "storage" {
  name    = "demo-volume-update"
  size    = 8000
  profile = "general-purpose"
  zone    = "us-south-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

## Expand the file system
{: #next-step-expandable-filesystem}

The volume expansion takes effect without a restart. However, to use the increased volume space, you must expand the file system so the increased volume capacity is recognized.

For more information about expanding the file system, see your OS Documentation. For example, [RHEL 8 - Modifying Logical Volume](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/modifying-the-size-of-a-logical-volume_configuring-and-managing-logical-volumes){: external} or [Microsoft&reg; - Extend a basic volume](https://docs.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume){: external}.

The following example is based on CentOS Linux 7. After you increased the volume capacity from 600 GB to 700 GB, you can log in to the virtual server instance to validate the increase. Then, increase the file system on the volume.

Extending a file system is a moderately risky operation. Consider taking a snapshot of the volume to prevent data loss.
{: tip} 

1. Establish the SSH connection to your virtual server instance by using the floating IP address that is assigned to the instance. For more information, see [Connecting to Linux instances](/docs/vpc?topic=vpc-vsi_is_connecting_linux).
1. Run the `lsblk` command to see the updated capacity. In the following example, `vdc` is the attached Block Storage volume.
   ```sh
   [root@docs-demo-instance ~]# lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   vda    253:0    0  100G  0 disk
   ├─vda1 253:1    0  200M  0 part /boot/efi
   └─vda2 253:2    0 99.8G  0 part /
   vdb    253:16   0 69.9G  0 disk
   vdc    253:32   0  700G  0 disk /myvolumedir
   vdd    253:48   0  370K  0 disk
   vde    253:64   0   44K  0 disk
   ```
   {: screen}

1. The volume is resized to 700G, but the file system still shows the previous size, 619140256 blocks.
   ```sh
   [root@docs-demo-instance ~]# df -hk
   Filesystem     1K-blocks    Used Available Use% Mounted on
   devtmpfs         3993976       0   3993976   0% /dev
   tmpfs            4004356       0   4004356   0% /dev/shm
   tmpfs            4004356   25092   3979264   1% /run
   tmpfs            4004356       0   4004356   0% /sys/fs/cgroup
   /dev/vda2      102877120 1178920  96449228   2% /
   /dev/vda1         204580   11468    193112   6% /boot/efi
   tmpfs             800872       0    800872   0% /run/user/0
   /dev/vdc       619140256   73752 587592840   1% /myvolumedir
   ```
   {: screen}

1. Run the `resize2fs`command to increase the file system.
   ```sh 
   [root@docs-demo-instance ~]# resize2fs /dev/vdc
   resize2fs 1.42.9 (28-Dec-2013)
   Filesystem at /dev/vdc is mounted on /myvolumedir; on-line resizing required
   old_desc_blocks = 75, new_desc_blocks = 88
   The filesystem on /dev/vdc is now 183500800 blocks long.
   ```
   {: screen}

   If the command returns `pvresize: command not found`, install the logical volume manager by running the command `yum install lvm2`.
   {: tip}

1. Confirm the new file system size. The example shows 722352120 blocks.
   ```sh
   [root@docs-demo-instance ~]# df -hk
   Filesystem     1K-blocks    Used Available Use% Mounted on
   devtmpfs         3993976       0   3993976   0% /dev
   tmpfs            4004356       0   4004356   0% /dev/shm
   tmpfs            4004356   25092   3979264   1% /run
   tmpfs            4004356       0   4004356   0% /sys/fs/cgroup
   /dev/vda2      102877120 1178920  96449228   2% /
   /dev/vda1         204580   11468    193112   6% /boot/efi
   tmpfs             800872       0    800872   0% /run/user/0
   /dev/vdc       722352120   72816 686590468   1% /myvolumedir
   ```
   {: screen}   
