---

copyright:
  years: 2019, 2020
lastupdated: "2020-05-12"

keywords: responsibilities, ha, high availability, disaster recovery

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

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.vpc_full}} (VPC). For a high-level view of the service types in {{site.data.keyword.Bluemix}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{:shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.vpc_short}}. For the overall terms of use, see [{{site.data.keyword.Bluemix}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).

## Incidents and operations management
{: #cloud-infrastructure}

IBM provides you with an enterprise cloud platform to deploy your workload. You choose how to set up, integrate, back up, and operate your workloads in the cloud.

### IBM Cloud infrastructure
{: #mgmt-infrastructure}

You and IBM have unique responsibilities for the set up and maintenance of IBM Cloud infrastructure and your workloads.

| Task | IBM Responsibilities |  Your Responsibilities |
|----------|-----------------------|-----------------------|
| Infrastructure | Deploy a fully managed, highly available, secured, IBM-owned infrastructure. | Use the provided API, CLI, or UI console to provision compute and storage, and to adjust networking configurations to meet the needs of your workload. |  
| Availability | Fulfill requests for VPC infrastructure, such as VPCs, subnets, virtual server instances, block storage volumes, security groups, access control lists, floating IPs, public gateways, and SSH keys across multiple availability zones (AZs) and multi-zone regions (MZRs).  | Design and deploy your workload in a way that achieves high availability using our provided tools, such as multiple availability zones. At a high level, deploy your workloads in different zones of the region, use at least two load balancers that are located in different zones, and either use DNS records to point to the load balancers, or ensure that your application can handle a list of IP addresses that it can connect to. For more information, see [Deploy isolated workloads across multiple locations and zones](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region). |
| Bring Your Own CIDR | Provide the ability to bring your own CIDR block to a subnet. | Make sure the CIDR blocks you specify for your VPC do not conflict with CIDR blocks used by any other network that you plan to connect to your VPC. |
| Backups | Load Balancer and VPN for VPC have off-site storage and replication of configuration data in an out-of-region disaster recovery node with daily backups. This data is fully managed by IBM Cloud and no customer input is required to ensure service recovery, although there can be up to a 24-hour loss of configuration data.| Set up your backup and recovery strategies for workload data. |
| Tools | Provide a suite of tools to automate VPC workload creation, such as the [IBM Cloud Terraform](https://ibm-cloud.github.io/tf-ibm-docs/index.html){: external} Provider. | None |
| Monitoring |Provide built-in virtual server instance monitoring and IBM Cloud Monitoring with Sysdig. | Monitor the health of your workload using either the built-in virtual server instance monitoring or IBM Cloud Monitoring with Sysdig. |
{: caption="Table 1. Responsibilites for IBM Cloud infrastructure" caption-side="top"}

### Application Orchestration
{: #app-orchestration}

Application orchestration is your responsibility and not the responsibility of IBM.

| Responsibilities |  Ownership |
|----------|-----------------------|
| Use the provided tools and features to configure and deploy; set up permissions; integrate with other services; externally serve; monitor the health; save, back up, and restore data; and otherwise manage your highly available and resilient workloads. | Customer |
| Design and deploy your workload in a way that achieves high availability using our provided tools, such as multiple availability zones. At a high level, deploy your workloads in different zones of the region, use at least two load balancers that are located in different zones, and either use DNS records to point to the load balancers, or ensure that your application can handle a list of IP addresses that it can connect to. | Customer |
{: caption="Table 4. Responsibilites for application orchestration" caption-side="top"}

## Change management
{: #vpc-workloads}

You and IBM have unique responsibilities for change management.

| Responsibilities |  Ownership |
|----------|-----------------------|
| Provide private network access to OS-based update repositories. | IBM |
| Maintain OS patches, version updates, and security updates.| Customer|
{: caption="Table 2. Responsibilites for VPC workloads" caption-side="top"}

## Security and regulation compliance
{: #security-rich-environment}

You and IBM have unique responsibilities for security and compliance.

| Responsibilities |  Ownership |
|----------|-----------------------|
| Maintain controls commensurate to current industry compliance standards. | IBM |
|Enable security features, such as encrypted disks. | IBM |
| Continuously monitor stock images to detect vulnerability and security compliance issues. | IBM |
| Use IBM Cloud Identity and Access Management (IAM) for authentication and authorization of VPC resources.| IBM |
|Provide audit records of the VPC resource lifecycle through IBM Cloud Activity Tracker with LogDNA. | IBM |
| Use security groups and network ACLs to secure your virtual server instances, such as restricting what IP addresses can SSH into the instance.| Customer |
| Choose how to connect your workload to the public internet, if applicable, either using a public gateway or floating IP. | Customer |
| Restrict user access to the appropriate resources and resource groups. | Customer |
| Integrate IBM Cloud Activity Tracker with LogDNA and IBM Cloud Monitoring with Sysdig data into your auditing and monitoring processes. | Customer |
|If your application requires end-to-end encryption, ensure that your connection is encrypted end-to-end. | Customer|
{: caption="Table 3. Responsibilites for a security-rich environment" caption-side="top"}

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
