---

copyright:
  years: 2020, 2022

lastupdated: "2022-01-14"

keywords: troubleshooting bare metal servers, hardware issues, firmware

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

The following topics cover common difficulties that you might encounter, and offers some helpful tips.

## Bare metal servers have hardware issues
{: #hardware_issues}

When hardware issues occur, you can request support from IBM by creating a support case. Click [here](https://cloud.ibm.com/unifiedsupport/cases/add%C2%A0) to create a support case. For information about how to create a case, see [Support cases](/docs/vpc?topic=vpc-getting-help#support-tickets).

After receiving the case, IBM operation team puts the bare metal server into maintenance mode.

You must power off the bare metal server before it can go into the **Maintenance** state by the IBM operation team.
{: note}

You cannot start a bare metal server that is in the **Maintenance** state. The KVM console is also disabled. You can't connect to the system over the network.

During the maintenance, IBM also has no access to your network and workloads. The data on the disks isn't examined. But we still recommend you use software encryption for added security.

You can delete the bare metal server that is in the **Maintenance** state.
{: note}

When the issues are fixed, the server is handed back to you and the state returns to **Stopped**.

## Firware updates

Firmware for bare metal servers is managed by IBM. Manual firmware changes aren’t supported on the disk controller or disk drives without direction from IBM.
