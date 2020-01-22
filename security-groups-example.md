---

copyright:
  years: 2019, 2020

lastupdated: "2020-01-22"

keywords: security group, VSI, ping, TCP, outbound, rules, metadata, setting up, vpc, vpc network

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Setting up security groups by using the CLI
{: #setting-up-security-groups-using-the-cli}

In this example, you create a virtual server instance (VSI) with a security group that is enabled by using the command-line interface (CLI). Figure 1 shows what the scenario looks like.
{:shortdesc}

![Figure showing a virtual server instance with a security group enabled](/images/security-groups-schematic.svg "Figure showing a virtual server instance with a security group enabled"){: caption="Figure 1. Instance with security group enabled" caption-side="top"}

Notice in Figure 1 that the instance named **SG4** has a floating IP `169.60.208.144` assigned to it, in addition to its internal VPC address `10.10.10.5`; therefore, it can talk to the public internet. The security group assigned to instance **SG4** is named "demosg".

The instance **SG8** is internal-only to the VPC, with a private IP address. The security group assigned to instance **SG8** is named "my_vpc_sg". Both of these instances exist within the VPC named `sgvpc` and also on the same subnet `10.10.10.0/24` so they can communicate with each other.

## Steps for creating an instance with a security group attached
{: #steps-for-creating-a-vsi-with-a-security-group-attached}

The security group rules for "my_vpc_sg" include basic functions of SSH, PING, and outbound TCP.

Notice that you must create the security group first, with the `ibmcloud is sgc` command, and then create the instance that uses this security group.

This example code skips a few steps, so here's where you can find more information:

 * For instructions about creating a VPC and subnet, see [Creating a VPC by using the CLI](/docs/vpc?topic=vpc-creating-a-vpc-using-cli).

You can copy and paste commands from this example CLI code to get started creating an instance with a security group attached. System responses are not shown completely in this sample code. You need to update your commands with the correct resource IDs for your _VPC_, _subnet_, _image_, and _key_, and the correct _security group ID number_.

1. Create a security group called “my_vpc_sg”

   ```
   ibmcloud is security-group-create my_vpc_sg $vpc
   ```
   {: pre}

   Save the ID in a variable so you can use it later, for example, in the variable named `sg`:

   ```
   sg=0738-2d364f0a-a870-42c3-a554-000000632953
   ```
   {: pre}

2. Add rules to allow SSH, PING, and outbound TCP

   ```
   ibmcloud is security-group-rule-add $sg inbound tcp --port-min 22 --port-max 22
   ibmcloud is security-group-rule-add $sg inbound icmp --icmp-type 8 --icmp-code 0
   ibmcloud is security-group-rule-add $sg outbound tcp
   ```
   {: codeblock}

3. Create an instance with the security group.

   ```
   ibmcloud is instance-create test-instance $vpc us-south-2 b-4x16 $subnet 1000 \ 
   --image $image --keys $key --security-groups $sg
   ```
   {: pre}

## Command list cheat sheet
{: #command-list-cheat-sheet}

For a complete list of the available VPC CLI commands for security groups, type:

```
ibmcloud is help | grep sg
```
{: pre}

To see your security group and its metadata, including rules, you'd type (for the previous example):

```
ibmcloud is sg $sg
```
{: pre}

To add a security group rule, here's an example command for adding a PING inbound rule to a security group:

```
ibmcloud is security-group-rule-add $sg inbound icmp --icmp-type 8 --icmp-code 0

```
{: pre}
