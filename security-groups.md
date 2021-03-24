---

copyright:
  years: 2019, 2020

lastupdated: "2020-10-12"

keywords:  

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

# About security groups
{: #using-security-groups}

IBM Cloud Security Groups for VPC give you a convenient way to apply rules that establish filtering to each network interface of a virtual server instance, based on its IP address. When you create a security group, you configure it to create the network traffic patterns you want.
{:shortdesc}

By default, a security group denies all traffic. As rules are added to a security group, it defines the traffic that the security group permits.

Rules are _stateful_, which means that reverse traffic in response to allowed traffic is automatically permitted. For example, if you create a rule to allow inbound TCP traffic on port 80, the rule also allows replying outbound TCP traffic on port 80 back to the originating host, without the need for another rule.

Security groups are scoped to a single VPC. This scoping implies that a security group can be attached _only_ to network interfaces of instances within the same VPC.

When an instance is created and no security groups are specified, the instance's primary network interface is attached to the _default_ security group of that instance's VPC. For more information, see [Updating the default security group](/docs/vpc?topic=vpc-updating-the-default-security-group#updating-the-default-security-group).

## Related links
{: #sg-related-links}

These links provide additional information about IBM Cloud Security Groups for VPC:

* [Security Groups CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#security-groups-cli-ref)
* [Security Groups API reference](https://{DomainName}/apidocs/vpc#list-security-groups)
* [Security Groups required permissions](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls#sg-authorizations-required-for-api-and-cli-calls)
* [Security Groups for VPC infrastructure resources for Terraform](/docs/terraform?topic=terraform-vpc-gen2-resources#sec-group)
* [Security Groups Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-network-security-group)
* [Security Groups quotas](/docs/vpc?topic=vpc-quotas#security-group-quotas)
