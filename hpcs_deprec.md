---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-25"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Secure data migration for {{site.data.keyword.hpvs}} from VPC to on-premises
{: #migration_guide}

This comprehensive guide walks you through the process of securely migrating data from your {{site.data.keyword.hpvs}} instance that is running on {{site.data.keyword.cloud_notm}} VPC to an on-premises environment.

{{site.data.keyword.hpvs}} cloud data volumes are fully encrypted using user‑provided seed values, ensuring that the data can be decrypted only by the originating user. These encrypted volumes can be transferred to an on‑premises environment preserving complete confidentiality. Decryption of the transferred volumes is only possible within a [IBM Confidential Computing Container Runtime](https://www.ibm.com/docs/en/hpvs) or [IBM Confidential Computing Container Runtime for Red Hat Virtualization Solutions](https://www.ibm.com/docs/en/hpcr/1.1.x) on‑premises deployment, because the [LUKS decryption passphrase](/docs/vpc?topic=vpc-hyper-protect-virtual-server-mng-data) is generated exclusively inside the Secure Execution boundary of the HPVS instance.
The Secure Execution boundary is designed to protect in‑memory secrets from tampering and inspection, including memory dumping or access attempts by system administrators or the hypervisor. As a result, encryption keys and passphrases remain isolated and protected throughout the lifecycle of the HPVS instance.

The migration steps that are described in this guide are valid only until the End of Service (EOS) window. For more information, see the [Service deprecation announcement](/docs/vpc?topic=vpc-ichpcs_deprecated_anmt).
{: important}

## Prerequisites
{: #migration_prereq}

Make sure that you have the following:

- Active {{site.data.keyword.cloud_notm}} account with HPVS instance
- A Virtual Server Instance (VSI) with `qemu-utils` package installed
- Sufficient storage space for volume snapshots
- Administrative access to both cloud and on-premises environments
- Original contract and seed files that are used for HPVS deployment
- Backup verification: Confirm that you have recent backups of your data
- Downtime planning: It is recommended to schedule a maintenance window and avoid modification to the volume during migration.
- Network connectivity: Ensure secure transfer mechanism (VPN, direct Link, or secure file transfer)
- Required tools: Install `qemu-utils` on your VSI:

   - For Ubuntu or Debian:
   
     ```bash
     sudo apt-get update && sudo apt-get install -y qemu-utils
     ```
     {: codeblock}
   
   - For RHEL or CentOS:

     ```bash
     sudo yum install -y qemu-img
     ```
     {: codeblock}

## Procedure
{: #migration_guide_procedure}

### Step 1: Taking the snapshot of the encrypted disk volume
{: #step1_snapshot_volume}

Take a volume snapshot of the encrypted disk volume that is attached to the HPVS instance. For more information, see [Creating Block Storage for VPC snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui).

### Step 2: Creating a VSI with QEMU using the snapshot
{: #step2_create_vsi}

Create a VSI by importing the snapshot. For more information, see [Creating a virtual server instance in the console](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui).

### Step 3: Identify the volume location
{: #step3_identify_volume}

1. SSH into your VSI:

   ```bash
   ssh -i /path/to/private_key user@<VSI_IP_ADDRESS>
   ```
   {: codeblock}

1. Run the `lsblk` command to list all block devices:

   ```bash
   lsblk
   ```
   {: codeblock}

1. Identify your volume in the output:

   **Example output:**

   ```text
   NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
   vda     254:0    0  100G  0 disk 
   ├─vda1  254:1    0 99.9G  0 part /
   ├─vda14 254:14   0    3M  0 part 
   └─vda15 254:15   0  124M  0 part /boot/efi
   vdb     254:16   0  368K  0 disk 
   vdc     254:32   0   44K  0 disk 
   vdd     254:48   0   10G  0 disk              ← new volume
   ├─vdd1  254:49   0   99M  0 part 
   └─vdd2  254:50   0  9.9G  0 part
   ```
   {: screen}

   The new volume is typically the last device listed (`vdd` in this example).
   {: tip}

4. Note the device path (example: `/dev/vdd`) for use in the next step.

### Step 4: Create a volume snapshot
{: #step4_create_snapshot}

Use QEMU tools to create a compressed snapshot of the volume.

1. Create a directory for storing the snapshot:

   ```bash
   sudo mkdir -p /var/lib/libvirt/images/hpvs/snap-image
   cd /var/lib/libvirt/images/hpvs/snap-image
   ```
   {: codeblock}

1. Create the snapshot by using `qemu-img`:

   ```bash
   sudo qemu-img convert -p -f raw -O qcow2 /dev/vdd vdd.qcow2
   ```
   {: codeblock}

   **Command parameters**:

   - `-p`: Show progress during conversion
   - `-f raw`: Source format (raw block device)
   - `-O qcow2`: Output format (QCOW2 compressed image)
   - `/dev/vdd`: Source device (replace with your device path)
   - `vdd.qcow2`: Output file name

   This process can take considerable time depending on the volume size. A 10 GB volume typically takes 5-15 minutes.
   {: note}

1. Verify that the snapshot was created successfully:

   ```bash
   ls -lh vdd.qcow2
   qemu-img info vdd.qcow2
   ```
   {: codeblock}

   **Expected output**:

   ```text
   image: vdd.qcow2
   file format: qcow2
   virtual size: 10 GiB (10737418240 bytes)
   disk size: 2.5 GiB
   cluster_size: 65536
   ```
   {: screen}

### Step 5: Transfer snapshot to on-premises
{: #step5_transfer_snapshot}

Securely transfer the snapshot file to your on-premises environment.

```bash
scp vdd.qcow2 user@on-premises-host:/var/lib/libvirt/images/hpvs/snap-image/
```
{: codeblock}

### Step 6: Configure on-premises virtual server
{: #step6_configure_onprem}

Update your on-premises virtual server configuration to use the migrated volume.

1. Locate your domain XML configuration file:

   - For HPVS: `hpvsnew.xml`
   - For HPCR: `hpcrnew.xml`

1. Edit the XML file to add or update the disk configuration:

   **For HPVS (`hpvsnew.xml`):**

   ```xml
   <domain type='kvm'>
     <name>hpvs-migrated</name>
     
     
     <devices>
       
       
       
       <disk type='file' device='disk'>
         <driver name='qemu' type='qcow2' cache='none' iommu='on'/>
         <source file='/var/lib/libvirt/images/hpvs/snap-image/vdd.qcow2'/>
         <target dev='vdb' bus='virtio'/>
         <address type='pci' domain='0x0000' bus='0x00' slot='0x07' function='0x0'/>
       </disk>
     </devices>
   </domain>
   ```
   {: codeblock}

   **For HPCR (`hpcrnew.xml`):**
   ```xml

   <domain type='kvm'>
     <name>hpcr-migrated</name>
     
     
     <devices>
       
       
       
       <disk type='file' device='disk'>
         <driver name='qemu' type='qcow2' cache='none' iommu='on'/>
         <source file='/var/lib/libvirt/images/hpvs/snap-image/vdd.qcow2'/>
         <target dev='vdb' bus='virtio'/>
         <address type='pci' domain='0x0000' bus='0x00' slot='0x07' function='0x0'/>
       </disk>
     </devices>
   </domain>
   ```
   {: codeblock}

   **Configuration parameters**:

   - `type='file'`: Disk source is a file
   - `device='disk'`: Device type is a disk
   - `driver name='qemu' type='qcow2'`: Use QEMU driver with QCOW2 format
   - `cache='none'`: Disable caching for better data integrity
   - `iommu='on'`: Enable IOMMU for security
   - `target dev='vdb'`: Device name inside the guest
   - `bus='virtio'`: Use VirtIO for better performance

1. Ensure that you have the original contract and seed files used during the initial HPVS deployment.
