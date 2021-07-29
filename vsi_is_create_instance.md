---

copyright:
  years: 2018, 2021
lastupdated: "2021-07-29"

keywords: instances, virtual servers, creating virtual servers, virtual server instances, virtual machines, Virtual Servers for VPC, compute, vsi, vpc, creating, UI, console

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}
{:beta .beta}

# Creating virtual server instances by using the UI
{: #creating-virtual-servers}

You can create one or more virtual server instances in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console.
{:shortdesc}

To create a virtual server instance:

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

   Be sure to select VPC infrastructure from the Menu icon.
   {: tip}

2. Click **Create** and enter the information in Table 1.

3. Click **Create virtual server instance** when you are ready to provision.

| Field | Value |
|-------|-------|
| Name  | A name is required for your virtual server instance. |
| Resource group | Select a resource group for the instance. |
| Tags | You can assign a label to this resource so that you can easily filter resources in your resource list. |
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the location where you want your virtual server instance to be created. |
| Placement group | Select a placement group for the instance. If you add a placement group, the instance is placed according to the placement group policy. See [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc) for more information. |
| | **Note:** This field is only available if `Add instance to placement group` is selected during provisioning. |
| Type of virtual server | A **Public** virtual server instance, created in a multi-tenant environment, is the default selection for a new instance. You can also select a **Dedicated** virtual server instance to create the instance in a single-tenant space. To provision a dedicated instance, you must have a dedicated host available or [create one](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances). A dedicated host is required if you are using a Windows custom image and [your own license](/docs/vpc?topic=vpc-byol-vpc-about#byol-vpc-windows). |
| Processor architecture | Select the processor architecture that your instance is created with. *x86* means x86_64 bit processor, and *s390x* is LinuxONE (s390x processor architecture).  |
| Operating system | Image - Select a stock image or a custom image for the operating system. A custom image can be one that you created externally and imported to COS, or a custom image created from a boot volume attached to an instance.<br>You can also select a RHEL or Windows custom image and bring your own license (BYOL).<br>All operating system images use cloud-init, which allows you to enter user metadata associated with the instance for post provisioning scripts.<br>For more information about available stock images, and custom image requirements, see [Images](/docs/vpc?topic=vpc-about-images). For information about creating BYOL custom images, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about). For information about creating an image from a volume, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc). |
| | Snapshot - Optionally, select a snapshot of a boot volume that includes an operating system. If you created a boot volume from a bootable snapshot, it appears under Boot Volume. If you want to use another bootable snapshot and create a new boot volume, click **Change** to select a different snapshot from the list of snapshots. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore). |
| Profile |  Select from popular profiles or all available vCPU and RAM combinations. The profile families are Balanced, Compute, and Memory. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles). |
| SSH Key | You must select an existing SSH key or upload a new SSH key to use before you can create the instance. SSH keys are used to securely connect to the instance after it's running. |
| | **Note:** Alpha-numeric combinations are limited to 100 characters. |
| | For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| User data | You can add user data that automatically performs common configuration tasks or runs scripts. For more information, see [User data](/docs/vpc?topic=vpc-user-data). |
| Boot volume | The default boot volume size for all profiles is 100 GB. |
| Data volumes | You can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **Create** in the Data volumes section of the page. For information about provisioning the volume, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi). |
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. You can use the default VPC, another existing VPC, or you can create a new VPC. To create a new VPC, click **New VPC**. |
| Network interfaces | By default the virtual server instance is created with a single primary network interface. You can click the pencil icon to edit the details of the network interface, for example, the subnet or security group that's associated with the interface. To include additional secondary network interfaces, click **New interface**. You can create and assign up to five network interfaces to each instance. For more information, see [About network interfaces](/docs/vpc?topic=vpc-using-instance-vnics#about-network-interfaces). |
{: caption="Table 1. Instance provisioning selections" caption-side="top"}

Do you prefer to create an instance by using the CLI? For more information, see [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli).
{: tip}

## Next steps
{: #next-steps-creating-virtual-servers-ui}

<!---A series of emails is sent to your administrator: Acknowledgment of the virtual server instance order, order approval and processing, and a message that the instance is created.--->

After the instance is created, [associate a floating IP address to the instance](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#reserving-a-floating-ip-address). Then, you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).  If you have an existing instance with a floating IP address, then it is not necessary to assign a second floating IP to another instance. You can connect to the first with a floating IP, then SSH to the second instance using the private subnet IP address that is automatically assigned to it.
