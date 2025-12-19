---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-18"

keywords: Block Storage for VPC, iscsi for VPC, SAN for VPC, format windows storage disk

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# Setting up your {{site.data.keyword.block_storage_is_short}} data volume for use (Windows)
{: #start-using-your-block-storage-data-volume-win}

If you want to use your {{site.data.keyword.block_storage_is_full}} volume as a file system, you need to partition the volume, format it, and then mount it as a file system. You can perform this operation after you created a {{site.data.keyword.block_storage_is_short}} volume and attached it to an instance.
{: shortdesc}

Follow this procedure to use your Block Storage volume on a Windows&reg; system.

## Setting up your volume for use with the Disk Management utility
{: #diskmanagementutil}

1. Log in to your Windows instance by using Remote Desktop. For more information, see [Connecting to Windows instances](/docs/vpc?topic=vpc-vsi_is_connecting_windows).
1. Start the Disk Management utility. On the taskbar, open the menu by right-clicking the Windows logo, and choose Disk Management.
  
   In Windows Server 2008, choose Start > Administrative Tools > Computer Management > Disk Management.
   {: note}

1. The Disk Management window shows the attached storage volume as an unknown, offline disk. Bring the volume online by right-clicking the {{site.data.keyword.block_storage_is_short}} volume. Choose **Online**.
1. If the disk is not initialized, you must initialize it before you can use it. If the disk is already initialized, skip to the next step.

   If you're mounting a volume that already has data on it, do not reformat the volume as that deletes the existing data. For example, if you restored the volume from a snapshot, the volume contains the data from the snapshot. Do not initialize the volume or you lose the data that you restored.
   {: warning}

    1. Right-click the disk, and choose **Initialize Disk**.
    1. In the **Initialize Disk** dialog box, select a partition style, and choose **OK**.
1. Right-click the disk, and choose **New Simple Volume**.
1. In the **New Simple Volume Wizard**, choose **Next**.
1. If you want to change the default maximum value, specify the size in MB in the Simple volume size field, and then choose **Next**.
1. Specify a preferred drive letter, if necessary, and then choose Next.
1. Specify a Volume Label and adjust the default settings as necessary, and then choose Next.
1. Review your settings, and then choose Finish to apply the modifications and close the New Simple Volume wizard.

## Setting up your volume for use with Windows PowerShell
{: #winpowershell}

1. Log in to your Windows instance by using Remote Desktop. For more information, see [Connecting to Windows instances](/docs/vpc?topic=vpc-vsi_is_connecting_windows).
1. On the taskbar, open the Start menu, and choose **Windows PowerShell**.
1. Use the following commands.

```powershell
Stop-Service -Name ShellHWDetection
Get-Disk | Where PartitionStyle -eq 'raw' | Initialize-Disk -PartitionStyle MBR -PassThru | New-Partition -AssignDriveLetter -UseMaximumSize | Format-Volume -FileSystem NTFS -NewFileSystemLabel "Volume Label" -Confirm:$false
Start-Service -Name ShellHWDetection
```
{: pre}

The script performs the following actions.
* Stops the ShellHWDetection service.
* Lists the disks where the partition style is raw.
* Creates a partition that spans the maximum size that the disk and partition type can support.
* Assigns an available drive letter.
* Formats the file system as NTFS with the specified file system label.
* Starts the ShellHWDetection service again.
