---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-04"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Security best practices for the metadata service
{: #imd-security-best-practices}

IBM takes data security seriously and recommends you follow these best practices to ensure the highest level of protection for your metadata.
{: shortdesc}

## Overview
{: #imd-sec-overview}

This topic describes how to configure security safeguards to protect your metadata by:

* Disabling access to the metadata service for an instance or an account when it's not needed.
* Limiting or not assigning trusted profiles for compute resource identities as necessary.
* Enhancing network security.

## Disable access to the metadata service for an instance or account
{: #imd-disable-instance-acct}

You can disable access to the service on an existing instance where it is enabled. See [Enable or disable access to the metadata service](/docs/vpc?topic=vpc-imd-configure-service).

## Use `iptables` firewall to limit access on Linux
{: #imd-iptables}

Use the `iptables` Linux firewall utility to create a barrier between the metadata service link-local address (trusted network) and the internet (untrusted network). Define rules that govern which traffic is allowed and which is blocked.

The following example uses Linux `iptables` and its owner module, based on its default installation Apache ID, to prevent the Apache Web server from accessing the metadata link-local address (169.254.169.254). It uses a deny rule to reject all metadata requests from any process that's running as that user.

```sh
sudo iptables --append OUTPUT --proto tcp --destination 169.254.169.254 --match owner --uid-owner apache --jump REJECT
```
{: pre}

This endpoint is accessible to all commands, processes, and software applications that are running within a virtual server instance. Access to the API endpoint is not available outside the virtual server instance. This step adds another level of security.
{: note}

Another alternative is to use allow rules to define access to particular users or groups. Allow rules require you to decide what software needs access to instance's metadata. By defining rules, you can prevent software from accidentally accessing the metadata service if you later change the software or configuration on the instance.

You can also define group usage of the allow rules. Add and remove users from a permitted group without changing the firewall rule.

The following example prevents access to the metadata service by all processes, except for processes that are running in the user account `trustworthy-user`:

```sh
sudo iptables --append OUTPUT --proto tcp --destination 169.254.169.254 --match owner ! --uid-owner trustworthy-user --jump REJECT
```
{: pre}

## Limit trusted profiles for compute resource identities
{: #imd-limit-trusted-profiles}

Limit trusted profiles that you create for compute resource identities in IAM. Optionally, don't assign a compute resource identity to an instance.

When you [remove trusted profiles](/docs/account?topic=account-trusted-profile-update&interface=ui#remove-tp-console){: ui}[remove trusted profiles](/docs/account?topic=account-trusted-profile-update&interface=cli#remove-tp-cli){: cli}[remove trusted profiles](/docs/account?topic=account-trusted-profile-update&interface=api#remove-tp-api){: api}, compute resources and federated users are unlinked from the profile, and can no longer apply the trusted profile identity.

You can also update existing trusted profiles by redefining the trust relationship, assigning access policies, and updating session limits. For more information, see [Updating trusted profiles](/docs/account?topic=account-trusted-profile-update).

## Additional network security measures
{: #imd-network-security}

Consider the following options for controlling network traffic to your virtual server instances:

* Restrict access to your instances by using [security groups](/docs/vpc?topic=vpc-configuring-the-security-group).

* Ensure your instance response hop limit is configured properly. An improper configuration can result in timeout errors while trying to reach the service. For more information, see [Set the metadata hop limit by using the UI](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#set-hop-limit-ui).

* Set up [access control lists](/docs/vpc?topic=vpc-using-acls)(ACL) to control all incoming and outgoing traffic in {{site.data.keyword.vpc_full}}. An ACL is a built-in, virtual firewall, similar to a security group. In contrast to security groups, ACL rules control traffic to and from the subnets, rather than to and from the instances.

* Use a {{site.data.keyword.vpn_vpc_full}} to establish private connections from your remote networks to your VPCs.

* Use {{site.data.keyword.cloud}} [Virtual Private Endpoints](/docs/vpc?topic=vpc-about-vpe) (VPE) for VPC to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, which are allocated from a subnet within your VPC. VPEs are virtual IP interfaces that are bound to an endpoint gateway created on a per service or service instance basis.

* Use IBM Cloud [flow Logs](/docs/vpc?topic=vpc-flow-logs) on the VPC to monitor the traffic that reaches your instances.

* [Enable secure access](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#secure-access-ui) to the metadata service. When secure access is enabled, the metadata service is accessible only to the virtual server instance by encrypted HTTP secure protocol (HTTPS).
