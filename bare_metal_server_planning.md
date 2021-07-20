---

copyright:
  years: 2021
lastupdated: "2021-07-20"

keywords: bare metal servers, planning, tutorial

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

# Planning for bare metal servers
{: #planning-for-bare-metal-servers}

When you are planning to create the bare metal servers to host your VMware environments in VPC, go through the configuration checklist on this page.

This is a Beta feature that requires special approval. Contact your IBM Sales representative if you are interested in getting access.
{:beta}

Start [creating a bare metal server](/vpc-ext/provision/bm) on VPC now.
{:tip}

| Item | Considerations |
|----------|---------|
| Required IAM permission | ___ Make sure your account is assigned the required user permissions. You must be assigned the `Editor` role of the target resource group to create bare metal servers in this resource group. You also need to be assigned `Bare Metal Console Administrator` to access the ESXi Direct Console User Interface (DCUI) and `Bare Metal Advanced Network Operator` to modify IP spoofing and infrastructure NAT configuration on the bare metal server's network interfaces. <br><br> **Tip:** If you have authorization as an `Editor` or `Admin` for a VPC resource group, then you also inherit authorization to create, delete, and modify bare metal servers within that resource group. <br><br> **Note:** Source IP spoofing and infrastructure NAT configuration cannot be modified in Beta.|
| Account limits | ___ Check your [account limits](/docs/vpc?topic=vpc-quotas#service-limits) for concurrent resources. The maximum number of bare metal servers that can be created per account is 25. |
| Image | ___ The beta offering provides VMware ESXi image with two licensing options: Licensed ESXi 7.x image and BYOL (bring-your-own) ESXi 7.x image. You can handle the licensing of your bare metal servers by yourself by selecting the BYOL option. |
| SSH key | ___ Make sure your [SSH key](/docs/vpc?topic=vpc-ssh-keys#ssh-keys) is available. |
| Location | ___ Determine what region and zone to select. |
| Subnet | ___ Determine which subnets you want the bare metal server to connect to. |
| Profile | ___ Select the [profile](/docs/vpc?topic=vpc-bare-metal-servers-profile) that fits your workload the best. Main considerations include the number of vCPU, RAM size, and whether NVMe drives are needed.
| Naming | ___ Make sure you have a unique name for the bare metal server. The server name must be unique within an account and region. |
| Network interfaces | ___ Determine how many network interfaces you need.<br><br>You can attach PCI and VLAN network interfaces to the bare metal servers to support the VMware networking topology. By default, each bare metal server is attached with one PCI network interface as the server's primary network interface. <br><br> To learn more about bare metal server network, see [Network of Bare Metal Servers for VPC](/docs/vpc?topic=vpc-bare-metal-servers-network). |
{: caption="Table 1. Checklist for planning to create bare metal servers" caption-side="top"}

<!-- and which [security group](/docs/vpc?topic=vpc-using-security-groups) to attach to each interface.-->

