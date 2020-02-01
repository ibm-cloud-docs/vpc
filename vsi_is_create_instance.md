---

copyright:
  years: 2018, 2020 
lastupdated: "2019-12-09"

keywords: vpc, vsi, virtual server instance, creating, UI, console

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

# Creating virtual server instances by using the UI
{: #creating-virtual-servers}

You can create one or more instances in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console.
{:shortdesc}

Before you can create an instance, you need to [create an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{:important}

To create an instance:
1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**. 

Be sure to select VPC infrastructure from the Menu icon.   
{: tip}

2. Click **New instance** and enter the information in Table 1.
3. Click **Create virtual server instance** when you are ready to provision.

| Field | Value |
|-------|-------|
| Name  | A name is required for your virtual server instance. |
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. |
| Resource group | Select a resource group for the instance. |
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the location where you want your virtual server instance to be created. |
| Processor architecture | Select the processor architecture (x86 or POWER) that you want to use for your instances. The default value is x86, which is good for general-purpose workloads. Available in Beta, the POWER processor can be used for general-purpose workloads, or accelerated AI and deep learning workloads that require faster throughput.  |
| Image | All images use cloud-init, which allows you to enter user metadata associated with the instance for post provisioning scripts. |
| Profile |  Select from popular profiles or all available vCPU and RAM combinations. The profile families are Balanced, Compute, Memory and GPU (available after selecting the POWER processor architecture). For more information, see [Profiles](/docs/vpc?topic=vpc-profiles). |
| SSH Key | You must select an existing SSH key or upload a new SSH key to use before you can create the instance. SSH keys are used to securely connect to the instance after it's running. |
| | **Note:** Alpha-numeric combinations are limited to 100 characters. |
| | For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| User data | You can add user data that automatically performs common configuration tasks or runs scripts. For more information, see [User data](/docs/vpc?topic=vpc-user-data). |
| Boot volume | The default boot volume size for all profiles is 100 GB. |
| Attached block storage volume | You can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **New block storage volume** |
| Network interfaces | Assign networking options to connect into the IBM Cloud VPC. You can create and assign up to five network interfaces to each instance. |
{: caption="Table 1. Instance provisioning selections" caption-side="top"}

Do you prefer to create an instance by using the CLI? For more information, see [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli).
{: tip}

## Next steps
{: #next-steps-creating-virtual-servers-ui}

<!---A series of emails is sent to your administrator: Acknowledgment of the virtual server instance order, order approval and processing, and a message that the instance is created.--->

After the instance is created, associate a floating IP address to the instance. Then, you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux).  If you have an existing instance with a floating IP address, then it is not necessary to assign a second floating IP to another instance. You can connect to the first with a floating IP, then SSH to the second instance using the private subnet IP address that is automatically assigned to it.
