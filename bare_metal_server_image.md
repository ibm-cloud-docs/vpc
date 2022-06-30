---

copyright:
  years: 2021, 2022
lastupdated: "2022-06-30"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Bare metal server images
{: #bare-metal-image}

When you provision a bare metal server on your VPC, you need to select an image to determine the operating system for the server.
{: shortdesc}

## Supported images
{: #bare-metal-images-supported}

The following operating systems are available as images when you create a bare metal server.

| Image | Architecture |
|---|---|
| [Red Hat Enterprise Linux 8.4](#bare-metal-images-rhel-considerations) | x86-64 |
| Red Hat Enterprise Linux for SAP | x86-64 |
| SUSE Red Hat Enterprise Linux for SAP  \n  \n For more information about SAP and bare metal servers, see [Fast Path of IBM Cloud Intel Bare Metal](https://cloud.ibm.com/docs/sap?topic=sap-fast-path-site-map-intel-bm). | x86-64 |
| [Ubuntu 20.04, 18.04](#bare-metal-images-ubuntu-considerations) | x86-64 |
| [VMware ESXi](#bare-metal-images-vmware-esxi-considerations) | x86-64 |
{: caption="Table 1. Bare metal server images" caption-side="bottom"}

Support for custom images, Debian, and Windows is planned. 
{: note}

<!--## Custom images
{: #bare-metal-custom-images}-->

<!--You can import an image from IBM Cloud Object Storage that you can use to create a bare metal server.-->

<!--### Custom image considerations
{: #bare-metal-custom-images-requirements}-->

<!--Custom images must meet the following requirements:-->

<!--* Must support UEFI boot mode
* A Pensando driver for networking
* Support x86 architecture-->

<!--Custom images have the following limitations:-->

<!--* Supports only QCOW2 images
* Encrypted images aren't supported-->

### Special considerations for Red Hat Enterprise Linux 8.4
{: #bare-metal-images-rhel-considerations}

* By default, the release lock feature for Red Hat Enterprise Linux 8.4 is disabled. To prevent the Red Hat Enterprise Linux from going beyond version 8.4 when you run an update, run the following commands from the command line:

   ```text
   # subscription-manager release --set=8.4
   ```
 {: codeblock}

   ```text
   # yum clean all
   ```
   {: codeblock}

### Special considerations for VMware ESXi images 
{: #bare-metal-images-vmware-esxi-considerations}

You can specify how a bare metal server is licensed with VMware&reg; ESXi by either bringing your own license (_ESXi 7.x BYOL_), or you can rent a license through {{site.data.keyword.cloud}} (_ESXi 7.x_)

* The _ESXi 7.x BYOL_ option provides ESXi in an evaluation mode. The evaluation period is 60 days and begins at the time of provisioning. Anytime during the 60-day evaluation period, you can convert from evaluation mode to licensed mode with your appropriate license that you provide.

* The _ESXi 7.x_ option provides ESXi in licensed mode and is activated during the provisioning process. Billing applies for {{site.data.keyword.cloud}} rented licenses. 

ESXi on Bare Metal Servers for VPC is charged monthly and is calculated per CPU based on the selected profile. If you choose to rent VMware ESXi with your server, you are subject to a prorated monthly cost for the license instead of an hourly rate. Proration amount is variable based on your billing anniversary date. 
{: note}

For more information about how to license ESXi, see [Licensing ESXi hosts](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.esxi.install.doc/GUID-28D25806-748B-49C0-97A1-E7DE5CB335A9.html){: external}.

You can view and manage your VMWare licenses [here](https://cloud.ibm.com/classic/devices/vmwarelicenses){: external}.

### Special considerations for Ubuntu images 
{: #bare-metal-images-ubuntu-considerations}

Ubuntu images don't include the VMD device driver in the standard kernel package that is needed to view attached NVMe drives on the system. To obtain this driver, install the `linux-modules-extra-ibm` package and then run `modprobe vmd`.

```java
# export DEBIAN_FRONTEND=noninteractive
# apt update
# apt install -y -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" linux-modules-extra-ibm linux-modules-extra-$(uname -r)
# modprobe vmd 
```
{: codeblock}

## Next steps
{: #bare-metal-images-next-steps}

* [Planning for bare metal servers](/docs/vpc?topic=vpc-planning-for-bare-metal-servers)
* [Creating a bare metal server](/docs/vpc?topic=vpc-creating-bare-metal-servers)
