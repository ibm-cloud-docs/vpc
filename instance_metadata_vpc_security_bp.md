---

copyright:
  years: 2021
lastupdated: "2021-11-02"

keywords: metadata service, security, virtual server instance, instance

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:preview: .preview}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Security best practices for the instance metadata service
{: #imd-security-best-practices}

IBM takes data security seriously and recommends you follow these best practices to ensure the highest level of protection for your instance metadata.
{: shortdesc}

This service is available only to accounts with special approval to use this service. Contact [IBM support](/docs/vpc?topic=vpc-getting-help) if you're interested in getting access.
{: preview}

## Overview
{: #imd-sec-overview}

This topic describes how to configure security safeguards to protect your metadata by:

* Disabling the metadata service for an instance or an account
* Limiting, or not assigning, trusted profiles for compute resource identities
* Enhancing network security

## Disable the metadata service for an instance or account
{: #imd-disable-instance-acct}

For Beta, the metadata service is enabled by default for allow-listed accounts. To disable the service, see [Enable or disable the instance metadata service](/docs/vpc?topic=vpc-imd-configure-service). Also, see this [troubleshooting issue](/docs/vpc?topic=vpc-imd-troubleshoot#imd-ts-1) for more information about disabling the service. At this time, you must use an account not on the Beta allow-list.

## Use iptables firewall to limit access on Linux
{: #imd-iptables}

Use the `iptables` Linux firewall utility to create a barrier between the metadata service link local address (trusted network) and the internet (untrusted network). Defines rules that govern which traffic is allowed and which is blocked.

This example uses Linux `iptables` and its owner module, based on its default installation Apache ID, to prevent the Apache Web server from accessing the metadata link local address (169.254.169.254). It uses a deny rule to reject all instance metadata requests from any process running as that user.

```
$ sudo iptables --append OUTPUT --proto tcp --destination 169.254.169.254 --match owner --uid-owner apache --jump REJECT
```
{: codeblock}

 This endpoint is accessible to all commands, processes, and software applications running within a virtual server instance. Access to the API endpoint is not available outside the virtual server instance. This step adds another level of security.
 {: note}

Another alternative is to use allow rules to define access to particular users or groups. Allow rules require you to make a decision about what software needs access to instance metadata. By defining rules, you can prevent software can accidentally the metadata service if you later change the software or configuration on the instance. 

You can also define group usage of the allow rules. Add and remove users from a permitted goup without changing the firewall rule. 

This example prevents access to the instance metadata service by all processes, except for processes running in the user account `trustworthy-user`:

```
sudo iptables --append OUTPUT --proto tcp --destination 169.254.169.254 --match owner ! --uid-owner trustworthy-user --jump REJECT
```
{: pre}

## Limit trusted profiles for compute resource identities

Limit trusted profiles you create for compute resource identities. Optionally, don't assign a compute resource identity to an instance. 

When you [remove trusted profiles](/docs/account?topic=account-trusted-profile-remove), compute resources and federated users are unlinked from the profile and can no longer apply the trusted profile identity.

You can also update existing trusted profiles by redefining the trust relationship, assigning access policies, and updating session limits, For more information, see [Updating trusted profiles](/docs/account?topic=account-trusted-profile-update).

## Additional network security measures
{: #imd-network-security}

Consider the following options for controlling network traffic to your virtual server instances:

* Restrict access to your instances using [security groups](/docs/vpc?topic=vpc-configuring-the-security-group).

* Set up [ACLs](/docs/vpc?topic=vpc-using-acls) to control all incoming and outgoing traffic in Virtual Private Cloud. An ACL is a built-in, virtual firewall, similar to a security group. In contrast to security groups, ACL rules control traffic to and from the subnets, rather than to and from the instances.

* Use a Virtual Private Network to establish private connections from your remote networks to your VPCs.

* Use {{site.data.keyword.cloud}} [Virtual Private Endpoints](/docs/vpc?topic=vpc-about-vpe) (VPE) for VPC to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, allocated from a subnet within your VPC. VPEs are virtual IP interfaces that are bound to an endpoint gateway created on a per service or service instance basis.

* Use IBM Cloud [flow Logs](/docs/vpc?topic=vpc-flow-logs) on the VPC to monitor the traffic that reaches your instances.
 
## Manage security and compliance with VPC Infrastructure Services
{: #imd-compliance}

Security and Compliance Center can help you monitor your VPC infrastructure to validate resource configurations in your account against a profile and identify potential issues as they arise. See [Available goals for Virtual Servers](/docs/vpc?topic=vpc-manage-security-compliance#virtual-servers-available-goals) to define security standards for your virtual server instances.
