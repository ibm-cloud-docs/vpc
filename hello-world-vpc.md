---

copyright:
  years: 2019, 2020
lastupdated: "2020-08-27"

keywords: cli, command line interface, tutorial, creating a vpc

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}
{:download: .download}

# Using the CLI to create VPC resources
{: #creating-a-vpc-using-cli}

You can create and configure an {{site.data.keyword.vpc_full}} using the {{site.data.keyword.cloud}} CLI.
{:shortdesc}

To create and configure your virtual private cloud (VPC) and other attached resources, perform the steps in the sections that follow, in this order:

1. Create a VPC and subnet to define the network.
1. If you want to allow all resources in the subnet to communicate with the public internet, attach a public gateway.
1. Create a virtual server instance. By default, a 100 GB boot volume is attached to the instance.
1. If you want more storage, create a block storage volume and attach it to your instance.
1. To define the inbound and outbound traffic that's allowed for the instance, configure its security group.
1. If you want your instance to be reachable from the internet, reserve and associate a floating IP address.

## Before you begin
{: #before-cli-tutorial}
Make sure that you set up your [environment](/docs/vpc?topic=vpc-set-up-environment).

## Log in to IBM Cloud
{: #log-in-to-ibm-cloud}

```
ibmcloud login --sso -a cloud.ibm.com
 ```
{: pre}

This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After the authentication steps, you'll be prompted to choose your account. If you have access to multiple users, select the user you want to log in as.

When you are prompted to select a region, select us-south.

Respond to any remaining prompts to finish logging in.

## Select the generation of VPC
{: #select-the-generation }

Use the following command to configure the CLI plug-in to target generation 2 virtual server instances for VPC.

```
ibmcloud is target --gen 2
```
{: pre}

## Create a VPC
{: #create-a-vpc-cli}

Use the following command to create a VPC named _my-vpc_.

```
ibmcloud is vpc-create my-vpc
```
{: pre}

From the output that's returned, save the ID in a variable so you can use it later, for example:

```
vpc="0738-59de4046-3434-4d87-bb29-0c99c428c96e"
```
{: pre}

The previous example does not create a VPC with classic access. If you require the VPC to have access to your classic resources, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure). You can only enable a VPC for classic access while creating it. In addition, you can only have one classic access VPC in your account at any time.
{: important}

## Create a subnet
{: #create-a-subnet-cli}

Before you create a subnet, select the zone and address prefix in which to create it. To list the address prefixes for each zone in your VPC, run the following command:

```
ibmcloud is vpc-address-prefixes $vpc
```
{: pre}

Let's pick the default address prefix for the `us-south-3` zone. From the command output, note the CIDR block of the address prefix. When you create a subnet, you must specify an IP range that's within one of the address prefixes of the selected zone.

A subnet cannot be resized after it is created.
{: important}

```
ibmcloud is subnet-create my-subnet $vpc us-south-3 --ipv4-cidr-block "10.0.1.0/24"
```
{: pre}

From the output that's returned, save the ID in a variable so you can use it later, for example:

```
subnet="0738-658756a4-1106-4914-969a-3b43b338524a"
```
{: pre}

The status of the subnet is `pending` when it's first created. Before you can create resources in the subnet, the subnet needs to move to the `available` status, which takes a few seconds. To check the status of the subnet, run this command:

```
ibmcloud is subnet $subnet
```
{: pre}

## Attach a public gateway
{: #attach-public-gateway-cli}

Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet.

To create a public gateway, run the following command:

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-3
```
{: pre}

From the output that's returned, save the ID in a variable so you can use it later, for example:

```
gateway="0738-446c0c63-f0b1-4043-b30d-644f55fde391"
```
{: pre}

To attach the public gateway to your subnet, run the following command:
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}

Only one public gateway per zone is allowed in a VPC, but that public gateway can be attached to multiple subnets in the zone. To find the public gateway for a zone, run the `ibmcloud is public-gateways` command and look for the particular VPC and Zone values.
{: tip}

## Add an SSH key
{: #add-ssh-key-cli}

Add your public SSH key to your {{site.data.keyword.cloud_notm}} account. This key is specified when you create the instance, and it's needed later to log in to the instance. You can use one key to provision multiple instances.

To see the available keys in your IBM Cloud account, run this command:

```
ibmcloud is keys
```
{: pre}

To add a key, run the following command. Substitute the path to your `id_rsa.pub` file.

```
ibmcloud is key-create my-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

From the output that's returned, save the ID in a variable so you can use it later, for example:

```
key="0738-859b4e97-7540-4337-9c64-384792b85653"
```
{: pre}

## Select a profile and image for the instance
{: #select-a-profile-and-image-for-the-instance}

To list all available instance profiles, run the following command:

```
ibmcloud is instance-profiles
```
{: pre}

To list all available images, run the following command:

```
ibmcloud is images
```
{: pre}

Deprecated images do not include the most current support.
{: tip}

Let's pick instance profile `bx2-2x8` and image `debian-9.x-amd64`. To get the image ID, run the following command:

```
image=$(ibmcloud is images | grep -i "debian.*available.*amd64.*public" | cut -d" " -f1)
```
{: pre}

## Create an instance
{: #create-an-instance}

Create an instance in the newly created subnet. Pass in your public SSH key so that you can log in after the instance is provisioned.

```
ibmcloud is instance-create my-instance $vpc us-south-3 bx2-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

Information about the network interface that is created for the new instance is not returned after the instance is created.
{: note}

From the output that's returned, save the ID of the instance in a variable so you can use it later, for example:

```
instance="0738-21179496-964e-4c00-8210-cf23d75750b3"
```
{:pre}

The status of the instance is `pending` when it's first created. Before you can proceed, the instance needs to move to the `running` status, which takes a few minutes. To check the status of the instance, run this command:

```
ibmcloud is instance $instance
```
{: pre}

From the output that's returned, save the ID of the primary network interface (`Primary Interface`) in a variable so you can use it later, for example:

```
nic="0738-4d9b3a58-f796-4e6a-b5ac-84f4216e9b68-glhvl"
```

## Create a block storage data volume
{: #create-block-storage-data-volume-cli}

You can create a block storage volume and attach it to your virtual server instance if you want more storage. When you create a block storage volume, you select a profile to optimize the performance of your compute workloads. See [Profiles](/docs/vpc?topic=vpc-block-storage-profiles#block-storage-profiles) for information about volume capacity and IOPS ranges based on the volume profile you select.  

To see a list of volume profiles, run:

```
ibmcloud is volume-profiles
```
{: pre}

Run this command to create a block storage data volume. Specify a name for your volume, volume profile, and the zone where you are creating the volume. To attach a block storage data volume to an instance, the instance and the block storage data volume must be created in the same zone.

```
ibmcloud is volume-create my-volume custom us-south-2 --iops 1000
```
{: pre}

From the output that's returned, save the ID of the volume in a variable so you can use it later:

```
vol=0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

The status of the volume is `pending` when it first is created. Before you can proceed, the volume needs to move to `available` status, which takes a few minutes.

To check the status of the volume, run this command:

```
ibmcloud is volume $vol
```
{: pre}

## Attach a block storage data volume to an instance
{: #attach-block-storage-data-volume-cli}

Use the following command to attach the volume to the virtual server instance, by using the variables that we created:

```
ibmcloud is instance-volume-attachment-add my-volume-attachment $instance $vol --auto-delete true
```
{: pre}

## Add rules to the default security group
{: #add-rules-to-default-security-group}

You can configure the security group to define the inbound and outbound traffic that is allowed for the instance. For example, you can add a rule to allow SSH traffic.

Find the security group for the VPC:

```
ibmcloud is vpc-sg $vpc
```
{: pre}

From the output that's returned, save the ID in a variable so you can use it later:

```
sg=0738-2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Now create a rule to allow SSH traffic:

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

Optionally, you can also add a rule to allow ping traffic:

```
ibmcloud is sg-rulec $sg inbound icmp --icmp-type 8 --icmp-code 0
```
{: pre}

For Windows images, make sure the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).
{: tip}

## Create a floating IP address for the instance
{: #create-floating-ip-address-cli}

Create a floating IP address if you want your instance to be reachable from the internet.

```
ibmcloud is floating-ip-reserve my-fip --nic-id $nic
```
{: pre}

From the output that's returned, save the `Address` in a variable so you can use it later:

```
address=169.48.88.0
```
{: pre}

## Log in to your instance
{: #log-in-to-your-instance-using-your-private-ssh-key}

For example, on Linux you can use a command of this form:

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

When you're prompted to continue connecting, type `yes`.

SSH access into the instance might be prevented by security groups. Make sure the instance's security group allows SSH access.
{: tip}

To connect to a Windows image, log in using its decrypted password. For instructions, see [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).

## Monitoring your instance
{: #monitoring-your-instance-cli-tutorial}

You can monitor the CPU, volume, memory, and network usage of your instance over time in the {{site.data.keyword.cloud_notm}} console. Because the monitoring data is stored in {{site.data.keyword.monitoringlong_notm}}, you must be authenticated to an instance of the Monitoring service in your account. For more information, see [Monitoring your instances](/docs/vpc?topic=vpc-monitoring).

## Create a VPN gateway
{: #create-a-vpn-gateway}

Create a VPN gateway on the subnet if you want to securely connect your VPC to another private network.

To create a VPN gateway, run the following command:

```
ibmcloud is vpn-gateway-create my-vpn-gateway $subnet
```
{: pre}

From the output that's returned, save the ID of the VPN gateway in a variable so you can use it later, for example:

```
vpn_gateway="0757-7e91085b-dc11-4707-aa4d-66e735e9a2bc"
```
{:pre}

The status of the VPN gateway is `pending` when it's first created. Before you can proceed, the VPN gateway needs to move to the `available` status, which takes a few minutes. To check the status of the VPN gateway, run this command:

```
ibmcloud is vpn-gateway $vpn_gateway
```
{: pre}

To create a VPN connection on the VPN gateway to peer address `169.61.161.150` and pre-shared key `mykey`, run the following command:

```
ibmcloud is vpn-gateway-connection-create my-vpn-conn $vpn_gateway 169.61.161.150 mykey
```
{: pre}

The status of the VPN connection is `down` when it is first created and becomes `up` after the connection is established. To check the status of VPN connections on a VPN gateway, run this command:

```
ibmcloud is vpn-gateway-connections $vpn_gateway
```
{: pre}

## Congratulations!
{: #congratulations-cli-tutorials}

You've successfully created and configured your VPC using the {{site.data.keyword.cloud_notm}} CLI. To try out more CLI commands, see the [CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpc-reference).
