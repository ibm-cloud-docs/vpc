---

copyright:
  years: 2018, 2021
lastupdated: "2019-11-10"

keywords: security, groups, encryption, traffic, rules, subnet, instance, firewall, encryption

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Security in your VPC
{: #security-in-your-vpc}

You can keep {{site.data.keyword.vpc_full}} and workloads secure by controlling network traffic using security groups, by using network access control lists (ACLs), or by using both types of control.
{: shortdesc}

## Security overview
{: #security-overview}

Security groups and access control lists (ACLs) provide ways to control the traffic across the subnets and instances in your {{site.data.keyword.vpc_full}}, using rules that you specify. Security groups and ACLs add security to your subnets and instances:

* Traffic to and from a subnet can be controlled by Access Control Lists (ACLs).
* Security Groups can control the traffic at the virtual server instance level.
* Allow you to set up a public gateway for subnet access to the internet, guarded by ACLs.
* Allow you to implement a floating IP for virtual server instance access to the internet, guarded by SGs.

If you are configuring a VPC with [{{site.data.keyword.cis_full_notm}}({{site.data.keyword.cis_short_notm}})](/docs/cis?topic=cis-iam-and-cis), you can prevent DDoS attacks by allowing traffic only through {{site.data.keyword.cis_short_notm}} (allowlist) in your VPC. Set your Network ACL and security groups to allowlist {{site.data.keyword.cis_short_notm}} traffic.
{: tip}

![Figure showing how a VPC can be subdivided with subnets](images/vpc-connectivity-and-security.svg "Figure showing how a VPC can be subdivided with subnets"){: caption="Figure 1. IBM VPC connectivity and security" caption-side="bottom"}

## Definitions
{: #definitions}

The sections that follow describe basic functions of ACLs and security groups, and the ways in which VPC supports end-to-end encryption.

### Access Control List
{: #access-control-list}

An **Access Control List (ACL)** can manage (that is, it can allow or deny) inbound and outbound traffic for a subnet. An ACL is stateless, which means that inbound and outbound rules must be specified separately and explicitly. Each ACL consists of rules, based on a *source IP*, *source port*, *destination IP*, *destination port*, and *protocol*.

Every VPC has a default ACL that allows all inbound and outbound traffic. You can edit the default ACL rules, or create a custom ACL and attach it to your subnets. A subnet can only have one ACL attached to it at any time, but one ACL can be attached to multiple subnets. For more information about how to use ACLs, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).

### Security groups
{: #sgs-security}

A **security group** acts as a virtual firewall that controls the traffic for one or more virtual server instances. A security group is a collection of rules that specify whether to allow or deny traffic for an associated instance. You can associate an instance with one or more security groups and edit the security group rules. For more information, see [Using security groups](/docs/vpc?topic=vpc-using-security-groups).

### Comparing security groups and access control lists
{: #compare-security-groups-and-access-control-lists}

Table 1 summarizes some key differences between security groups and ACLs.

|  | Security Groups | ACLs    |
|-------------|-----------------|---------|
| Control level  | Virtual server instance    | Subnet  |
| State   | Stateful - When an inbound connection is permitted, it is allowed to reply | Stateless - Both inbound and outbound connections must be explicitly allowed |
| Supported rules | Uses allow rules only | Uses allow and deny rules|
| How rules are applied | All rules are considered | Rules are processed in sequence |
| Relationship to the associated resource | An instance can be associated with multiple security groups| Multiple subnets can be associated with the same ACL|
{: caption="Table 1. Differences between security groups and ACLs" caption-side="bottom"}

### End-to-end encryption
{: #end-to-end-encryption}

Although {{site.data.keyword.vpc_short}} doesn't provide end-to-end encryption, it allows for it. For example, if you have a secure endpoint on a virtual server instance (such as an HTTPS server on port 443), you can attach a floating IP to that instance, and then your connection is end-to-end encrypted from the client to the server on port 443. Nothing in the path forces a decryption. However, if you use an insecure protocol such as HTTP on port 80, your data is not encrypted from end to end.

If your application requires end-to-end encryption, then it is *your* responsibility to ensure that your connection is encrypted end-to-end.
{: important}
