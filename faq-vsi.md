---

copyright:
  years: 2018, 2026
lastupdated: "2026-02-16"

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for virtual server instances
{: #faqs-for-vsis}

## Can a vNIC have both a floating IP and private IP address?
{: #faq-vsi-0}
{: faq}

Yes, a vNIC on a virtual server instance has a private IP and can be attached to floating IP.

## Instance1 has two vNICs, called vNIC1 and vNIC2. Can these two vNICs be attached to the same subnet?
{: #faq-vsi-1}
{: faq}
 
Yes, you can attach multiple network interfaces of an instance to the same subnet.

## Can an instance be created without a subnet, such as with only a floating IP address?
{: #faq-vsi-2}
{: faq}

No, a virtual server instance must be provisioned in a subnet.

## Can an instance be attached to multiple VPCs?
{: #faq-vsi-3}
{: faq}
{: support}

No, a virtual server instance can be provisioned in only one VPC.

## During floating IP assignment in a VPC, a customer must specify the network interface of the instance, is that correct?
{: #faq-vsi-4}
{: faq}

Yes. Initially, assigning the floating IP to the primary network interface of a server helps establish the data path. Later, you can associate the floating IP to a different network interface if you want. Alternatively, you can manually configure routing for the interface in the guest operating system. For more information, see [Adding or editing network interfaces](/docs/vpc?topic=vpc-using-instance-vnics#editing-network-interfaces).

## Imagine that instance1 in a VPC has only vNIC1 and it is attached to Subnet1. Subnet1 is attached to a Public Gateway (PGW). Can a customer still assign a floating IP to instance1?
{: #faq-vsi-6}
{: faq}

Yes, a server can be on a subnet that is attached to a public gateway and also have a floating IP. The assignment of floating IP to an instance is not related to whether a public gateway is attached to the subnet. A floating IP associated to an instance takes precedence over the public gateway that is attached to the subnet.

## What regions are available?
{: #faq-vsi-7}
{: faq}
{: support}

You can create virtual server instances for {{site.data.keyword.vpc_full}} in Chennai (in-che), Dallas (us-south), Frankfurt (eu-de), London (eu-gb), Madrid (eu-es), Montreal (ca-mon), Osaka (jp-osa), São Paulo (br-sao), Sydney (au-syd), Tokyo (jp-tok), Toronto (ca-tor), and Washington DC (us-east).

## Can I use existing virtual server instances from my classic infrastructure with an {{site.data.keyword.vpc_short}}?
{: #faq-vsi-8}
{: faq}
{: support}

You can migrate a virtual server instance from the classic infrastructure to a VPC. You need to create an image template, export it to {{site.data.keyword.cos_full_notm}}, and then customize the image to meet the requirements of the VPC infrastructure. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc).

## What virtual server families are supported in {{site.data.keyword.vpc_short}}?
{: #faq-vsi-10}
{: faq}
{: support}

Currently, public virtual servers in the balanced, memory, and compute families are supported. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles#profiles).

## What do I do if an instance is in a bad state, such as continually starting or stopping?
{: #faq-vsi-11}
{: faq}
{: support}

You can issue a command to force the instance to stop. Use the {{site.data.keyword.cloud_notm}} CLI to obtain the instance ID, and then run the following command, `ibmcloud is instance-stop --no-wait -f`.  When the instance is stopped, you can either restart it or delete it.

## When I attempt to update my Ubuntu image with apt, I receive an error about the grub menu.lst file. How do I fix it?
{: #faq-vsi-12}
{: faq}
{: support}

Edit the file "`/boot/grub/menu.lst`" by changing `# groot=LABEL...` into `# groot=(hd0)`. Then, run following command, `sudo update-grub-legacy-ec2`.

## In what cases is my virtual server migrated to a different host?
{: #faq-vsi-13}
{: faq}

In limited cases a virtual server might need to be migrated to a different host. If a migration is required, the virtual server is shut down, migrated, and then restarted. A virtual server might be migrated in the following cases:

* Infrastructure [maintenance](/docs/vpc?topic=vpc-about-cloud-maintenance). You might receive an email that's indicating that [maintenance](/docs/vpc?topic=vpc-about-cloud-maintenance) is required on a system that is hosting your virtual server. Your virtual server might need to be migrated as part of the infrastructure [maintenance](/docs/vpc?topic=vpc-about-cloud-maintenance).
* An unplanned host outage. The following actions apply if an unexpected host failure occurs:
    * The virtual servers that were running on the host are stopped when the outage is detected.
    * The virtual servers are automatically reassigned and restarted on a different compute host in the same multi-region zone.
    * The virtual servers that are restarted use the same boot volume, and the same data volumes as the original virtual server.
    * The virtual server that was restarted is assigned the same floating IP, static IP, and dynamic IP addresses on the new node.
    * If the virtual server cannot be scheduled to run on another compute node, it is placed in a `FAILED` state.

## Can I use an encrypted image?
{: #faq-vsi-14}
{: faq}

Yes, you can encrypt a supported custom image with LUKS encryption and your own passphrase. For more information, see [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image). When your image is encrypted and imported to {{site.data.keyword.vpc_short}}, you can use it to provision virtual server instances.

## Can I use my own license for a custom image that I create?
{: #faq-vsi-15}
{: faq}

Yes, for certain versions of Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server, and Windows operating systems, you can bring your own license (BYOL) to the IBM Cloud VPC when you import a custom image. These images are registered and licensed by you. You maintain control over your license and incur no additional costs by using your license. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about).

## How does IBM handle maintenance?
{: #faq-vsi-16}
{: faq}

For more information, see [Understanding Cloud Maintenance Operations](/docs/vpc?topic=vpc-about-cloud-maintenance).

## What happens if my virtual server host fails?
{: #faq-vsi-17}
{: faq}

For more information, see [Understanding Cloud Maintenance Operations](/docs/vpc?topic=vpc-about-cloud-maintenance).

## What are the disks that I see associated with my new Windows virtual server instance?
{: #faq-vsi-18}
{: faq}

When you provision a Windows virtual server instance with a stock image, disk manager might show unexpected disks. After a new Windows instance is provisioned from a stock image, a cloud-init disk and a swap disk are present. The cloud-init disk might display a size of 378 KB. The swap disk might display a size of 44 KB; the swap disk is turned off eventually. These small disks are working as designed. You are not to attempt to delete or format either of these disks that are associated with your new Windows virtual server instance.

## What is _image from volume_ and how does it relate to virtual server instances?
{: #faq-vsi-19}
{: faq}

You can create a custom image from a boot volume that is attached to a virtual server instance. Then, you can use the custom image to provision new virtual server instances. For more information, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc).

## What is the virtual server instance identifier and SMBIOS system-uuid?
{: #faq-vsi-20}
{: faq}

The virtual server instance is automatically assigned an instance identifier (ID), which includes the SMBIOS system-uuid as a portion of the ID, when the instance is created. IDs are immutable, globally unique, and never reused, so the ID uniquely identifies a particular instantiation of a virtual server instance across all of IBM Cloud. The ID, including the SMBIOS system-uuid portion, is static and persists for the lifecycle of the virtual server instance until that virtual server instance is deleted.

For more information, including how to retrieve this information from within your virtual server, see [Retrieving the virtual server instance identifier](/docs/vpc?topic=vpc-managing-virtual-server-instances#retrieve-VSI-instance-identifer) section in Managing virtual server instances.

## Can I access metadata about my virtual server instances?
{: #faq-vsi-21}

Yes, if your account is granted special approval, you can access the metadata service to get information about your VPC compute resources. The metadata service is a REST API that you invoke by using a well-known URI to retrieve instance-specific information from the metadata server. For more information, see For more information, see [About Metadata for VPC (Beta)](/docs/vpc?topic=vpc-imd-about).

## Can I use the same operating system image for more than one account?
{: #faq-vsi-22}
{: faq}

Yes, when you create a custom image for {{site.data.keyword.vpc_short}}, you can import those images into a private catalog for use among multiple accounts. A private catalog provides a way for you to manage access to products for multiple accounts, if those accounts are within the same enterprise. You must first complete all the steps to import the custom image into {{site.data.keyword.vpc_short}} before you can import the image into a private catalog. For more information about private catalog considerations and limitations, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images).

## Can I use a custom image in a private catalog with an instance group?
{: #faq-vsi-23}
{: faq}

Yes, you can use a custom image in a private catalog with an instance group. However, you must first create a service-to-service policy to `globalcatalog-collection.instance.retrieve` before you can create the instance group. For more information, see [Using a custom image in a private catalog with an instance group](/docs/vpc?topic=vpc-private-catalog-image-instance-group&interface=ui).

## Can I change the boot volume associated with my resource group after creating the resource group?
{: #faq-vsi-24}
{: faq}

No. The resource group of a volume is set at resource creation and cannot be changed. This behavior is shared by all VPC resources.

## Can I move my volume to another region or zone?
{: #faq-vsi-25}
{: faq}

No, a volume is restricted to the zone it was created in. However, you can move the volume's data by creating a new snapshot from the volume, and creating a new volume from that snapshot in a different zone.

## Why I am not able to connect to internet from my virtual server instance?
{: #faq-vsi-26}
{: faq}

Virtual server instances use IBM VPC DNS server addresses such as `161.26.0.10` and `161.26.0.11`. If you are unable to connect to the internet, check whether you are able to ping both of these IP addresses from your instance. For example, your Linux instances should have the following entries automatically when they are provisioned.

```sh
more /etc/resolv.conf
Generated by NetworkManager

nameserver 161.26.0.10
nameserver 161.26.0.11
```
{: codeblock}

You can also check whether you have rules to allow UDP port 53 for DNS traffic in a security group.
