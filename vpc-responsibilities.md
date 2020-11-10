---

copyright:
  years: 2019, 2020
lastupdated: "2020-11-10"

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
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}


# Understanding your responsibilities when using Virtual Private Cloud
{: #responsibilities-vpc}
{: help}
{: support}

Learn about the management responsibilities that you have when you use {{site.data.keyword.vpc_full}} (VPC). {{site.data.keyword.vpc_short}} customer responsibilities apply across all VPC infrastructure services, unless otherwise noted. For overall terms of use, see [Cloud Services terms](/docs/overview/terms-of-use?topic=overview-terms#terms).
{:shortdesc}

## Overview of shared responsibilities
{: #overview-by-resource}

{{site.data.keyword.vpc_short}} is an infrastructure-as-a-service (IaaS) offering in the [{{site.data.keyword.cloud_notm}} shared responsibility model](/docs/overview/terms-of-use?topic=overview-shared-responsibilities). Review the following table of who is responsible for particular cloud resources when you use {{site.data.keyword.vpc_short}}. Then, you can view more granular tasks for shared responsibilities in [Tasks for shared responsibilities by area](#task-responsibilities).

| Resource | Incident and Operations Management | Change Management | Identity and Access Management | Security and Regulation Compliance | Disaster Recovery |
| - | - | - | - | - | - |
| Data | Customer | Customer | Customer | Customer | Customer |
| Application | Customer | Customer | Customer | Customer | Customer |
| Operating system | Customer | Customer | Customer | Customer | Customer |
| Virtual servers | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | Shared | Shared | Shared |
| Virtual storage | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | Shared | Shared | Shared |
| Virtual network | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | Shared | Shared | Shared |
| Hypervisor | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Physical servers and memory | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Physical storage | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Physical network and devices | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | Shared |
| Facilities and data centers | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
{:caption="Table 1. Responsibilities by resource" caption-side="top"}

## Tasks for shared responsibilities by area
{: #task-responsibilities}

After reviewing the [overview](#overview-by-resource), see what tasks you and IBM share responsibility for when you use {{site.data.keyword.vpc_short}}.

### Incident and operations management
{: #incident-and-ops}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities |  Your Responsibilities |
|----------|-----------------------|-----------------------|
| Infrastructure | {{site.data.keyword.IBM_notm}} deploys a fully managed, highly available, secured, IBM-owned infrastructure. | The Customer uses the provided API, CLI, or UI console to provision compute and storage, as well as to adjust networking configurations to meet the needs of their workload. To automate the provisioning and management of VPC service instances, you can use automation tools, such as IBM Cloud Schematics or Terraform. |  
| Availability | {{site.data.keyword.IBM_notm}} fulfills requests for VPC infrastructure, such as VPCs, subnets, virtual server instances, block storage volumes, security groups, access control lists, floating IPs, public gateways, and SSH keys across multiple availability zones (AZs) and multi-zone regions (MZRs). | The Customer designs and deploys their workload in a way that achieves high availability using our provided tools, such as multiple availability zones. At a high level, you are responsible for deploying workloads in different zones of the region, using at least two load balancers that are located in different zones, and either using DNS records to point to the load balancers or ensuring that your application can handle the list of IP addresses that it can connect to. For more information, see [Deploy isolated workloads across multiple locations and zones](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region). |
| Bring your own CIDR | {{site.data.keyword.IBM_notm}} provides the ability to bring your own CIDR block to a subnet. | The Customer ensures the CIDR blocks that they specify for their VPC do not conflict with CIDR blocks used by any other network that they plan to connect to their VPC. |
| Monitoring | {{site.data.keyword.IBM_notm}} provides built-in virtual server instance monitoring and IBM Cloud Monitoring with Sysdig. | The Customer monitors the health of their workload using either the built-in virtual server instance monitoring or IBM Cloud Monitoring with Sysdig. |
| Logs | {{site.data.keyword.IBM_notm}} provides access to logs for debugging purposes. | The Customer uses LogDNA to check logs, as needed. |
| Workloads | {{site.data.keyword.IBM_notm}} provides tools and features for customer use. | The Customer uses the provided tools and features to configure and deploy their highly available and resilient workloads by setting up permissions, integrating with other services, externally serving and monitoring health, as well as saving, backing up, and restoring data. |
| Flow logs | {{site.data.keyword.IBM_notm}} provides the ability to collect flow log data from various endpoints. | The Customer understands the IBM Cloud Flow Logs for VPC data retention process and ensures that their destination COS bucket is properly secured and encrypted. |
{: caption="Table 2. Responsibilities for incidents and operations" caption-side="top"}

### Change management
{: #change-management}

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|-----------------------|
| OS-based update repositories | {{site.data.keyword.IBM_notm}} provides private network access to OS-based update repositories. | The Customer maintains OS patches, version updates, and security updates. |
{: caption="Table 3. Responsibilities for change management" caption-side="top"}

### Identity and access management
{: #iam-responsibilities}

Identity and access management includes tasks such as authentication, authorization, access control policies, as well as approving, granting, and revoking access.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|-----------------------|
| IBM Cloud IAM  | {{site.data.keyword.IBM_notm}} provides the function to restrict access to resources through the IBM Cloud console and REST APIs. | The Customer is responsible for managing access to resources through IBM Cloud Identity and Access Management (IAM). |
{: caption="Table 4. Responsibilities for identity and access management" caption-side="top"}

### Security and regulation compliance
{: #security-compliance}

Security and regulation compliance includes tasks such as security control implementation and compliance certification.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|-----------------------|
| Compliance | {{site.data.keyword.IBM_notm}} maintains controls commensurate to current industry compliance standards. {{site.data.keyword.IBM_notm}} also maintains General Data Protection Regulation (GDPR) readiness for customer compliance. See [Understanding IBM Cloud compliance](/docs/overview?topic=overview-compliance) for details. | The Customer is responsible for ensuring their own compliance with various laws and regulations, including the European Union General Data Protection Regulation. |
| Security features | {{site.data.keyword.IBM_notm}} enables security features, such as encrypted disks. | The Customer uses the provided security features, such as restricting user access to the appropriate resources and resource groups. |
| Vulnerabilities | {{site.data.keyword.IBM_notm}} continuously monitors stock images to detect vulnerability and security compliance issues. | The Customer is responsible for their education on possible vulnerabilities and security issues through security bulletins that describe actions to remediate any vulnerabilities. A Customer can use the [IBM Cloud status](/docs/get-support?topic=get-support-viewing-cloud-status) website to find announcements and security bulletin notifications about key events that affect the IBM Cloud platform, infrastructure, and major services. |
| Audit records | {{site.data.keyword.IBM_notm}} provides audit records of the VPC resource lifecycle through IBM Cloud Activity Tracker with LogDNA. | The Customer uses IBM Cloud Activity Tracker with LogDNA tooling to monitor audit records. |
| Security groups and ACLs | {{site.data.keyword.IBM_notm}} provides the ability to restrict access to virtual server instances using security groups and networks ACLs. | The Customer uses security groups and network ACLs to secure their virtual server instances, such as restricting what IP addresses can SSH into the instance. |
| Public Network Access | {{site.data.keyword.IBM_notm}} provides options to use a public gateway or floating IPs. | The Customer chooses how to connect their workload to the public internet, if applicable, either through a public gateway or floating IP. |
| Access restriction | {{site.data.keyword.IBM_notm}} provides security measures for customers to restrict access to resources and resource groups. | The Customer restricts user access to the appropriate resources and resource groups.
| Activity tracker | {{site.data.keyword.IBM_notm}} provides logging and monitoring tools. | The Customer integrates IBM Cloud Activity Tracker with LogDNA and IBM Cloud Monitoring with Sysdig data into their auditing and monitoring processes. |
| Encryption |  IBM Cloud VPN for VPC supports encrypted traffic using IKE/IPsec policies. | The Customer ensures that their connection is encrypted end-to-end, if required.
{: caption="Table 5. Responsibilities for security and regulation compliance" caption-side="top"}

### Disaster recovery
{: #disaster-recovery}

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|-----------------------|
| Load balancer and VPN disaster recovery | IBM Cloud Load Balancer and VPN for VPC have off-site storage and replication of configuration data in an out-of-region disaster recovery node with daily backups. This data is fully managed by IBM Cloud and no customer input is required to ensure service recovery, although there can be up to a 24-hour loss of configuration data.| The Customer sets up their backup and recovery strategies for workload data. |
{: caption="Table 6. Responsibilities for disaster recovery" caption-side="top"}
