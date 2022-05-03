---

copyright:
  years: 2018, 2022

lastupdated: "2022-05-03"


keywords: vpc, virtual private cloud, vpc ui, console, access control list, virtual server instance, subnet, block storage volume, security group, images, monitoring, ssh key, ip range

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Using the {{site.data.keyword.cloud_notm}} console to create VPC resources
{: #creating-a-vpc-using-the-ibm-cloud-console}
{: toc-content-type="tutorial"}
{: toc-services="vpc"}
{: toc-completion-time="30m"}

You can create and configure an {{site.data.keyword.vpc_full}} (VPC) by using the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

## Objectives
{: #vpc_tutorials_objectives}
To create and configure your VPC and other attached resources, do the steps in the sections that follow, in this order:

1. Create a VPC and subnet to define the network. When you create your subnet, attach a public gateway if you want to allow all resources in the subnet to communicate with the public internet.
1. To limit the subnet's inbound and outbound traffic, you can configure an access control list (ACL). By default, all traffic is allowed.
1. Create a virtual server instance. By default, a 100 GB boot volume is attached to the instance.
1. If you want more storage, create a block storage volume and attach it to your instance.
1. To define the inbound and outbound traffic that is allowed for the instance, configure its security group.
1. If you want your instance to be reachable from the internet, reserve and associate a floating IP address.
1. To distribute requests over multiple instances, create a load balancer.
1. To enable your VPC to connect securely to another private network, such as your on-premises network or another VPC, create a virtual private network (VPN).

After you enter data on the provisioning pages, you can click the **Get sample API call** button to view the sequence of API requests that correspond to your settings. Viewing the API calls is a good way to learn about the API and understand actions and their dependencies.
{: tip}

## Audience
{: #vpc_tutorials_audience}
This tutorial is intended for software developers and system administrators who are creating a VPC environment and deploying an instance for the first time.
{: shortdesc}

## Before you begin
{: #before}

Set up your account to access VPC. Make sure that your account is [upgraded to a paid account](/docs/account?topic=account-accountfaqs#changeacct){: new_window}.

Make sure that you have an SSH key. The key is used to connect to the virtual server instance. For example, generate an SSH key on your Linux server by running the following command:
```
ssh-keygen -t rsa -C "user_ID"
```
{: pre}

This command generates two files. The generated public key is in the `id_rsa.pub` file under an ``.ssh`` directory in your home directory, for example, ``/Users/<USERNAME>/.ssh/id_rsa.pub``. For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys).

If you plan to create an [application load balancer](/docs/vpc?topic=vpc-load-balancers) and use HTTPs for the listener, an SSL certificate is required. You can manage certificates with [IBM Certificate Manager](https://{DomainName}/catalog/services/certificate-manager){: external}. 

{{site.data.content.load-balancer-grant-service-auth}} 

Allow extra time for completing any optional steps.
{: note}

## Creating a VPC and subnet
{: #creating-a-vpc-and-subnet}
{: help}
{: support}
{: step}

To create a VPC and subnet:

1. Open [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}).
1. Click **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Network > VPCs** and click **Create**.
1. Enter a name for the VPC, such as `my-vpc`.
1. Select a resource group for the VPC. Use resource groups to organize your account resources for access control and billing purposes. For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).
1. _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
1. Select the region where you want the VPC created. 
1. The create VPC process assigns a default ACL. Later in this tutorial we'll configure rules for the ACL.
1. Select whether the default security group allows inbound SSH and ping traffic to virtual server instances in this VPC. By default both types of traffic are allowed. We'll configure more rules for the default security group later.
1. _Optional:_ Select whether you want to enable your VPC to access classic infrastructure resources. By default classic access is not enabled. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

    You can only enable a VPC for classic access while creating the VPC. In addition, you can only have one classic access VPC in your account at any time.
    {: important}

1. _Optional:_ Clear the **Default address prefixes** option if you don't want to assign default address prefixes to each zone in your VPC. After you create your VPC, you can go to its details page and set your own address prefixes.
1. By default the create VPC process defines three subnets. If you need to edit the properties that are defined for the subnet, click the pencil icon for the subnet that you want to edit. You can also remove a subnet that is pre-defined by clicking the minus icon. If you need to add a subnet, complete the following steps. 
    1. Click **Add subnet** and enter a name for the new subnet in your VPC, such as `my-subnet`.
    1. Select a location for the subnet. The location consists of a region and a zone.

        The region that you select is used as the region of the VPC. All additional resources that you create in this VPC are created in the selected region.
        {: tip}
    
    1. Select a resource group for the subnet.
    1. The most efficient location for your IP range (CIDR block) is calculated to maximize available IP addresses. You can customize the IP range by selecting a different address prefix, changing the number of addresses, or by entering your IP range manually.

        A subnet cannot be resized after it is created.
        {: important}

    1. Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet.  

        You can also attach the public gateway after you create the subnet.
        {: tip}

1. Click **Create virtual private cloud**.

## Configuring the ACL
{: #configuring-the-acl}
{: step}

You can configure the ACL to limit inbound and outbound traffic to the subnet. By default, all traffic is allowed.

Each subnet can be attached to only one ACL. However, each ACL can be attached to multiple subnets.

To configure the ACL:

1. In the navigation pane, click **Network > Subnets**.
1. Click the subnet that you created.
1. In the **Subnet details** area, click the name of the ACL.
1. Click **Create** to configure inbound and outbound rules that define what traffic is allowed in or out of the subnet. For each rule, specify the following information:
   * Select whether to allow or deny the specified traffic.
   * Select the protocol to which the rule applies.  
   * For the source and destination of the rule, specify the IP range and ports for which the rule applies. For example, if you want all inbound traffic to be allowed to the IP range 192.168.0.0/24 in your subnet, specify **Any** as the source and 192.168.0.0/24 as the destination. But if you want to allow inbound traffic only from 169.168.0.0/24 to your entire subnet, specify 169.168.0.0/24 as the source and **Any** as the destination for the rule.
   * Specify the rule's priority. Rules with lower numbers are evaluated first and override rules with higher numbers. For example, if a rule with priority 2 allows HTTP traffic and a rule with priority 5 denies all traffic, HTTP traffic is still allowed.  

   See [Configuring rules](/docs/vpc?topic=vpc-acl-create-ui#example-configuring-rules) for an example.
   {: note}

1. When you finish creating rules, click the **Access control lists** breadcrumb at the beginning of the page.

For more information, see [About access control lists](/docs/vpc?topic=vpc-using-acls).

## Creating a virtual server instance
{: #creating-a-vsi}
{: help}
{: support}
{: step}

To create a virtual server instance in the newly created subnet:

1. Click **Compute > Virtual server instances** in the navigation pane and click **Create**.
1. Enter a name for the instance, such as `my-instance`.
1. Select a resource group for the instance.
1. _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
1. In the **Location** field, select the zone in which to create the instance.
1. Select the type of virtual server that you want to create. By default, Public is selected. 
1. Select the processor architecture for the virtual server that you want to create. By default, x86 is selected. 
1. Select an image (that is, operating system and version), such as *Debian GNU/Linux* and *ibm-debian-9-13-minimal-amd64-4*. Alternatively, you can select a snapshot. 
1. To set the instance size, select a profile. If you want to choose something other than the default, click **View all profiles** to choose a different core, RAM, and network performance combination that's most appropriate for your workload. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles).
1. Select an existing SSH key or add an SSH key that is to be used to access the virtual server instance. To add an SSH key, click **Create key** and name the key. After you enter your previously generated public key value, click **Add SSH key**.
1. _Optional:_ Enter user data to run common configuration tasks when your instance starts. For example, you can specify cloud-init directives or shell scripts for Linux images. For more information, see [User Data](/docs/vpc?topic=vpc-user-data).
1. _Optional:_ If you want the virtual server instance to be included in a placement group, select **Add instance to placement group**. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc).
1. Note the boot volume. In the current release, 100 GB is allotted for the boot volume. *Auto Delete* is enabled for the volume; the boot volume is deleted automatically if the instance is deleted.
1. In the **Data volumes** area, click **Create** to attach a block storage volume to your instance if you want more storage. In this tutorial, we'll create a block storage volume and attach it to the instance later.
1. In the **Networking** area, select the VPC that you created.
1. In the **Network interfaces** area, you can edit the network interface and change its name. If you have more than one subnet in the selected zone and VPC, you can attach a different subnet to the interface. If you want the instance to exist in multiple subnets, you can create more interfaces.

   Each interface has a maximum network bandwidth of 16 Gbps. If the profile you selected for this instance has a maximum network bandwidth greater than 16 Gbps, you might want to create more interfaces to optimize network performance. For more information, see [Network performance notes for profiles](/docs/vpc?topic=vpc-profiles#network-perf-notes-for-profiles).
   {: tip}

   You can also select which security groups to attach to each interface. By default, the VPC's default security group is attached. The default security group allows inbound SSH and ping traffic, all outbound traffic, and all traffic between instances in the group. All other traffic is blocked; you can configure rules to allow more traffic. If you later edit the rules of the default security group, those updated rules will apply to all current and future instances in the group.
1. Click **Create virtual server instance**. The status of the instance starts as *Pending*, changes to *Stopped*, and then *Running*. You might need to refresh the page to see the change in status.

## Creating and attaching a block storage volume
{: #creating-a-block-storage-volume}
{: help}
{: support}
{: step}

You can create a block storage volume and attach it to your virtual server instance if you want more storage.

To create and attach a block storage volume:

1. In the navigation pane, click **Storage > Block storage volumes**.
1. Click **New volume** and specify the following information.
   * **Name**: Enter a name for the block storage volume, such as `data-volume-1`.
   * **Resource group**: Select a resource group for the block storage volume. You can use resource groups to organize your account resources for access control and billing purposes. For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).
   * **Tags**: _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Location**: Select a location for the block storage volume. The location consists of a region and a zone, for example US South 1.
   * **Size**: Specify the size of the volume between 10 GBs and 2000 GBs.
   * **IOPs**: Select one of the IOPs Tiers or click Custom to enter an IOPs value based on volume size.
   * **Encryption**: Accept the default *Provider managed* encryption option.
1. Click **Create volume**.
1. In the list of block storage volumes, find the volume that you created. When the status is Available, click "..." and select **Attach to instance**.
1. Select the instance to which you want to attach the volume and click **Attach**.

## Configuring the security group for the instance
{: #configuring-the-security-group}
{: step}

You can configure the security group to define the inbound and outbound traffic that is allowed for the instance. For example, after you configure ACL rules for the subnet based on your company's security policies, you can further restrict traffic for specific instances depending on their workloads.

To configure the security group:

1. In the navigation pane, click **Compute > Virtual server instances**.
1. Click your instance to view its details.
1. In the **Network interfaces** section, click the name of the security group.
1. On the **Rules** tab of the security group details page, click **Create** to configure inbound and outbound rules that define what type of traffic is allowed to and from the instance. For each rule, specify the following information:  
   * Select the protocols and ports to which the rule applies.    
   * Specify a CIDR block or IP address for the permitted traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to or from all instances that are attached to the selected security group. 

   **Tips:**  
   * All rules are evaluated, regardless of the order in which they're added.
   * Rules are stateful, which means that return traffic in response to allowed traffic is automatically permitted. For example, you created a rule that allows inbound TCP traffic on port 80. That rule also allows replying outbound TCP traffic on port 80 back to the originating host, without the need for another rule.
   * For Windows images, make sure that the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).
1. _Optional:_ To view interfaces that are attached to the security group, click **Attached resources** tab and review the Attached interfaces section.
1. When you finish creating rules, click the **Security groups** breadcrumb at the beginning of the page.

   For more information about security groups, see [About security groups](/docs/vpc?topic=vpc-using-security-groups).
   {: note}

## Reserving a floating IP address
{: #reserving-a-floating-ip-address}
{: step}

Reserve and associate a floating IP address if you want your instance to be reachable from the internet.

Your instance must be running before you can associate a floating IP address. It can take a few minutes for the instance to be up and running.
{: tip}

To reserve and associate a floating IP address:

1. In the navigation pane, click **Compute > Virtual server instances**.
1. Click your instance to view its details.
1. In the **Network interfaces** section, click **Reserve** for the interface that you want to associate with a floating IP address.

You can later reassign this floating IP address to another instance in the same zone. To do this, you can find the floating IP address on the **Network > Floating IPs** page, click its overflow menu (**...**), and click **Unassociate**. Then, click **Associate** to select the instance and network interface that you want to associate with the floating IP address.
{: tip}

## Connecting to your instance
{: #connecting-to-your-instance}
{: step}

Using the floating IP address that you created, ping your instance to make sure it's up and running:

```sh
ping <public-ip-address>
```
{: pre}

### Connecting to Linux images
{: #connecting-to-linux images}

Since you created your instance with a public SSH key, you can now connect to it directly by using your private key:

```sh
ssh -i <path-to-private-key-file> root@<public-ip-address>
```
{: pre}

For more information about how to connect to your instance, see [Connecting to Linux instances](/docs/vpc?topic=vpc-vsi_is_connecting_linux).

To connect to a Windows image, log in using its decrypted password. For instructions, see [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).

You can now access your IBM Cloud virtual server instance by connecting to a VNC or serial console. This is a quick-and-easy way for you to interact with the instance without using a Secure Shell. For more information about this feature, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console)
{: note}

## Monitoring your instance
{: #monitoring-your-instance}
{: step}

You can monitor the CPU, volume, memory, and network usage of your instance over time. Metrics are retained in the system for two weeks.

To monitor your instance:

1. Click **Virtual server instance** in the navigation pane.
1. Click the name of your instance.
1. On the virtual server instance details page, click the **Monitoring** tab.

Because the monitoring data is stored in {{site.data.keyword.mon_full_notm}}, you must be authenticated to an instance of {{site.data.keyword.mon_full_notm}} in your account. For more information, see [IBM Cloud monitoring](/docs/cloud-infrastructure?topic=cloud-infrastructure-monitoring-iaas).
{: important}

## Creating a load balancer (optional)
{: #tutorial-load-balancer}
{: step}

You can create two different types of {{site.data.keyword.cloud_notm}} load balancers: an application load balancer and a network load balancer. For comparison information and instructions on how to create an  {{site.data.keyword.cloud_notm}} load balancer, see [Load balancers overview](/docs/vpc?topic=vpc-nlb-vs-elb).

## Creating a VPN gateway (optional)
{: #vpn-ui}
{: step}

You can create a virtual private network (VPN) so your VPC can connect securely to another private network, such as an on-premises network or another VPC. For more information, see [Creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway).

## Viewing resources associated with a VPC
{: #vpc-layout}
{: step}

You can quickly view the resources that are associated with a VPC by accessing the resource view: In the navigation, click **VPC layout**. You can select the VPC that you are interested in, if multiple VPCs are configured for your account. For each VPC you can see the associated subnets, and within each subnet you can see how many instances are running, stopped, and failed. You can also see how many IP addresses are available in each subnet. From the lists of instances and associated IP addresses, you can click a specific instance to view its details.

## Congratulations!
{: #congratulations}

You've successfully created and configured your VPC by using the IBM Cloud console. You can continue to develop your VPC by adding more instances, subnets, and other resources.
