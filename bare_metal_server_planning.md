---

copyright:
  years: 2021, 2025
lastupdated: "2025-06-11"

keywords: planning bare metal servers

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning for Bare Metal Servers on VPC
{: #planning-for-bare-metal-servers}

When you are planning to create a bare metal server on {{site.data.keyword.vpc_full}} (VPC) to host your high performance applications on x86 architecture, use the following configuration checklist as a guide.
{: shortdesc}


| Item | Considerations |
|----------|---------|
| Required IAM permissions | Make sure that your account has the required user permissions. You must be assigned the `Editor` role of the target resource group to create bare metal servers in this resource group.  \n - For x86 architecture, you also need `Bare Metal Console Administrator` to access the ESXi Direct Console User Interface (DCUI) and `Bare Metal Advanced Network Operator` to modify IP spoofing and infrastructure NAT configuration on network interfaces. |
| Account limits | Check your [account limits](/docs/vpc?topic=vpc-quotas) for concurrent resources. The maximum number of servers per account is 25. |
| Image | Select a [Bare metal server image](/docs/vpc?topic=vpc-bare-metal-image).|
| SSH key | Make sure that your [SSH key](/docs/vpc?topic=vpc-ssh-keys#ssh-keys) is available. |
| Location | Determine what region and zone to select. |
| Subnet | Determine which subnets that you want the server to connect to. |
| Profile | Select a [profile](/docs/vpc?topic=vpc-bare-metal-servers-profile) that fits your workload. Main considerations include the number of vCPU, amount of RAM, and whether NVMe drives are needed. |
| Naming | Make sure that you use a unique name for the server. |
| Network bandwidth | Select an appropriate total bandwidth allocated to your server from 10 up to 200 Gbps. 200 Gbps is only available with Intel Sapphire Rapids profiles. This allotment is shared across all interfaces, and can be adjusted after provisioning. |
| Network interfaces | Determine how many network interfaces that you need.  \n - For x86 architecture, you can attach PCI and VLAN network interfaces to support the VMware networking topology. By default, each server is attached with one PCI network interface as the primary network interface. For more information about the bare metal server network, see [Networking overview for Bare Metal Servers on VPC](/docs/vpc?topic=vpc-bare-metal-servers-network).|
{: caption="Planning checklist to create a bare metal server" caption-side="bottom"}

## Next steps
{: #next-steps-bare-metal-servers-on-vpc}

* If you're ready to get started, you can [create a bare metal server](/infrastructure/provision/bm){: external} on {{site.data.keyword.vpc_full}} now.
* For more information about creating a bare metal server on VPC, see [Creating bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).
