---

copyright:
  years: 2020
lastupdated: "2020-12-09"

keywords: security and compliance, security, compliance

subcollection: vpc

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:term: .term}

# Managing security and compliance with VPC Infrastructure Services
{: #manage-security-compliance}

{{site.data.keyword.vpc_full}} is integrated with the {{site.data.keyword.compliance_short}} to help you manage security and compliance for your organization.
{: shortdesc}


With the {{site.data.keyword.compliance_short}}, you can monitor controls and goals that pertain to {{site.data.keyword.vpc_short}} infrastructure.


## Monitoring security and compliance posture with VPC
{: #monitor-vpc}

As a security or compliance focal, you can use the {{site.data.keyword.vpc_short}} [goals](x2117978){: term} to help ensure that your organization is adhering to the external and internal standards for your industry. By using the {{site.data.keyword.compliance_short}} to validate the resource configurations in your account against a [profile](x2034950){: term}, you can identity potential issues as they arise.

All of the goals for {{site.data.keyword.vpc_short}} are added to the {{site.data.keyword.cloud_notm}} Best Practices Controls 1.0 profile but can also be mapped to other profiles.
{: note}

To start monitoring your resources, check out [Getting started with {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic-security-compliance-getting-started).

### Available goals for VPC
{: #vpc-available-goals}

* Ensure no VPC security groups allow incoming traffic from IP 0.0.0.0/0 to SSH port 22
* Ensure no VPC security groups allow incoming traffic from 0.0.0.0/0 to RDP ports 3389, 3390
* Ensure the default security group of every VPC restricts all traffic
* Ensure TLS 1.2 for all inbound traffic via load balancer for VPC
* Ensure logging for VPC flow logs is enabled in all VPCs
* Ensure no VPC security groups have inbound ports open to the internet (0.0.0.0/0)
* Ensure no VPC security groups have outbound ports open to the internet (0.0.0.0/0)
* Ensure a VSI has at least one VPC security group attached
* Ensure all network interfaces of a VSI have at least one VPC security group attached 
