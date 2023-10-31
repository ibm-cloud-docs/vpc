---

copyright:
  years:  2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Working with virtual network interfaces
{: #vni-working}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

You can create a virtual network interface for an {{site.data.keyword.cloud}} using the UI, CLI, API, and Terraform.
{: shortdesc}

A virtual network interface can be created without attaching to a target. Therefore, a virtual network interface can exist even when its target is removed. For example, within a VPC, your instance has a public IP address by which the instance can be reached from the internet, and a private IP address by which it can be reached from other instances in your VPC. You configured the instance with custom security group rules. If you delete the instance, the public IP address and security groups are detached. The private IP address is released and can be reserved again by another resource. If you want to create a new instance with the same public IP address, private IP address, and security groups as the deleted instance, you must reserve the private IP address again, and individually attach the previous public IP address and security groups.

You can use a virtual network interface to manage the IP addresses and security groups in a separate resource with a lifecycle that is independent of your instances. Create a virtual network interface with a private IP address, public IP address, and security groups. Then, create an instance with this virtual network interface. Later you can delete the instance, preserving the virtual network interface, and create a new instance with the same virtual network interface.

## Before you begin
{: #vni-prerequisites}

To create a virtual network interface, you must have the following prerequisites:

* A VPC instance
* A subnet in which to create a virtual network interface
* You must have IAM Administrator role permission to configure IP spoofing and infrastructure NAT.
