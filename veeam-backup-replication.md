---

copyright:
  years: 2020, 2025
lastupdated: "2025-03-11"

keywords: instance backup, veeam, replication software

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Installing and operating the Veeam Backup and Replication software
{: #using-veeam-backup-replication-software}

Although the [Veeam Agent](/docs/vpc?topic=vpc-using-veeam-agent) is the recommended method, you can also use the Veeam Backup and Replication software as an optional way of managing backups of {{site.data.keyword.vsi_is_full}}.
{: shortdesc}

The Veeam Backup and Replication software can be installed only on a Microsoft Windows operating system. However, you can use it to manage backups for multiple virtual server instances that are deployed with any of the supported Linux&reg; and Windows operating systems. Supported Linux&reg; distributions include CentOS, RHEL, Ubuntu, and Debian.

The following example provides information on installing, configuring, and creating a backup of a volume that is attached to a CentOS instance. It also describes how to restore the backed-up volume data by using the Veeam Agent for Linux. In addition, specific instructions are provided for completing a volume backup and restoration, but you would use similar instructions if you prefer to do file and folder backup and restore operations.

In general, similar instructions apply to the other Linux operating system types supported for virtual server instances for VPC.
{: note}

## Before you begin
{: #before-you-begin-veeam-backup-restore}

* Make sure that you order a [Veeam license](/docs/vpc?topic=vpc-ordering-veeam-licenses).
* Provision and set up a Microsoft Windows virtual server instance for VPC. The Veeam Backup and Replication software requires a Microsoft Windows instance with at least 8 vCPUs, 32 GB of memory, and a secondary Block Storage device for the Veeam backup repository.
* Make sure that all resources to be backed up are in the same VPC as the Microsoft Windows instance.

## Installing and configuring
{: #installing-configuring-veeam-backup-replication}

1. After you provision your Microsoft Windows instance for VPC, use the Microsoft Remote Desktop to connect to the instance. For more information, see [Connecting to Windows instances](/docs/vpc?topic=vpc-vsi_is_connecting_windows).
2. Use the Server Manager Dashboard to bring the secondary volume online, create a volume, and assign the drive letter `E:\`.
3. Use a browser to download the Veeam Backup and Replication software from the [Download Veeam Products](https://www.veeam.com/products/downloads.html){: external} page.
4. Install the Veeam Backup and Replication software by using the Veeam installation instructions.
5. Transfer the Veeam license ID file to the Microsoft Windows instance. Start the Veeam Backup and Replication software and use the acquired license ID file to activate the software.
6. From the **Backup Infrastructure** menu, select **Backup Repositories** and confirm that the **Default Backup Repository** path is `E:\Backup`. If the path is not `E:\Backup`, then select **add backup repository**. Choose **Direct attached storage > Microsoft Windows**. Select your Microsoft Windows instance as the server and populate the server with the `E:\` drive. In this case, you end up with two repositories, and you need to select the one pointing to your `E:\` drive for backup operations.

## Backing up and restoring
{: #backing-up-restoring-veeam-backup}

### Volume backup of CentOS instance for VPC
{: #volume-backup-centos-instance-vpc}

#### Before you begin
{: #before-you-begin-volume-backup}

You must complete the following requirements before you create a backup job for the CentOS virtual server instance for VPC:

* Transfer the private key (for example, `~/.ssh/id_rsa`), which is associated with the public SSH key that is used to create the CentOS instance, to the Microsoft Windows instance.
* Make sure that the Microsoft Windows instance can communicate over the private network with the CentOS instance.

#### Creating a volume backup job
{: #creating-volume-backup-job}

To create a backup job for a CentOS virtual server instance, complete the steps that are outlined in this [Veeam Backup and Replication guide](https://helpcenter.veeam.com/archive/backup/100/agents/agent_job_create_linux.html){: external}.

To begin, start the Veeam Backup and Replication software. From the _HOME_ menu, select **Backup Job > Linux Computer**. As you go through the Veeam Backup and Replication software control panes, make the following selections:

| Menu pane        | Value                                                                    |
|------------------|--------------------------------------------------------------------------|
| Job Mode         | Select **Server** for the type and **Managed by backup server** for the mode.|
| Name             | Enter **Agent Backup Job: CentOS server 1**.|
| Computers        | Select **Add**. Next, select **Individual Computer**. Enter the private IP address of the CentOS instance. For **Credentials**, select **Add** and then select the **Linux private key**. For a **username**, select "root". For the **password**, leave blank. For **private key**, select **Browse**, and then go to the location of the `id_rsa` file that was transferred to the system and select it.|
| Backup Mode      | Choose **Volume Level Backup**.|
| Objects          | Choose **Add** and then select **Device**. Add the device path for each volume you want to back up. For this example, the “/” root volume, the device path is input at `/dev/vda2`.|
| Storage          | Choose the backup repository where you want the backup to be stored.|
| Guest Processing | Select any options that you want.|
| Schedule         | Choose the preferred time schedule, and then select **Finish** to complete the job creation.|
{: caption="Creating volume backup job menu panes and values" caption-side="bottom"}

The backup job runs at the scheduled time. On the initial run, the Veeam Agent for Linux is installed and configured on the CentOS instance for VPC. The selected volumes are then backed up to the Veeam backup repository.
{: note}

### Recover volume backup for CentOS instance for VPC
{: #recover-volume-backup}

To recover files from a volume backup, complete the following steps:

1. Log in to the CentOS virtual server instance.
2. From the command line, start the Veeam Agent configuration tool, which was automatically installed by the Veeam Backup and Replication software:

   ```sh
   veeamconfig ui
   ```
   {: pre}

3. Select **Recover Files**, and then select the **Job Name** of the backup that you want to recover.
4. Select the **Restore Point** (creation timestamp of the backup). This mounts the backup into `/mnt/backup`. Each individual device and volume that is backed up is mounted inside of `/mnt/backup`.
5. When you recover your files, unmount the backup.
