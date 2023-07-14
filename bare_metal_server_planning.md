---

copyright:
  years: 2021, 2023
lastupdated: "2023-07-06"

keywords: planning bare metal servers

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning for Bare Metal Servers on VPC
{: #planning-for-bare-metal-servers}

When you are planning to create a bare metal server on {{site.data.keyword.vpc_full}} (VPC) to host your VMware&reg; environments on x86 architecture or your Linux operation systems on s390x architecture, use the following configuration checklist as a guide.
{: shortdesc}

| Item | Considerations |
|----------|---------|
| Required IAM permissions | ___ Make sure that your account has the required user permissions. You must be assigned the `Editor` role of the target resource group to create bare metal servers in this resource group.  \n - For x86 architecture, you also need `Bare Metal Console Administrator` to access the ESXi Direct Console User Interface (DCUI) and `Bare Metal Advanced Network Operator` to modify IP spoofing and infrastructure NAT configuration on network interfaces.  \n - For s390x architecture, only IP spoofing is supported.  \n If you have authorization as an `Editor` or `Admin` for a VPC resource group, then you also inherit authorization to create, delete, and modify servers within that resource group. |
| Account limits | ___ Check your [account limits](/docs/vpc?topic=vpc-quotas#service-limits) for concurrent resources. The maximum number of servers per account is 25. |
| Image | - For x86 architecture, you can use the VMware ESXi image Licensed ESXi 7.x image.  \n - For s390x architecture, choose an IBM-provided Linux image or select a supported custom image for BYOL (bring your own license). |
| SSH key | ___ Make sure that your [SSH key](/docs/vpc?topic=vpc-ssh-keys#ssh-keys) is available. |
| Location | ___ Determine what region and zone to select. |
| Subnet | ___ Determine which subnets that you want the server to connect to. |
| Profile | - For x86 architecture, select a [profile](/docs/vpc?topic=vpc-bare-metal-servers-profile) that fits your workload. Main considerations include the number of vCPU, amount of RAM, and whether NVMe drives are needed. |
| Naming | ___ Make sure that you use a unique name for the server. |
| Network interfaces | ___ Determine how many network interfaces that you need.  \n - For x86 architecture, you can attach PCI and VLAN network interfaces to support the VMware networking topology. By default, each server is attached with one PCI network interface as the primary network interface. For more information about the bare metal server network, see [Networking overview for Bare Metal Servers on VPC](/docs/vpc?topic=vpc-bare-metal-servers-network). |
{: caption="Table 1. Planning checklist to create a bare metal server" caption-side="bottom"}


## Next steps
{: #next-steps-bare-metal-servers-on-vpc}

* If you're ready to get started, you can [create a bare metal server](/vpc-ext/provision/bm){: external} on {{site.data.keyword.vpc_full}} now.
* For more information about creating a bare metal server on VPC, see [Creating bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).
