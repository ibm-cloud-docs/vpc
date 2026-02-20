---

copyright:
  years: 2020, 2026
lastupdated: "2026-02-20"

keywords: instance backup, veeam, veeam license, order license, virtual private cloud

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Installing and operating the Veeam Agent
{: #using-veeam-agent}

The [Veeam Agent for Linux](https://www.veeam.com/products/physical/linux-backup-recovery.html){: external} is supported on Linux&reg; operating systems. Supported distributions include CentOS, RHEL, Ubuntu, and Debian. Similarly, the [Veeam Agent for Windows](https://www.veeam.com/products/physical/windows-backup-recovery.html){: external} is available for use on Windows instances for {{site.data.keyword.vpc_short}}.
{: shortdesc}

[Deprecated]{: tag-deprecated} The Veeam Agent is not supported for LinuxONE (s390x processor architecture).
{: note}

The following example provides information on installing, configuring, and operating the Linux Agent on a virtual server instance that is provisioned with CentOS. Similar instructions apply to the other operating system types, although you need to replace any OS-specific commands. The example provides instructions for completing a volume backup and restoration. If you prefer to do a file and folder backup and restoration operations, you use a similar procedure. The wanted restore point is mounted, and the wanted backup files are copied to the virtual server instance.

Full image-level (entire machine) backup and restore is not supported.
{: important}

## Before you begin
{: #before-you-begin-veeam-agent}

* Make sure that you order a [Veeam license](/docs/vpc?topic=vpc-ordering-veeam-licenses).
* Provision and set up a CentOS virtual server instance for VPC. CentOS is used for this example.
* Make sure that your secondary volume is properly configured before you install the Veeam software on the instance. In the following example, it is expected that an `ext3` file system exists on the secondary volume and mounted on `/veeam_disk`.

## Installing and configuring
{: #installing-configuring-veeam-agent}

After you provisioned your instance and configured the secondary volume, [download the Veeam Agent for Linux](https://www.veeam.com/linux-backup-download.html){: external} software installation package onto the instance.

1. Install the downloaded package.

   ```sh
   yum install ./veeam-release-el7-1.0.7-1.x86_64.rpm
   ```
   {: pre}

2. After the package is installed, install the Veeam Linux agent.

   ```sh
   yum install veeam
   ```
   {: pre}

3. Transfer the Veeam license ID file to the instance. The Veeam Linux agent is configured by the Veeam configuration tool. From the command line, enter the following command.

   ```sh
   veeamconfig ui
   ```
   {: pre}

4. Follow the menu prompts and use the license ID to activate the software.

## Backing up
{: #backing-up-veeam-agent}

To complete a backup operation of a volume on the CentOS virtual server instance, follow the steps that are outlined in the [Veeam Agent for Linux 4.0 guide](https://helpcenter.veeam.com/archive/agentforlinux/40/userguide/backup_job_create_gui.html){: external}.

To begin, select **Configure** from the initial menu. As you go through the Veeam Agent control windows, be sure to make the following selections:

| Menu        | Value |
|-------------|-------|
| Name        | Input a name for the backup job. |
| Back up     | Select **Volume Level Backup**. |
| Volumes     | Add either a **Device**, **mount point**, **LVM**, or **BTRFS**. In this example, add a **mount point** and select the "/" mount point. |
| Destination | Choose **Local** and input the location of the secondary volume to be used for the Veeam backup repository. For this example, the location input is `/veeam_disk`.|
| Advanced    | Select any options that you want. This example uses the default options. |
| Schedule    | Input the time schedule for the backup job to run. |
| Summary     | Select **start job now** (if wanted) and then select **Finish**. |
{: caption="Veeam Agent for Linux menu panes and values" caption-side="bottom"}

The backup runs at the scheduled time.
{: note}

## Restoring
{: #restoring-veeam-agent}

*	The _original CentOS instance for VPC_ refers to a virtual server instance for VPC that successfully completed a volume backup to the secondary volume.
*	The _new CentOS instance for VPC_ refers to a virtual server instance for VPC that was provisioned with CentOS with no secondary volume is activated the Veeam Linux Agent.

### Restoring files to original CentOS virtual server instance
{: #restore-files-original-instance}

The following example shows how you can restore files from a volume backup to the original CentOS virtual service instance.

1. Log in to the CentOS virtual server instance. From the command line, start the Veeam configuration tool:

   ```sh
   veeamconfig ui
   ```
   {: pre}

2. Select **Recover Files**, and then select the **Job Name** of the backup you want to recover.
3. Select the **Restore Point** (creation timestamp of the backup). This selection mounts the backup into `/mnt/backup`. Each individual device and volume that is backed up is mounted inside of `/mnt/backup`.
4. When you recover your files, unmount the backup.

### Restoring files to new CentOS virtual server instance
{: #restore-files-new-instance}

The following example shows how you can take a secondary volume that holds a Veeam backup repository and import the repository onto a new CentOS virtual server instance for {{site.data.keyword.vpc_short}}.

#### Detaching secondary volume from original instance
{: #detach-secondary-volume}

The original CentOS instance for VPC needs to be shut down so that it doesn't have access to the secondary volume. The secondary volume can be detached through the {{site.data.keyword.cloud_notm}} CLI:

```sh
ibmcloud is in-vold <Original VPC instance ID> <volume attachment ID>
```
{: pre}

#### Attaching a secondary volume to a new instance
{: #attach-secondary-volume}

The secondary volume is then attached to the new CentOS instance for VPC by running:

```sh
ibmcloud is in-vola <volume attachment name> <New VPC instance ID> <volume  ID>
```
{: pre}

#### Verifying secondary volume
{: #verify-secondary-volume}

After the secondary volume is attached to the new CentOS instance, you need to verify that the secondary volume is available.

Use the following example:

```sh
$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    253:0    0  100G  0 disk
├─vda1 253:1    0    1G  0 part /boot
└─vda2 253:2    0   99G  0 part /
vdb    253:16   0  368K  0 disk
vdc    253:32   0   44K  0 disk [SWAP]
vdd    253:48   0  300G  0 disk
```
{: screen}

In this example, the secondary volume is the `vdd` device.

#### Mounting the volume
{: #mount-volume}

Make a mount point directory and mount the volume.

```sh
mkdir /veeam_disk
mount /dev/vdd /veeam_disk
```
{: screen}

#### Importing the backup repository
{: #import-backup-repository}

The secondary volume is now ready for the Veeam Agent to import the backup repository. Use the following steps to import the backup repository.

1. Start the Veeam Agent:

   ```sh
   veeamconfig ui
   ```
   {: pre}

2. Select **Recover Files**, and **Import backup**. Then, select the backup location. In this example, it is:

   ```sh
   vdd	virtio /veeam_disk
   ```
   {: screen}

3. In **Browse for backup files**, select the appropriate backup job file directory and then select the backup job file. In this example, this file is the backup job file:

   ```sh
   BackupJob1.vdm
   ```
   {: screen}

4. Select the backup creation timestamp. This selection mounts the backup into `/mnt/backup`. Each individual device and volume that is backed up is mounted inside of `/mnt/backup`.

5. When you recover your files, unmount the backup.
