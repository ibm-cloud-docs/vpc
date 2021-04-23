---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-19"

keywords: security and compliance, security, compliance, fortress

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

To start monitoring your resources, check out [Getting started with {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-getting-started).

### Available goals for VPC
{: #vpc-available-goals}

* Ensure that no VPC security groups allow incoming traffic from IP 0.0.0.0/0 to SSH port 22
* Ensure that no VPC security groups allow incoming traffic from 0.0.0.0/0 to RDP ports 3389, 3390
* Ensure that the default security group of every VPC restricts all traffic
* Ensure that TLS 1.2 for all inbound traffic through load balancer for VPC
* Ensure that logging for VPC flow logs is enabled in all VPCs
* Ensure that no VPC security groups have inbound ports open to the internet (0.0.0.0/0)
* Ensure that no VPC security groups have outbound ports open to the internet (0.0.0.0/0)
* Ensure that a virtual server instance has at least one VPC security group attached
* Ensure that all network interfaces of a virtual server instance have at least one VPC security group attached 

### Available goals for load balancers
{: #lb-available-goals}
* Ensure that no public VPC application or network load balancers can be created
* Ensure that the VPC load balancer is configured with multiple members in the pool
* Ensure that the VPC load balancer listener is configured with default pool
* Ensure that the application load balancer has health check configured when created
* Ensure that the health check protocol is either HTTP or HTTPS
* Ensure that the HTTPS protocol is configured for VPC application load balancer pools that are associated with HTTPS listeners
* Ensure that the HTTPS (SSL and TLS) instead of HTTP is used for application load balancer

### Available goals for VPN gateways
{: #vpn-available-goals}

* Ensure that application end-to-end traffic is encrypted
* Ensure that IKE policy encryption is not `triple_des`
* Ensure that IKE policy authentication is set to `sha256`
* Ensure that IPsec policy encryption is not `triple_des`
* Ensure that IPsec policy authentication is set to `sha256`
* Ensure that IPsec policy does not have PFS disabled
* Ensure that Diffie-Hellman group is at least group 14
* Ensure that a strong pre-shared key is used for authentication with at least 24 alphanumeric characters
* Ensure that local and remote CIDR ranges cover only devices that are accessible through VPN
* Ensure that Dead Peer Detection Action is set to `restart`
