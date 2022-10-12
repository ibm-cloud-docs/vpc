---

copyright:
  years: 2022
lastupdated: "2022-10-11"

keywords: linuxone bare metal servers, s390x bare metal servers

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About LinuxONE Bare Metal Servers
{: #about-linuxone-bare-metal-servers}

s390x Bare Metal Servers for VPC is available for customers with special approval to preview this service in the Washington DC (us-east) and São Paulo (br-sao) regions. 
{: preview}

LinuxONE Bare Metal Servers provide dedicated CPU cores, memory, and I/O channels to host your Linux&reg; workloads on s390x bare metal servers on your {{site.data.keyword.vpc_full}}. You can use supported Linux operating system images on your s390x bare metal servers. As s390x bare metal servers are integrated with the VPC platform, you can take advantage of the network, storage, and security capacity provided by the VPC.
{: shortdesc}

The s390x bare metal servers that you provision are supported by Processor Resource/Systems Manager (PR/SM). PR/SM is a hypervisor firmware that allows for virtualization of hardware and can manage multiple operating systems in a single central processing complex. The PR/SM separates or shares physical resources such as cores, I/O channels, and LAN interfaces across multiple logical partitions (LPARs). LPARs are the equivalent of LinuxONE bare metal server instances, and each LPAR supports an independent operating system (OS) that is loaded by a separate initial program load (IPL) operation.

## Key features
{: #bare-metal-features}

The following features are included with s390x bare metal servers.

### Workload profiles
{: #bare-metal-features-profiles}

You can choose different profiles to meet your individual workload needs and help accelerate deployment of your compute resources. You get maximum performance without oversubscribing.

For more information, see [s390x bare metal server profiles](/docs/vpc?topic=vpc-s390x-bare-metal-servers-profile).

### Latest semiconductor technology

s390x bare metal servers use IBM® Integrated Facility for Linux (IFL) that are dedicated to Linux workloads and offer on-chip acceleration for compression and encryption.

### EAL5+ isolation

s390x bare metal servers are equivalent to LPARs under the control of a PR/SM Hypervisor, which is designed for Common Criteria Evaluation Assurance Level 5+ (EAL5+) security certification.

For more information, see [Security target for PR/SM for IBM z15 and IBM LinuxONE III systems](https://commoncriteriaportal.org/files/epfiles/1133b_pdf.pdf){: external}.

## Benefits
{: #bare-metal-benefits}

You have the following benefits when you provision an s390x bare metal server.

### Rapid scaling
{: #bare-metal-rapid-scaling-benefit}

Quickly scale your dedicated environment - often, in 10 minutes or less when resources are available.

### Network orchestration
{: #bare-metal-network-orchestration-benefit}

A network orchestration layer handles the networking for all bare metal servers that are within an {{site.data.keyword.vpc_full}} across regions and zones. Create multiple, virtual private clouds in multizone regions. Network orchestration also helps improve security, reduce latency, and increase high availability.

You are responsible for security on your s390x bare metal server. That means you need upgrade or update the operating system as needed to make sure that vulnerabilities are addressed in a timely manner. s390x bare metal servers with floating IPs are internet-facing and you need to take appropriate precautions. For more information, see [Understanding your responsibilities](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance).
{: note}

## Pricing options
{: #bare-metal-pricing-options}

Pay-as-you-go bandwidth is per gigabyte. Your billing charges accrue from provision to cancellation, and are billed in arrears. Total pricing includes server profiles and software, internet data transfers, and extra VPC services. Each extra component is priced separately and is included as part of your total {{site.data.keyword.vpc_full}} charges. Service tiers are bound to your account, not to any specific VPC.

For more information about pricing, see [Pricing](https://www.ibm.com/cloud/vpc/pricing#tab_2651670){: external}.
