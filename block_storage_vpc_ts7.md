---

copyright:
  years: 2025
lastupdated: "2025-09-29"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot, expanding boot volume, windows

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I expand my C: drive on Windows Server
{: #troubleshooting-unexpandable-c-drive}
{: troubleshoot}
{: support}

You used a stock image of Windows Server 2022 Standard Edition or Windows Server 2025 Standard Edition to create your virtual server. You also chose to expand the boot volume up to 250 GB. However, the C: drive cannot be extended.
{: tsSymptoms}

Because the stock windows image supports both BIOS and UEFI boot, when the boot volume is created, it has a C: drive and a Recovery partition that prevents the expansion of the C: drive.
{: tsCauses}

You can remove the D: drive before you expand the C: drive. Take a snapshot of your boot volume as a precaution before you begin.
{: tsResolve}

Open the Command Prompt or PowerShell as an administrator, and run the following commands:

1. Disable the Windows Recovery Environment, so the WinRE entry is removed from the Boot Configuration Data and the WinRE image is detached.

   ```sh
   reagentc /disable
   ```
   {: pre}

   ```sh
   PS: C:\Users\Administrator> reagentc /disable
   REAGENTC.EXE: Operation Successful.
   ```
   {: screen}

1. Start the DiskPart utility.

   ```sh
   diskpart
   ```
   {: pre}

   ```sh
   PS: C:\Users\Administrator> diskpart

   Microsoft DiskPart version 10.0.26100.1150
   ```
   {: screen}

1. Select the boot disk and list the partitions.

   ```sh   
   sel disk 1
   ```
   {: pre} 

   ```sh
   list part
   ```
   {: pre}

   ```sh
   DISKPART> sel disk 1

   Disk 1 is now the selected disk.

   DISKPART> list part

     Partition ###  Type          Size    Offset
     -------------  ------------  ------  -------
     Partition 1    Primary       100 MB  1024 KB
     Partition 2    Primary        98 GB   101 MB
     Partition 3    Primary       200 MB    98 GB
     Partition 4    Recovery      681 MB    99 GB
   ```
   {: screen}  

1. Identify for the partition with the type "recovery", and select it and delete it.
   
   ```sh
   sel part 4
   ```
   {: pre}

   ```sh
   del part override
   ```
   {: pre}

   ```sh
   DISKPART> sel part 4

   Partition 4 is now the selected partition.

   DISKPART> del part override

   DiskPart Successfully deleted the selected partition

   DISKPART> list part

     Partition ###  Type          Size    Offset
     -------------  ------------  ------  -------
     Partition 1    Primary       100 MB  1024 KB
     Partition 2    Primary        98 GB   101 MB
     Partition 3    Primary       200 MB    98 GB
   ```
   {: screen}

1. Delete the next partition.

   ```sh
   DISKPART> sel part 3

   Partition 3 is now the selected partition.

   DISKPART> del part override

   DiskPart Successfully deleted the selected partition

   DISKPART> list part

     Partition ###  Type          Size    Offset
     -------------  ------------  ------  -------
     Partition 1    Primary       100 MB  1024 KB
     Partition 2    Primary        98 GB   101 MB
   ```
   {: screen}

1. List the disks.
   ```sh
   DISKPART> list disk

     Disk ###  Status        Size    Free     Dyn  Gpt
     --------  ------------  ------  -------  ---  ---
     Disk 0    Online         44 KB    44 KB
   * Disk 1    Online        100 GB  1087 MB
     Disk 2    Online        378 KB   378 KB
   ```
   {: screen}  

1. Select the second partition and extend it.

   ```sh
   DISKPART> sel part 2

   Partition 2 is now the selected partition.

   DISKPART> extend

   DiskPart successfully extended the volume.
   ```
   {: screen}

1. Now you can exit the DiskPart utility by typing the following command.

   ```sh
   exit
   ```
   {: pre}

1. Re-enable the Windows Recovery Environment.

   ```sh
   PS: C:\Users\Administrator> reagentc /ensable
   REAGENTC.EXE: Operation Successful.
   ```
   {: screen}
