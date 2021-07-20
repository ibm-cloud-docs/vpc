---

copyright:
  years: 2020, 2021

lastupdated: "2021-07-20"

keywords: troubleshooting bare metal servers, hardware issues 

subcollection: vpc


---

{:beta: .beta}
{:codeblock: .codeblock}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:tip: .tip}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Troubleshooting Bare Metal Servers for VPC (Beta)
{: #troubleshoot_bare_metal}

## Bare metal servers have hardware issues
{: #hardware_issues}

When hardware issues occur, you can request support from IBM by creating a support case.

Click [here](https://cloud.ibm.com/unifiedsupport/cases/add%C2%A0) to create a case. For information about how to create a case, see [Support tickets](/docs/vpc?topic=vpc-getting-help#support-tickets).

After receiving the case, IBM operation team will put the bare metal server into maintenance mode.

You must first power off the bare metal server before it can be turned to the **Maintenance** state by the IBM operation team.
{: note}

You cannot start a bare metal server that is in the **Maintenance** state. The KVM console will also be disabled. You will not be able to connect to the system over the network.

During the maintenance, IBM also has no access to your network and workloads in any way. The data on the disks will not be examined. But we still recommend you use software encryption for added security.

You can delete the bare metal server that is in the **Maintenance** state.
{: note}

When the issues are fixed, the server will be handed back to you and the state will be turned back to **Stopped**.
