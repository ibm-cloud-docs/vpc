---

copyright:
  years: 2021
lastupdated: "2021-09-16"

keywords: bare metal servers, planning, planning bare metal server

subcollection: vpc

---

{:beta: .beta}
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

# Planning for Bare Metal Servers on VPC (Beta)
{: #planning-for-bare-metal-servers}

When you are planning to create a bare metal server on {{site.data.keyword.vpc_full}} (VPC) to host your VMware&reg; environments, use the following configuration checklist as a guide.
{: shortdesc}

Bare metal servers on VPC is a Beta feature that requires special approval. Contact your IBM Sales representative if you're interested in getting access.
{: beta}

| Item | Considerations |
|----------|---------|
| Required IAM permissions | ___ Make sure that your account has the required user permissions. You must be assigned the `Editor` role of the target resource group to create bare metal servers in this resource group. You also need `Bare Metal Console Administrator` to access the ESXi Direct Console User Interface (DCUI) and `Bare Metal Advanced Network Operator` to modify IP spoofing and infrastructure NAT configuration on network interfaces. <br><br> If you have authorization as an `Editor` or `Admin` for a VPC resource group, then you also inherit authorization to create, delete, and modify servers within that resource group. <br><br> You can't modify source IP spoofing and infrastructure NAT configuration in Beta.{: important}|
| Account limits | ___ Check your [account limits](/docs/vpc?topic=vpc-quotas#service-limits) for concurrent resources. The maximum number of servers per account is 25. |
| Image | ___ The Beta offering provides VMware ESXi image with two licensing options: Licensed ESXi 7.x image and BYOL (bring-your-own) ESXi 7.x image. You can manage the licensing yourself by selecting the BYOL option. |
| SSH key | ___ Make sure that your [SSH key](/docs/vpc?topic=vpc-ssh-keys#ssh-keys) is available. |
| Location | ___ Determine what region and zone to select. |
| Subnet | ___ Determine which subnets that you want the server to connect to. |
| Profile | ___ Select a [profile](/docs/vpc?topic=vpc-bare-metal-servers-profile) that fits your workload. Main considerations include the number of vCPU, amount of RAM, and whether NVMe drives are needed.
| Naming | ___ Make sure that you have a unique name for the server. <br><br>The server name must be unique within an account and region.{: note} |
| Network interfaces | ___ Determine how many network interfaces that you need.<br><br>You can attach PCI and VLAN network interfaces to your servers to support the VMware networking topology. By default, each server is attached with one PCI network interface as the primary network interface. <br><br> For more information about the bare metal server network, see [Network of Bare Metal Servers for VPC](/docs/vpc?topic=vpc-bare-metal-servers-network). |
{: caption="Table 1. Planning checklist to create a bare metal server" caption-side="top"}

## Next steps
{: #next-steps-bm-on-vpc}

* If you're ready to get started, you can [create a bare metal server](/vpc-ext/provision/bm) on {{site.data.keyword.vpc_full}} now.
* For more information about creating a bare metal server on VPC, see [Creating bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).

<!-- and which [security group](/docs/vpc?topic=vpc-using-security-groups) to attach to each interface.-->

