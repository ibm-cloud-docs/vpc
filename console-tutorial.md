---

copyright:
  years: 2018, 2019

lastupdated: "2019-09-30"


keywords: vpc, virtual private cloud, vpc ui, console, access control list, virtual server instance, subnet, block storage volume, security group, images, monitoring, ssh key, ip range

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}

# Using the {{site.data.keyword.cloud_notm}} console to create VPC resources
{: #creating-a-vpc-using-the-ibm-cloud-console}

You can create and configure an {{site.data.keyword.vpc_full}} (VPC) by using the {{site.data.keyword.cloud_notm}} console.
{:shortdesc}

To create and configure your VPC and other attached resources, perform the steps in the sections that follow, in this order:

1. Create a VPC and subnet to define the network. When you create your subnet, attach a public gateway if you want to allow all resources in the subnet to communicate with the public internet.
1. Create a generation 2 virtual server instance. By default, a 100 GB boot volume is attached to the instance.
1. If you want more storage, create a block storage volume and attach it to your instance.
1. To define the inbound and outbound traffic that's allowed for the instance, configure its security group.
1. If you want your instance to be reachable from the internet, reserve and associate a floating IP address.

After you enter data on the provisioning pages, you can click the **Get sample API call** button to view the sequence of API requests that correspond to your settings. Viewing the API calls is a good way to learn about the API and understand actions and their dependencies.
{: tip}

## Before you begin
{: #before}

Set up your account to access VPC. Make sure that your account is [upgraded to a paid account](/docs/account?topic=account-accountfaqs#changeacct){: new_window}. 

Make sure that you have an SSH key. The key is used to connect to the virtual server instance. For example, generate an SSH key on your Linux server by running the following command:
```
ssh-keygen -t rsa -C "user_ID"
``` 
{: pre}

This command generates two files. The generated public key is in the `id_rsa.pub` file under an ``.ssh`` directory in your home directory, for example, ``/Users/<USERNAME>/.ssh/id_rsa.pub``.

If you plan to create a load balancer and use HTTPs for the listener, an SSL certificate is required. You can manage certificates with [IBM Certificate Manager](https://{DomainName}/catalog/services/certificate-manager){: external}. You must also create an authorization to allow your load balancer instance to access the Certificate Manager instance that contains the SSL certificate. You can create an authorization through [Identity and Access Authorizations](https://{DomainName}/iam/authorizations){: external}. For the source, select **VPC Infrastructure** as the Source service, **Load Balancer for VPC** as the Resource type, and **All resource instances** for the Source resource instance. Select **Certificate Manager** as the Target service and assign **Writer** for the service access role. Set the Target service instance to **All instances** or to your specific Certificate Manager instance. For more information, see [Using Load Balancers in IBM Cloud VPC](/docs/vpc?topic=vpc-load-balancers).

## Creating a VPC and subnet
{: #creating-a-vpc-and-subnet}

To create a VPC and subnet:

1. Open [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}).

1. Click **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Network > VPCs** and click **New virtual private cloud**.

    Make sure that you're creating a VPC with generation 2 compute resources.
    {: important}

1. Enter a name for the VPC, such as `my-vpc`.
1. Select a resource group for the VPC. Use resource groups to organize your account resources for access control and billing purposes. For more information, see [Best practices for organizing resources in a resource group](/docs/resources?topic=resources-bp_resourcegroups).
1. _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/resources?topic=resources-tag).
1. Select whether the default security group allows inbound SSH and ping traffic to virtual server instances in this VPC. We'll configure more rules for the default security group later.
1. Enter a name for the new subnet in your VPC, such as `my-subnet`.
1. Select a resource group for the subnet.
1. Select a location for the subnet. The location consists of a region and a zone.

    The region that you select is used as the region of the VPC. All additional resources that you create in this VPC are created in the selected region.
    {: tip}
1. Enter an IP range for the subnet in CIDR notation, for example: `10.240.0.0/24`. In most cases, you can use the default IP range. If you want to specify a custom IP range, you can use the IP range calculator to select a different address prefix or change the number of addresses.
1. Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet.  

    You can also attach the public gateway after you create the subnet.
    {: tip}

1. Click **Create virtual private cloud**.
1. To create another subnet in this VPC, click the **Subnets** tab and click **New subnet**. When you define the subnet, make sure to select `my_vpc` in the **Virtual private cloud** field.

## Creating a virtual server instance
{: #creating-a-vsi}

To create a virtual server instance in the newly created subnet:

1. Click **Compute > Virtual server instances** in the navigation pane and click **New instance**.
1. Enter a name for the instance, such as `my-instance`.
1. Select the VPC that you created.
1. Select a resource group for the instance.
1. _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/resources?topic=resources-tag).
1. In the **Location** field, select the zone in which to create the instance.
1. To set the instance size, select one of the popular profiles or click **All profiles** to choose a different core, RAM, and network performance combination that's most appropriate for your workload.
  
    After you create your instance, you can't update the profile.
    {: important}
  
1. Select an existing SSH key or add an SSH key that will be used to access the virtual server instance. To add an SSH key, click **New key** and name the key. After you enter your previously generated public key value, click **Add SSH key**.
1. _Optional:_ Enter user data to run common configuration tasks when your instance starts. For example, you can specify cloud-init directives or shell scripts for Linux images. For more information, see [User Data](/docs//vpc?topic=vpc-user-data).
1. Note the boot volume. In the current release, 100 GB is allotted for the boot volume. *Auto Delete* is enabled for the volume; the boot volume is deleted automatically if the instance is deleted.
1. Select an image (that is, operating system and version) such as Debian GNU/Linux 9.x Stretch/Stable.
1. In the **Attached block storage volume** area, click **New block storage volume** to attach a block storage volume to your instance if you want more storage. In this tutorial, we'll create a block storage volume and attach it to the instance later.
1. In the **Network interfaces** area, you can edit the network interface and change its name. If you have more than one subnet in the selected zone and VPC, you can attach a different subnet to the interface. If you want the instance to exist in multiple subnets, you can create more interfaces. 

  <!-- Each interface has a maximum network bandwidth of 16 Gbps. If the profile you selected for this instance has a maximum network bandwidth greater than 16 Gbps, you might want to create more interfaces to optimize network performance. For more information, see [Network performance notes for profiles](/docs//vpc?topic=vpc-profiles#network-perf-notes-for-profiles).
  {: tip} -->

  You can also select which security groups to attach to each interface. By default, the VPC's default security group is attached. The default security group allows inbound SSH and ping traffic, all outbound traffic, and all traffic between instances in the group. All other traffic is blocked; you can configure rules to allow more traffic. If you later edit the rules of the default security group, those updated rules will apply to all current and future instances in the group.
1. Click **Create virtual server instance**. The status of the instance starts as *Pending*, changes to *Stopped*, and then *Powered On*. You might need to refresh the page to see the change in status.

## Creating and attaching a block storage volume
{: #creating-a-block-storage-volume}

You can create a block storage volume and attach it to your virtual server instance if you want more storage.

To create and attach a block storage volume:

1. In the navigation pane, click **Storage > Block storage volumes**.
1. Click **New volume** and specify the following information.
  * **Name**: Enter a name for the block storage volume, such as `data-volume-1`.
  * **Resource group**: Select a resource group for the block storage volume. You can use resource groups to organize your account resources for access control and billing purposes. For more information, see [Best practices for organizing resources in a resource group](/docs/resources?topic=resources-bp_resourcegroups).
  * **Tags**: _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/resources?topic=resources-tag).
  * **Location**: Select a location for the block storage volume. The location consists of a region and a zone, for example US South 1.
  * **Size**: Specify the size of the volume between 10 GBs and 2000 GBs.
  * **IOPs**: Select one of the IOPs Tiers or click Custom to enter an IOPs value based on volume size.
  * **Encryption**: For this release, accept the default *Provider managed* encryption option.
1. Click **Create volume**.
1. In the list of block storage volumes, find the volume that you created. When the status is Available, click "..." and select **Attach to instance**.
1. Select the instance to which you want to attach the volume and click **Attach**.

## Configuring the security group for the instance
{: #configuring-the-security-group}

You can configure the security group to define the inbound and outbound traffic that is allowed for the instance.

To configure the security group:

1. In the navigation pane, click **Compute > Virtual server instances**.
1. Click your instance to view its details.
1. In the **Network interfaces** section, click the security group.
1. Click **Add rule** to configure inbound and outbound rules that define what type of traffic is allowed to and from the instance. For each rule, specify the following information:  
   * Specify a CIDR block or IP address for the permitted traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to or from all instances that are attached to the selected security group.   
   * Select the protocols and ports to which the rule applies.    

   **Tips:**  
  * All rules are evaluated, regardless of the order in which they're added.
  * Rules are stateful, which means that return traffic in response to allowed traffic is automatically permitted. For example, you created a rule that allows inbound TCP traffic on port 80. That rule also allows replying outbound TCP traffic on port 80 back to the originating host, without the need for another rule.
1. _Optional:_ To view interfaces that are attached to the security group, click **Attached interfaces** in the navigation pane.
1. When you finish creating rules, click the **All security groups for VPC** breadcrumb at the beginning of the page.

### Example security group
{: #example-security-group}

For example, you can configure the following inbound rules:

 * Allow all SSH traffic (TCP port 22)
 * Allow all ping traffic (ICMP type 8)

| Protocol | Source Type | Source | Value |
|-----------|------|------|------|------------------|-------|
| TCP| Any | 0.0.0.0/0 | Ports 22-22
| ICMP | Any | 0.0.0.0/0 | Type: 8, Code: Any|
{: caption="Table 3. Configuration information for inbound rules" caption-side="top"}

Then, configure outbound rules that allow all TCP traffic:

| Protocol | Destination Type | Source | Value |
|-----------|------|------|------|------------------|-------|
| TCP| Any | 0.0.0.0/0 | Any port|
{: caption="Table 4. Configuration information for outbound rules" caption-side="top"}

## Reserving a floating IP address
{: #reserving-a-floating-ip-address}

Reserve and associate a floating IP address if you want your instance to be reachable from the internet.  

Your instance must be running before you can associate a floating IP address. It can take a few minutes for the instance to be up and running.
{: tip}

To reserve and associate a floating IP address:

1. In the navigation pane, click **Compute > Virtual server instances**.
1. Click your instance to view its details.
1. In the **Network interfaces** section, click **Reserve** for the interface that you want to associate with a floating IP address.

If you later want to reassign this floating IP address to another instance in the same zone, find the floating IP address on the **Network > Floating IPs** page, click its overflow menu (**...**), and click **Unassociate**. Then, click **Associate** to select the instance and network interface that you want to associate with the floating IP address.
{: tip}

## Connecting to your instance
{: #connecting-to-your-instance} 

Using the floating IP address that you created, ping your instance to make sure it's up and running:

```
ping <public-ip-address>
```
{:pre}

### Connecting to Linux images
{: #connecting-to-linux images}

Since you created your instance with a public SSH key, you can now connect to it directly by using your private key:

```
ssh -i <path-to-private-key-file> root@<public-ip-address>
```
{:pre}

For more information about how to connect to your instance, see [Connecting to Linux instances](/docs/vpc?topic=vpc-vsi_is_connecting_linux).

## Monitoring your instance
{: #monitoring-your-instance}

You can monitor the CPU, volume, memory, and network usage of your instance over time. Metrics are retained in the system for two weeks.

To monitor your instance:

1. Click **Virtual server instance** in the navigation pane.
1. Click the name of your instance.
1. Click **Monitoring** in the navigation pane.

Because the monitoring data is stored in {{site.data.keyword.monitoringlong_notm}}, you must be authenticated to an instance of the Monitoring service in your account. For more information, see [Setting up the monitoring service for VPC](/docs/vpc?topic=vpc-monitoring#setup-monitoring)
{: important}

## Congratulations!
{: #congratulations} 

You've successfully created and configured your VPC by using the IBM Cloud console. You can continue to develop your VPC by adding more instances, subnets, and other resources.
