---

copyright:
  years: 2019
lastupdated: "2019-12-17"

keywords: vpc, responsibilities, ha, high availability, disaster recovery

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}


# Your responsibilities by using Virtual Private Cloud
{: #responsibilities-vpc}

Learn about management responsibilities and terms and conditions that you have when you use {{site.data.keyword.vpc_full}}.
{:shortdesc}

## Management responsibilities
{: #management-responsibilities}

IBM provides you with an enterprise cloud platform to deploy your workload. You choose how to set up, integrate, back up, and operate your workloads in the cloud.

### Cloud Infrastructure
{: #cloud-infrastructure}

IBM's responsibilities:
- Deploy a fully managed, highly available, secured, IBM-owned infrastructure.
- Fulfill requests for VPC infrastructure, such as VPCs, subnets, virtual server instances, block storage volumes, security groups, access control lists, floating IPs, public gateways, and SSH keys across multiple availability zones.
- Provide the ability to bring your own CIDR block to a subnet.
- Load Balancer and VPN for VPC have off-site storage and replication of configuration data in an out-of-region disaster recovery node with daily backups. This data is fully managed by IBM Cloud and no customer input is required to ensure service recovery, although there can be up to a 24-hour loss of configuration data.

Your responsibilities:
- Use the provided API, CLI, or UI console to provision compute and storage, and to adjust networking configurations to meet the needs of your workload.
- Make sure the CIDR blocks you specify for your VPC do not conflict with CIDR blocks used by any other network that you plan to connect to your VPC.
- Design and deploy your workload in a way that achieves high availability using our provided tools, such as multiple availability zones. At a high level, deploy your workloads in different zones of the region, use at least two load balancers that are located in different zones, and either use DNS records to point to the load balancers, or ensure that your application can handle a list of IP addresses that it can connect to.

### VPC Workloads
{: #vpc-workloads}

IBM's responsibilities:
- Provide a suite of tools to automate VPC workload creation, such as the [IBM Cloud Terraform](https://ibm-cloud.github.io/tf-ibm-docs/index.html){: external} Provider.
- Provide private network access to OS-based update repositories.

Your responsibilities:
- Maintain OS patches, version updates, and security updates.
- Set up your backup and recovery strategies for workload data.
- Monitor the health of your workload using either the built-in virtual server instance monitoring or IBM Cloud Monitoring with Sysdig.

### Security-rich Environment
{: #security-rich-environment}

IBM's responsibilities:
- Maintain controls commensurate to current industry compliance standards.
- Enable security features, such as encrypted disks.
- Continuously monitor stock images to detect vulnerability and security compliance issues.
- Use IBM Cloud Identity and Access Management (IAM) for authentication and authorization of VPC resources.
- Provide audit records of the VPC resource lifecycle through IBM Cloud Activity Tracker with LogDNA.

Your responsibilities:
- Use security groups and network ACLs to secure your virtual server instances, such as restricting what IP addresses can SSH into the instance.
- Choose how to connect your workload to the public internet, if applicable, either using a public gateway or floating IP.
- Restrict user access to the appropriate resources and resource groups.
- Integrate IBM Cloud Activity Tracker with LogDNA and IBM Cloud Monitoring with Sysdig data into your auditing and monitoring processes.
 
### App Orchestration
{: #app-orchestration}

IBM's responsibilities:
- None

Your responsibilities:
- Use the provided tools and features to configure and deploy; set up permissions; integrate with other services; externally serve; monitor the health; save, back up, and restore data; and otherwise manage your highly available and resilient workloads.
- Design and deploy your workload in a way that achieves high availability using our provided tools, such as multiple availability zones. At a high level, deploy your workloads in different zones of the region, use at least two load balancers that are located in different zones, and either use DNS records to point to the load balancers, or ensure that your application can handle a list of IP addresses that it can connect to.


## Prohibited usage of IBM Cloud Virtual Private Cloud
{: #terms}

Clients must not misuse {{site.data.keyword.cloud_notm}} infrastructure.

Misuse includes:
- Any illegal activity
- Distribution or execution of malware
- Harming or interfering with the use of IBM Cloud infrastructure
- Harming or interfering with the use of any other service or system
- Unauthorized access to any service or system
- Unauthorized modification of any service or system
- Violation of the personal rights of others

See [Cloud Services terms](/docs/overview/terms-of-use?topic=overview-terms) for overall terms of use.
