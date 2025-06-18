---

copyright:
  years: 2018, 2025
lastupdated: "2025-06-18"

keywords:

subcollection: vpc

content-type: tutorial
services: vpc
account-plan: paid
completion-time: 30m

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
1. Create a virtual server instance. By default, a boot volume is attached to the instance. For most virtual server instances, the default boot volume size is 100 GB. The default boot volume size for a z/OS virtual server instance is 250 GB.
1. If you want more storage, create a Block Storage volume and attach it to your instance.
1. To define the inbound and outbound traffic that is allowed for the instance, configure its security group.
1. If you want your instance to be reachable from the internet, reserve and associate a floating IP address.
1. To distribute requests over multiple instances, create a load balancer.
1. To enable your VPC to connect securely to another private network, such as your on-premises network or another VPC, create a virtual private network (VPN).

After you enter data on the provisioning pages, you can click **Get sample API call** to view the sequence of API requests that correspond to your settings. Viewing the API calls is a good way to learn about the API and understand actions and their dependencies.
{: tip}

## Audience
{: #vpc_tutorials_audience}

This tutorial is intended for software developers and system administrators who are creating a VPC environment and deploying an instance for the first time.
{: shortdesc}

## Before you begin
{: #before}

Set up your account to access VPC. Make sure that your account is [upgraded to a paid account](/docs/account?topic=account-accountfaqs#changeacct){: external}.

You can use an existing SSH key, upload a new SSH key, or you can create an SSH key when you create the virtual server instance. For this tutorial, you can either select an existing key or create a new key when you create the virtual server. For more information about uploading an SSH key or creeating an SSH key in {{site.data.keyword.vpc_short}}, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys).

If you plan to create an [application load balancer](/docs/vpc?topic=vpc-load-balancers) and use HTTPs for the listener, an SSL certificate is required. You can manage certificates with [IBM Secrets Manager](/docs/secrets-manager?topic=secrets-manager-getting-started).

{{site.data.content.load-balancer-grant-service-auth}}

Allow extra time for completing any optional steps.
{: note}

## Creating a VPC and subnet
{: #creating-a-vpc-and-subnet}
{: help}
{: support}
{: step}

To create a VPC and subnet follow these steps:

1. Open [{{site.data.keyword.cloud_notm}} console](/login){: external}.
1. Click **Navigation menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Network > VPCs** and click **Create**.
1. Enter a name for the VPC, such as `my-vpc`.
1. Select a resource group for the VPC. Use resource groups to organize your account resources for access control and billing purposes. For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).
1. _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
1. The create VPC process assigns a default ACL. Later in this tutorial, you can configure rules for the ACL.
1. Select whether the default security group allows inbound SSH and ping traffic to virtual server instances in this VPC. By default the **Allow SSH** and **Allow ping** options for the default security group are not selected. We can configure more rules for the default security group later.
1. _Optional:_ Select whether you want to enable your VPC to access classic infrastructure resources. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

    You can enable a VPC for classic access only while you're creating the VPC. In addition, you can have only one classic access VPC in your account at any time.
    {: important}

1. _Optional:_ Clear the **Default address prefixes** option if you don't want to assign default address prefixes to each zone in your VPC. After you create your VPC, you can go to its details page and set your own address prefixes.
1. By default the create VPC process defines three subnets. If you need to edit the properties that are defined for the subnet, click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") for the subnet that you want to edit. You can also remove a subnet that is pre-defined by clicking the minus icon. If you need to add a subnet, complete the following steps.
1. Click **Add subnet** and enter a name for the new subnet in your VPC, such as `my-subnet`.
1. Select a location for the subnet. The location consists of a region and a zone. The region that you select is used as the region of the VPC. All extra resources that you create in this VPC are created in the selected region.
1. Select a resource group for the subnet.
1. Enter an IP range for the subnet in CIDR notation, for example: `10.240.0.0/24`. In most cases, you can use the default IP range. If you want to specify a custom IP range, you can use the IP range calculator to select a different address prefix or change the number of addresses.

    A subnet cannot be resized after it is created.
    {: important}

1. Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet. You can also attach the public gateway after you create the subnet.

1. Click **Create virtual private cloud**.

## Configuring the ACL
{: #configuring-the-acl-ct}
{: step}

You can configure the ACL to limit inbound and outbound traffic to the subnet. By default, all traffic is allowed.

Each subnet can be attached to only one ACL. However, each ACL can be attached to multiple subnets.

To configure the ACL, follow these steps:

1. In the navigation pane, click **Network > Subnets**.
1. Click the subnet that you created.
1. In the **Subnet details** area, click the name of the ACL.
1. Click **Add rule** to configure inbound and outbound rules that define what traffic is allowed in or out of the subnet. For each rule, specify the following information:
   * Select whether to allow or deny the specified traffic.
   * Select the protocol to which the rule applies.
   * For the source and destination of the rule, specify the IP range and ports for which the rule applies. For example, if you want all inbound traffic to be allowed to the IP range `192.168.0.0/24` in your subnet, specify **Any** as the source and `192.168.0.0/24` as the destination. But if you want to allow inbound traffic only from `169.168.0.0/24` to your entire subnet, specify `169.168.0.0/24` as the source and **Any** as the destination for the rule.
   * Specify the rule's priority. Rules with smaller numbers are evaluated first and override rules with higher numbers. For example, if a rule with priority 2 allows HTTP traffic and a rule with priority 5 denies all traffic, HTTP traffic is still allowed.

   See [Configuring rules](/docs/vpc?topic=vpc-acl-create-ui#example-configuring-rules) for an example.
   {: tip}

1. When you finish creating rules, click the **Access control lists** breadcrumb at the beginning of the page.

For more information, see [About access control lists](/docs/vpc?topic=vpc-using-acls).

## Creating a virtual server instance
{: #creating-a-vsi}
{: help}
{: support}
{: step}

To create a virtual server instance in the newly created subnet, use these steps:

1. Click **Compute > Virtual server instances** in the navigation pane and click **Create**.
1. For the **Location**, select the geography, region, and zone in which to create the instance.
1. Enter a name for the instance, such as `my-instance`.
1. Select a resource group for the instance.
1. _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
1. Click **Change image** to select an image (that is, operating system and version), such as Debian GNU/Linux, _ibm-debian-11-7-minimal-amd64-2_. On the Select an image page, you can select a stock image, custom image, catalog image, snapshot, or an existing volume. If the geographic location where you are provisioning an instance supports it, you have the option to select the architecture for your virtual server, either _x86_ or _s390x_.

    _For z/OS Wazi aaS custom image only:_ If you use the custom image that is created by using Wazi Image Builder, select **Custom image** for the operating system and the image is called `wazi-custom-image` by default.
    {: note}

1. To set the instance size, click **Change profile** to choose a profile that contains a combination of core, RAM, and network performance that's most appropriate for your workload. For more information, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles) and [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles).

    _For z/OS virtual server instances only:_ z/OS virtual server instances require a minimum profile of 2 vCPUs x 16 GB RAM (2x16). When you select the profile for any z/OS stock images with RAM smaller than 8 GB, you might encounter the `IAR057D` message. For more information, see [IAR057D](https://www.ibm.com/docs/en/zos/3.1.0?topic=messages-iar057d){: external}.
    {: note}

1. Select an existing SSH key or create an SSH key that is to be used to access the virtual server instance. To create an SSH key, click **Create an SSH key** and name the key. Select **Generate a key pair for me**, click **Save private key**, then **Save public key**. When this action completes, click **Create**.
1. Note the boot volume. _Auto Delete_ is enabled for the volume; the boot volume is deleted automatically if the instance is deleted.
1. In the **Data volumes** area, click **Create** to attach a Block Storage volume to your instance if you want more storage. In this tutorial, you create a Block Storage volume and attach it to the instance later.

    _For z/OS Wazi aaS custom image only:_ When you create a z/OS virtual server instance with the z/OS Wazi aaS custom image, you need to add a data volume by selecting `Import from Snapshot`. The snapshot is called `wazi-custom-image-data` by default.
    {: note}

1. For the Virtual Private Cloud in the **Networking** section, select the VPC that you created.

1. In the **Network interfaces** area, you can edit the network interface and change its name. If you have more than one subnet in the selected zone and VPC, you can attach a different subnet to the interface. If you want the instance to exist in multiple subnets, you can create more interfaces.

   Each interface has a maximum network bandwidth of 16 Gbps. If the profile you selected for this instance has a maximum network bandwidth greater than 16 Gbps, you might want to create more interfaces to optimize network performance. For more information, see [Managing network interfaces](/docs/vpc?topic=vpc-using-instance-vnics).
   {: tip}

   You can also select which security groups to attach to each interface. By default, the VPC's default security group is attached. The default security group allows inbound SSH and ping traffic, all outbound traffic, and all traffic between instances in the group. All other traffic is blocked; you can configure rules to allow more traffic. If you later edit the rules of the default security group, those updated rules will apply to all current and future instances in the group.

1. _Optional:_ In the Advanced options section, you can enter **User data** to run common configuration tasks when your instance starts. For example, you can specify cloud-init directives or shell scripts for Linux images. For more information, see [User Data](/docs/vpc?topic=vpc-user-data).

1. The **Metadata** setting is disabled by default. When the metadata service is enabled, the instance collects instance configuration information and user data. For more information, see [About instance metadata for VPC](/docs/vpc?topic=vpc-imd-about). Metadata isn't supported for {{site.data.keyword.cloud}} Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instances and z/OS virtual server instances.

1. The **Add to dedicated host** setting is disabled by default. For more information about dedicated hosts, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances).

1. The **Add to placement group** setting is disabled by default. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc).

1. The **Host failure auto restart** setting is enabled by default. To disable host failure auto restart, click the toggle. For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=ui).

1. Click **Create virtual server instance**. The status of the instance starts as _Pending_, changes to _Stopped_, and then _Running_. You might need to refresh the page to see the change in status.

## Creating and attaching a Block Storage volume
{: #creating-a-block-storage-volume}
{: help}
{: support}
{: step}

You can create a Block Storage volume and attach it to your virtual server instance if you want more storage.

To create and attach a Block Storage volume, follow these steps:

1. In the navigation pane, click **Storage > Block storage volumes**.
1. Click **New volume** and specify the following information.
   * **Name**: Enter a name for the Block Storage volume, such as `data-volume-1`.
   * **Resource group**: Select a resource group for the Block Storage volume. You can use resource groups to organize your account resources for access control and billing purposes. For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).
   * **Tags**: _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Location**: Select a location for the Block Storage volume. The location consists of a region and a zone, for example US South 1.
   * **Size**: Specify the size of the volume between 10 GBs and 2000 GBs.
   * **IOPS**: Select one of the IOPS Tiers or click Custom to enter an IOPS value based on volume size.
   * **Encryption**: Accept the default _Provider managed_ encryption option.
1. Click **Create block storage volume**.
1. In the list of Block Storage volumes, find the volume that you created. When the status is Available, click "..." and select **Attach to instance**.
1. Select the instance to which you want to attach the volume and click **Attach**.
1. _For z/OS virtual server instances only:_ To verify the newly attached Block Storage volume with its address assigned, you can find the information on your z/OS virtual server instance console through a broadcast message that is sent to you with the affected device address.


## Configuring the security group for the instance
{: #configuring-the-security-group-step}
{: step}

You can configure the security group to define the inbound and outbound traffic that is allowed for the instance. For example, after you configure ACL rules for the subnet based on your company's security policies, depending on their workloads, you can further restrict traffic for specific instances.

To configure the security group, follow these steps:

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
   * If your z/OS virtual server instance is created by using the z/OS dev and test stock image, you can refer to the [Reserved configurations](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=vpc-configurations-in-zos-stock-images){: external} table when adding additional ports to the security group of your z/OS virtual server instance.

1. _Optional:_ To view interfaces that are attached to the security group, click **Attached resources** tab and review the Attached interfaces section.
1. When you finish creating rules, click the **Security groups** breadcrumb at the beginning of the page.

For more information about security groups, see [About security groups](/docs/vpc?topic=vpc-using-security-groups).

## Reserving a floating IP address (optional)
{: #reserving-a-floating-ip-address}
{: step}

Reserve and associate a floating IP address if you want your instance to be reachable from the internet.

Your instance must be running before you can associate a floating IP address. It can take a few minutes for the instance to be up and running.
{: tip}

To reserve and associate a floating IP address, following these steps:

1. In the navigation pane, click **Compute > Virtual server instances**.
1. Click your instance to view its details.
1. In the **Network interfaces** section, click **Reserve** for the interface that you want to associate with a floating IP address.

You can later reassign this floating IP address to another instance in the same zone. To reassign this floating IP address, you can find the floating IP address on the **Network > Floating IPs** page, click its overflow menu (**...**), and click **Unassociate**. Then, click **Associate** to select the instance and network interface that you want to associate with the floating IP address.

## Create a public gateway (optional)
{: #creating-a-public-gateway}
{: step}

Create a public gateway and connect your virtual server instance if you want multiple instances to be reachable from the internet.

To create a public gateway:
1. Go to the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public gateways**. The Public gateways for VPC dashboard appear.
1. Click **Create** to go to the **Create public gateway** menu.

A public gateway provides virtual server instances only outbound connectivity, whereas a floating IP address provides virtual server instances outbound and inbound connectivity. A floating IP address exposes a service on the internet to inbound activity.

For more information about how to create a public gateway, see [Creating Public Gateways](/docs/vpc?topic=vpc-create-public-gateways&interface=ui).

## Connecting to your instance
{: #connecting-to-your-instance}
{: step}

Using the floating IP address that you created, ping your instance to make sure that it's up and running:

```sh
ping <public-ip-address>
```
{: pre}

### Connecting to Linux images
{: #connecting-to-linux-images}

Because you created your instance with a public SSH key, you can now connect to it directly by using your private key:

```sh
ssh -i <path-to-private-key-file> root@<public-ip-address>
```
{: pre}

The user login 'root' is disabled by default in Fedora Core OS. The user login 'core' can be used to log in to Fedora Core OS instances.
{: note}

For more information about connecting to your instance, see [Connecting to Linux instances](/docs/vpc?topic=vpc-vsi_is_connecting_linux).

To connect to a Windows image, log in using its decrypted password. For more information, see [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).

To connect to your z/OS virtual server instance, see [Connecting to z/OS virtual server instances](/docs/vpc?topic=vpc-vsi_is_connecting_zos).

You can now access your {{site.data.keyword.cloud}} virtual server instance by connecting to a VNC or serial console, which is a quick-and-easy way for you to interact with the instance without using a Secure Shell. However, accessing your z/OS virtual server instance by using a VNC console on the IBM Cloud UI is not supported. For more information about this feature, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console).

## Monitoring your instance
{: #monitoring-your-instance}
{: step}

You can monitor the CPU, volume, memory, and network usage of your instance over time. Metrics are retained in the system for two weeks.

To monitor your instance, follow these steps:

1. Click **Virtual server instance** in the navigation pane.
1. Click the name of your instance.
1. Click **Monitoring** in the navigation pane.

Because the monitoring data is stored in {{site.data.keyword.mon_full_notm}}, you must be authenticated to an instance of {{site.data.keyword.mon_full_notm}} in your account. For more information, see [IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-getting-started).
{: important}

## Creating a load balancer (optional)
{: #create-load-balancer}
{: step}

You can create two different types of {{site.data.keyword.cloud_notm}} load balancers: an application load balancer and a network load balancer. For more information about creating an {{site.data.keyword.cloud_notm}} load balancer, see [Load balancers overview](/docs/vpc?topic=vpc-nlb-vs-elb).

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

You successfully created and configured your VPC by using the IBM Cloud console. You can continue to develop your VPC by adding more instances, subnets, and other resources.
