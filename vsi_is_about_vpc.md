---

copyright:
  years: 2018, 2019
lastupdated: "2019-09-30"

keywords: virtual server instances, VSI, planning, best practices

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# About virtual server instances for VPC
{: #about-advanced-virtual-servers}

Virtual server instances for VPC give you access to all of the benefits of {{site.data.keyword.vpc_short}}, including network isolation, security, and flexibility. 
{:shortdesc}

## What are virtual server instances for VPC?
{: #what-are-vsis}
With virtual server instances for VPC, you can quickly provision instances with high network performance. When you provision an instance, you select a profile that matches the amount of memory and compute power that you need for the application that you plan to run on the instance. After you provision an instance, you control and manage those infrastructure resources. 

## How are virtual server instances for VPC different from other IBM virtual server offerings?
{: #how-are-vsis-different}

In today's IBM Cloud Virtual Server offering, instances use native subnet and VLAN networking to communicate to each other within a data center (and single pod). Using subnet and VLAN networking in one pod works well until you must scale up or have large virtual resource demands that require resources to be created between pods. (Adding appliances for VLAN spanning can get expensive and complicated!) 

{{site.data.keyword.vpc_short}} adds a network orchestration layer that eliminates the pod boundary, creating increased capacity for scaling instances. The network orchestration layer handles all of the networking for all virtual server instances that are within a VPC across regions and zones. With the software defined networking capabilities that VPC provides, you have more options for multi-vNIC instances and larger subnet sizes. 



