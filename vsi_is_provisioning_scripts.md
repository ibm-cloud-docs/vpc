---

copyright:
  years: 2019, 2025
lastupdated: "2025-05-28"

keywords: user data, virtual server username

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# User data
{: #user-data}



When you create an {{site.data.keyword.vsi_is_full}} instance, you can specify optional user data that automatically performs common configuration tasks or runs scripts. For more information about user data and usernames, see [IAM access](/docs/account?topic=account-userroles).
{: shortdesc}

{{site.data.keyword.vpc_short}} uses Cloud-init technology to configure virtual server instances. The **User Data** field on the *New virtual server for VPC* page allows users to put in custom configuration options by using cloud-init. Cloud-init supports several formats for configuration data, including yaml in a cloud-config file.

You can specify cloud-config data directly in the **User Data** field, or you can include the cloud-config data in a text file and specify the file name when you create your instance. For example, if you save the cloud-config data in `userdata.blob`, specify `-user-data @userdata.blob` when you create an instance by using the CLI.

When you create a virtual server instance by using the IBM Hyper Protect Container Runtime image, you must pass a contract as part of the **User Data** field. Since the image is a locked down image with SSH access disabled, the only way to interact with the instance is by passing the contract.

The size limit of the **User Data** field (or file) is 64 K bytes.
{: tip}

## User data examples for Linux
{: #user-data-examples-for-linux}

The default user account for Linux depends on the operating system. To see the list of operating systems and their corresponding default user accounts, see [Connecting to Linux instances: Determining the default user account](/docs/vpc?topic=vpc-vsi_is_connecting_linux&interface=ui#determining-default-user-account).

### Adding a user and SSH key
{: #add-user-ssh-key}

The following cloud-init example shows how a Linux user can add a user and provide the user with an authorized SSH key. The **Name** field has the public key that is added to `~/.ssh/authorized_keys`.


{{_include-segments/linux-ed25519-ssh-key-type-note.md}}

```yaml
#cloud-config
users:
  - name: demouser
    gecos: Demo User
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: users, admin
    ssh_import_id: None
    lock_passwd: true
    ssh_authorized_keys:
        - <ssh public key>
```
{: codeblock}

The following shell script example shows how a Linux user can add an SSH key for the current user.

```sh
#!/bin/bash
echo <sshKey> > ~/.ssh/authorized_keys
```
{: codeblock}

You can paste one of these examples directly into the **User Data** field. The user data is then available to the virtual server instance during provisioning.

If you specify to include a file and have spaces that precede the file name, the data isn't interpreted correctly. Verify that `#!/bin/sh` or `#!/bin/bash` are the first characters on the line that immediately follows the end of file designation (`<<EOF`). The characters can't be indented.
{: tip}

For more Linux user data examples and information, see [Cloud config examples)](https://cloudinit.readthedocs.io/en/latest/reference/examples.html){: external}.

### Configuring a single disk instance storage by using cloud-config script
{: #configure-instance-storage-cloud-config}

Instance storage is a feature of VPC where you can request virtual server instances with fast, local storage attached. For more information about instance storage, refer to [About instance storage](/docs/vpc?topic=vpc-instance-storage).

By default, when you provision a virtual server instance with instance storage and then log in to that server for the first time, the instance storage disks are not configured. They show up as block devices (for example `/dev/vdb`), and need the following done before you can use them with a file system:

* Partition the device.
* Format the partition with a file system.
* Mount the file system.

These activities can also be performed automatically when you use the **User Data** field to provision the virtual instance. The `cloud-config` script defines these actions:

* Instructs the cloud initialization process to run during the first boot of your virtual instance.
* Automatically partitions the device.
* Formats the partition with an `ext4` file system.
* Mounts the file system.

Do not specify the **User Data** field to automatically configure an instance storage disk that is not part of the virtual instance definition. Attempting to automatically configure an instance storage disk that is not part of the virtual instance definition would target `dev/vdb device` and might point to the cloud-init data source, which would corrupt the cloud-init configurations such as hostname and ssh keys.
{: important}

The following example shows user data that automatically configures an instance storage disk. This example can be used with an instance profile that specifies a single disk.

```yaml
#cloud-config
# Cloud-init supports simple partition and file system config.
# This user data yaml will create a full partition on the first
# virtio_blk device after the boot device, initialize ext4 on it and
# mount it on a folder matching the label.
#
disk_setup:
  /dev/vdb:
    table_type: 'mbr'
    layout:
    - 100
    overwrite: false

fs_setup:
  - label: /mnt/inststg1
    filesystem: 'ext4'
    device: /dev/vdb1
    overwrite: false

runcmd:
  - [ mkdir, /mnt/inststg1 ]

mounts:
  - ["/dev/vdb1", "/mnt/inststg1"]

mount_default_fields: [ None, None, "auto", "defaults,nofail", "0", "2" ]
```
{: codeblock}

This script configures /dev/vdb, the first instance storage device with virtio_blk interface type available on the virtual instance. This script can be pasted into the **User Data** field or imported by using the Import user data link on the UI.

This script was tested with a stock image of `Ubuntu 20.04 LTS Focal Fossa`. While the script might be appropriate for other Linux stock and custom images, it is possible that adjustments to the script might be required. 

The cloud-config script is not appropriate for Windows virtual servers. If your instance does not contain instance storage, do not use this script. It might configure an unintended device.
{: important}

See the following information about items that are found in the cloud-config script:

* disk_setup: creates a single default partition spanning the entire disk with a master boot record. The “overwrite: false” causes this operation to be skipped if the device is already partitioned. 
* fs_setup: creates an ext4 file system on the first partition of the vdb block device and gives it the label “inststg1”. You can adjust the file system type and label to suit your needs. The “overwrite: false” setting prevents formatting the partition if it is already formatted with a file system.
* runcmd: creates a directory to use as a mount-point off ‘/’ in the hierarchical parent file system.
* mounts: performs the mount of the file system device onto /inststg1. You can rename the directory path here and under runcmd.
* mount_default_fields: creates a permanent mount directive in the /etc/fstab file. If you issue a reboot command, the virtual server comes back up with the instance storage file system still mounted.

The CLI and API also support the **User Data** field.
{: note}

This cloud-config script example automatically configures an instance storage `/dev/vdb` block device. If you reboot your virtual server only, this configuration continues to work because the configuration and data persists. However, if you start a virtual server that was previously stopped, then the virtual server has a fresh set of instance storage disks when it boots. This situation would require the cloud-init steps to be manually run again. By default, the cloud-config script is run on only the first boot. You can also edit the cloud-init section of the cloud.config file so that the cloud-init steps are automatically run on each boot. For the steps, see the [Edit the cloud_cloud_init_modules section of the cloud.config file to run on each boot](#edit-cloud-init-run-cloud-config-each-boot) section.

### Configuring a two-disk instance storage by using cloud-config script
{: #configure-two-disk}

The following example shows user data that automatically configures an instance storage disk. This example can be used with an instance profile that specifies two disks.

```yaml
#cloud-config
# Cloud-init supports simple partition and file system config.
# This user data yaml will create a full partition on the first two
# virtio_blk devices after the boot device, initialize ext4 on them and
# mount them on new folders off of ‘/mnt/’.
#
disk_setup:
  /dev/vdb:
    table_type: 'mbr'
    layout:
    - 100
    overwrite: false
  /dev/vdc:
    table_type: 'mbr'
    layout:
    - 100
    overwrite: false

fs_setup:
  - label: /mnt/inststg1
    filesystem: 'ext4'
    device: /dev/vdb1
    overwrite: false
  - label: /mnt/inststg2
    filesystem: 'ext4'
    device: /dev/vdc1
    overwrite: false

runcmd:
  - [ mkdir, /mnt/inststg1 ]
  - [ mkdir, /mnt/inststg2 ]

mounts:
  - ["/dev/vdb1", "/mnt/inststg1"]
  - ["/dev/vdc1", "/mnt/inststg2"]

mount_default_fields: [ None, None, "auto", "defaults,nofail", "0", "2" ]
```
{: codeblock}

This cloud-config script example automatically configures both the instance storage `/dev/vdb` and `dev/vdc` block devices, and assumes a `virtio_blk` interface type for instance storage. If reboot your virtual server only, this configuration continues to work because the configuration and data persists.

If you start a virtual server that was stopped, then the virtual server has a fresh set of instance storage disks when it boots. This situation requires the cloud-init steps to be manually run again. By default, the cloud-config script is run on only the first boot. You can also edit the cloud-init section of the cloud.config file so that the cloud-init steps are automatically run on each boot.

#### Edit the cloud_cloud_init_modules section of the cloud.config file to run on each boot
{: #edit-cloud-init-run-cloud-config-each-boot}

The cloud-config script in the previous example automatically configures an instance storage `/dev/vdb` block device. If you reboot your virtual server only, this configuration continues to work because the configuration and data persists. However, if you start a virtual server that was stopped, then the virtual server has a fresh set of instance storage disks when it boots. This situation requires the cloud-init steps to be manually run again. By default, the cloud-config script is run on only the first boot.

* You need a provisioned instance with the **User Data** field that specifies the cloud-init yaml data from the previous example.
* When you log in to the started instance, the file system must be configured and mounted.
* You do not have other directives in the disk_setup or mount sections in the cloud-config yaml that are specified as user data.

To run the cloud-config script on each successive boot, use the following procedure:

1. Edit the `etc/cloud/cloud.cfg` file.
1. Look for the `cloud_init_modules` section - it contains the disk_setup and mount modules.
1. Change the disk_setup and mount modules lines to `always`. The section looks like this.

   ```yaml
   # The modules that run in the 'init' stage
   cloud_init_modules:
   - migrator
   - seed_random
   - bootcmd
   - write-files
   - growpart
   - resizefs
   - [disk_setup, always]
   - [mounts, always]
   - set_hostname
   - update_hostname
   - update_etc_hosts
   - ca-certs
   - rsyslog
   - users-groups
   - ssh
   ```
   {: codeblock}

1. Save the file, then stop and then start the instance. Your mount is automatically created to a newly partitioned and formatted file system.

## User data example for Windows
{: #user-data-example-for-windows}

The following example shows user data that can be passed to a Windows instance. This user data sample sets the time zone.

```yaml
"user_data": "Content-Type: multipart/mixed; boundary=MIMEBOUNDARY\nMIME-Version: 1.0\n\n--MIMEBOUNDARY\nContent-Type: text/cloud-config; charset=\"us-ascii\"\nMIME-Version: 1.0\nContent-Transfer-Encoding: 7bit\nContent-Disposition: attachment; filename=\"cloud-config\"\n#cloud-config\n\nset_timezone: America/Detroit\n\n--MIMEBOUNDARY--\n"
```
{: codeblock}

For more Windows user data examples and information, see [Cloudbase-init 1.0 documentation](https://cloudbase-init.readthedocs.io/en/latest/userdata.html){: external}.

## User data examples for Fedora Core OS
{: #user-data-examples-for-fed-core}

The following examples show user data that can be passed to a Fedora Core OS instance.

The user login 'root' is disabled by default in Fedora Core OS. The user login 'core' can be used to log in to Fedora Core OS instances.
{: note}

Fedora Core OS user data must be in ignition format.

Use the following example to boot a Fedora Core OS instance

   ```sh
   ibmcloud is instance-create $NAME $VPC $ZONE $PROFILE $SUBNET --image-id $IMAGE --key-ids $SSHKEY --user-data @example.ign
   ```
   {: pre}

Use the following example to create a local user.

1. Write the Butane config in the YAML format.

   Butane config
   ```yaml
   variant: fcos
   version: 1.4.0
   passwd:
     users:
       - name: demouser
   ```
   {: codeblock}


2. Use Butane to convert the Butane config into an Ignition config.

   Ignition config
   ```json

   "ignition": {
       "version": "1.4.0"
     },
     "passwd": {
       "users": [
         {
           "name": "demouser"
         }
       ]
     }
   }
   ```
   {: codeblock}


Use the following sample user data to add an SSH key for a local user.

1. Write the Butane config in the YAML format.

   Butane config
   ```yaml
   variant: fcos
   version: 1.4.0
   passwd:
     users:
       - name: demouser
         ssh_authorized_keys:
           - <ssh public key>
   ```
   {: codeblock}


2. Use Butane to convert the Butane config into an Ignition config.

   Ignition config
   ```json

     "ignition": {
       "version": "1.4.0"
     },
     "passwd": {
       "users": [
         {
           "name": "demouser",
           "sshAuthorizedKeys": [
             "<ssh public key>"
           ]
         }
       ]
     }
   }
   ```
   {: codeblock}


For more Fedora Core OS user data examples and information, see [Fedora Project documentation](https://docs.fedoraproject.org/en-US/fedora-coreos/producing-ign/){: external}.

## Next steps
{: #next-steps-user}

After you choose a profile, it's time to plan for and create an instance.
* [Planning for instances](/docs/vpc?topic=vpc-vsi_best_practices)
* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli)
