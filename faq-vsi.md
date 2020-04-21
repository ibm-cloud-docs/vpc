---

copyright:
  years: 2019, 2020
lastupdated: "2020-04-21"

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}

# FAQs for virtual server instances
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

## During floating IP assignment in a VPC, a customer must specify the vNIC of the instance, is that correct?
{: #faq-vsi-4}
{: faq}

Yes, and in fact, it must be the primary network interface of a server.

## Imagine that instance1 in a VPC has only vNIC1 and it is attached to Subnet1. Subnet1 is attached to a Public Gateway (PGW). Can a customer still assign a floating IP to instance1?
{: #faq-vsi-6}
{: faq}

Yes, a server can be on a subnet that is attached to a public gateway and also have a floating IP. The assignment of floating IP to an instance is not related to whether a public gateway is attached to the subnet. A floating IP associated to an instance takes precedence over the public gateway that is attached to the subnet.

## What regions are available?
{: #faq-vsi-7}
{: faq}
{: support}

You can create virtual server instances for {{site.data.keyword.vpc_full}} in Dallas (us-south), Washington DC (us-east), London (eu-gb), and Frankfurt (eu-de).

## Can I use existing virtual server instances from my classic infrastructure with an {{site.data.keyword.vpc_short}}?
{: #faq-vsi-8}
{: faq}
{: support}

You can migrate a virtual server instance from the classic infrastructure to a VPC. You need to create an image template, export it to IBM Cloud Object Storage, and then customize the image to meet the requirements of the VPC infrastructure. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc).

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

Edit the file "`/boot/grub/menu.lst`" by changing `# groot=LABEL...` into `# groot=(hd0)`. Then, run following command, `sudo update-grub-legacy-ec2`. For more information, see [Error: groot must be grub root device on ubuntu](https://developer.ibm.com/answers/questions/462237/error-groot-must-be-grub-root-device-on-ubuntu/){: external}.

## In what cases is my virtual server migrated to a different host?
{: #faq-vsi-13}
{: faq}

In limited cases a virtual server might need to be migrated to a different host. If a migration is required, the virtual server is shut down, migrated, and then restarted. A virtual server might be migrated in the following cases:

* Infrastructure maintenance. You might receive an email indicating that maintenance is required on a system that is hosting your virtual server. Your virtual server might need to be migrated as part of the infrastructure maintenance.
* An unplanned host outage. The following actions apply if there is an unexpected host failure:
    * The virtual servers that were running on the host are stopped when the outage is detected. 
    * The virtual servers are automatically reassigned and restarted on a different compute host in the same multi-region zone. 
    * The virtual servers that are restarted use the same boot volume, and the same data volume(s) as the original virtual server.  
    * The virtual server that was restarted is assigned the same floating IP, static IP, and dynamic IP addresses on the new node.  
    * If the virtual server cannot be scheduled to run on another compute node, it is placed in a `FAILED` state.


