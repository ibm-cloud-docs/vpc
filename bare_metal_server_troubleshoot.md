---

copyright:
  years: 2020, 2022

lastupdated: "2022-01-21"

keywords: troubleshooting bare metal servers, hardware issues, firmware

subcollection: vpc


---

{:codeblock: .codeblock}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:tip: .tip}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:external: target="_blank" .external}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:important: .important}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Troubleshooting Bare Metal Servers for VPC
{: #troubleshoot_bare_metal}

The following topics cover common difficulties that you might encounter, and offers some helpful tips.

## How do I fix a hardware issue?
{: #bare-metal-troubleshoot-hardware-issues}
{: troubleshoot}
{: support} 

If your bare metal server experiences a hardware issue, you can request support by creating a support case. Click [here](https://cloud.ibm.com/unifiedsupport/cases/add%C2%A0) to create a support case. For more information about creating a case, see [Support cases](/docs/vpc?topic=vpc-getting-help#support-tickets).

After the operation team receives the case, the server goes into a maintenance state.

You must power off the bare metal server before it can go into the **Maintenance** state.
{: note}

You can't start a bare metal server that's in a **Maintenance** state. The KVM console is also unavailable. You can't connect to the system over the network.

During maintenance, IBM also has no access to your network and workloads. The data on the disks isn't examined. But we still recommend you use software encryption for added security.

You can delete the bare metal server that is in the **Maintenance** state.
{: note}

When the issues are fixed, the server is handed back to you and the state returns to **Stopped**.

## How do I manage firmware updates?
{: #bare-metal-troubleshoot-firmware-update}
{: troubleshoot}
{: support} 

Firmware for bare metal servers is managed by IBM. Manual firmware changes aren’t supported on the disk controller or disk drives without direction from IBM.
