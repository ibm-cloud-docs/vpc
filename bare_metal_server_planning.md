---

copyright:
  years: 2021, 2022
lastupdated: "2022-09-19"

keywords: bare metal servers, planning, planning bare metal server

subcollection: vpc

---

{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Planning for Bare Metal Servers on VPC
{: #planning-for-bare-metal-servers}

When you are planning to create a bare metal server on {{site.data.keyword.vpc_full}} (VPC) to host your VMware&reg; environments on x86 architecture or your Linux operation systems on s390x architecture, use the following configuration checklist as a guide.
{: shortdesc}

| Item | Considerations |
|----------|---------|
| Required IAM permissions | ___ Make sure that your account has the required user permissions. You must be assigned the `Editor` role of the target resource group to create bare metal servers in this resource group. <ul><li>For x86 architecture: You also need `Bare Metal Console Administrator` to access the ESXi Direct Console User Interface (DCUI) and `Bare Metal Advanced Network Operator` to modify IP spoofing and infrastructure NAT configuration on network interfaces.</li><li>For s390x architecture: Only IP spoofing is supported. </li></ul> <br><br> If you have authorization as an `Editor` or `Admin` for a VPC resource group, then you also inherit authorization to create, delete, and modify servers within that resource group. |
| Account limits | ___ Check your [account limits](/docs/vpc?topic=vpc-quotas#service-limits) for concurrent resources. The maximum number of servers per account is 25. |
| Image | <ul><li>For x86 architecture: You have two VMware ESXi image licensing options: Licensed ESXi 7.x image and BYOL (bring-your-own) ESXi 7.x image. You can manage the licensing yourself by selecting the BYOL option. </li><li>For s390x architecture: Choose one of the IBM provided Linux images. Note that the BYOL option is not supported for LinuxONE Bare metal servers. </li></ul>|
| SSH key | ___ Make sure that your [SSH key](/docs/vpc?topic=vpc-ssh-keys#ssh-keys) is available. |
| Location | ___ Determine what region and zone to select. |
| Subnet | ___ Determine which subnets that you want the server to connect to. |
| Profile | <ul><li>For x86 architecture: Select a [profile](/docs/vpc?topic=vpc-bare-metal-servers-profile) that fits your workload. Main considerations include the number of vCPU, amount of RAM, and whether NVMe drives are needed.</li><li>For s390x architecture: Select a [profile](/docs/vpc?topic=vpc-linuxone-bare-metal-servers-profile) that fits your workload. </li></ul> |
| Naming | ___ Make sure that you have a unique name for the server. <br><br>The server name must be unique within an account and region.{: note} |
| Network interfaces | ___ Determine how many network interfaces that you need.<br><br><ul><li>For x86 architecture: You can attach PCI and VLAN network interfaces to your servers to support the VMware networking topology. By default, each server is attached with one PCI network interface as the primary network interface. <br><br> For more information about the bare metal server network, see [Networking overview for Bare Metal Servers on VPC ](/docs/vpc?topic=vpc-bare-metal-servers-network). </li><li>For s390x architecture: each LinuxONE Bare Metal server is attached with one hipersocket network interface. <br><br>For more information about the LinuxONE bare metal server network, see [Networking overview for LinuxONE Bare Metal servers ](/docs/vpc?topic=vpc-linuxone-bare-metal-servers-network).</li></ul>|
{: caption="Table 1. Planning checklist to create a bare metal server" caption-side="bottom"}

## Next steps
{: #next-steps-bm-on-vpc}

* If you're ready to get started, you can [create a bare metal server](/vpc-ext/provision/bm) on {{site.data.keyword.vpc_full}} now.
* For more information about creating a bare metal server on VPC, see [Creating bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).

<!-- and which [security group](/docs/vpc?topic=vpc-using-security-groups) to attach to each interface.-->
