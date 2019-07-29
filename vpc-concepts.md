---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

keywords: glossary, terminology, concepts, vpc, virtual private cloud, definitions

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# VPC concepts
{: #vpc-concepts}

Learn more about {{site.data.keyword.vpc_full}} by getting to know these key VPC concepts.
{:shortdesc}

## Floating IP
{: #fip-def}
Floating IP is a method to provide inbound and outbound access to the internet for VPC resources such as virtual server instances using assigned _Floating IP addresses_ from a pool.

Floating IP addresses are public IP addresses that are provided by the system from the pre-existing pool. You can't bring your own public IP addresses. Floating IP addresses are reachable from the internet, and they can be associated to an instance by means of a network interface.

You can reserve a floating IP address from the pool of available floating IP addresses provided by IBM, and you can associate it to or disassociate it from any instance in the same VPC. Floating IPs allow a server to communicate with the public internet and also with a private subnet within your cloud environment.

## Image
{: #image-def}
The information required to launch a virtual server instance, or _instance_. Typically, it is a snapshot image of a commercially-available operating system, used for booting.

## Instance
{: #instance-def}
A virtual server instance, or VSI, that runs within a VPC. To provision scalable compute resources in a VPC, you'll create virtual server instances with the core and RAM configuration that's right for your workload.

## Profile
{: #profile-def}
A profile is a popular combination of vCPU and RAM that can be instantiated quickly to start up a virtual server instance. Three families of profiles are supported: Balanced, Compute, and Memory.

## Public gateway
{: #pgw-def}
A public gateway enables outbound-only access for a subnet to connect to the internet. Subnets are private by default; however, you can attach a subnet to a public gateway so that all virtual server instances in that subnet can connect to the internet.

Public gateways uses Many-to-1 NAT, which means that thousands of instances with private addresses use one public IP address to talk to the public internet. Public gateways don't enable the internet to initiate a connection with the instances in the attached subnet.

## Region
{: #region-def}
A geographic area within which a VPC's services, resources, and applications are deployed. Each region contains multiple zones, which represent independent fault domains. {{site.data.keyword.vpc_short}} spans multiple zones within its assigned region.

## Security Group
{: #security-group-def}
A security group acts as a virtual firewall that controls the inbound and outbound traffic for one or more virtual server instances. A security group is a collection of rules that specify whether to allow traffic for an associated instance. When you create an instance, you can associate it with one or more security groups. Given the correct permissions, customers can modify security group rules.

## Subnet
{: #subnet-def}
A subnet is an IP address range, bound to a single zone, which cannot span multiple zones or regions. A subnet can span the entirety of a zone in an {{site.data.keyword.vpc_short}}.

For the purposes of {{site.data.keyword.vpc_short}}, the important characteristic for a subnet is the fact that subnets can be isolated from one another, as well as being interconnected in the usual way. It is the isolation of subnets that allows you to create a private space within the public cloud.

## SSH Key
{: #ssh-key-def}
Automatically generated, cryptographic access credentials that control access to virtual server instances in a VPC.

## Virtual Private Cloud (VPC)
{: #vpc-def}
A secure, isolated virtual network within IBM Cloud. It provides fine-grained control over virtual infrastructure and network traffic segmentation, along with security and the ability to scale dynamically.

## Zone
{: #zone-def}
An independent fault domain. A zone is an abstraction designed to assist with improved fault tolerance and decreased latency. A zone guarantees the following properties:

 * Because each zone is an independent fault domain, it is extremely unlikely for two zones in a region to fail simultaneously.
 * Traffic between zones in a region will experience less than 2ms in latency.
