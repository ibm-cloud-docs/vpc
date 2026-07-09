---

copyright:
  years: 2020, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot bare metal server, hardware issue, bare metal maintenance, maintenance state, hardware failure, bare metal support case

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How do I fix a hardware issue?
{: #bare-metal-troubleshoot-hardware-issues}
{: troubleshoot}
{: support}

Your bare metal server is experiencing a hardware issue.
{: shortdesc}

Your bare metal server experiences a hardware issue or is not functioning as expected.
{: tsSymptoms}

Bare metal server can exeprience hardware issues for a variety of reasons that IBM to perform maintenance on the server.
{: tsCauses}

Request support by creating a support case. For more information, see [Creating support cases](/docs/support?topic=support-open-case).
{: tsResolve}

After the operations team receives the case, the server enters a **Maintenance** state. Note the following behavior while your server is in this state:

- You must power off the bare metal server before it can enter the **Maintenance** state.
- You cannot start a bare metal server that is in a **Maintenance** state.
- The KVM console is unavailable.
- You cannot connect to the server over the network.
- IBM does not have access to your network and workloads during maintenance. The data on the disks is not examined. However, IBM recommends that you use software encryption for added security.

You can delete the bare metal server while it is in a **Maintenance** state.
{: note}

When the issue is resolved, the server is returned to you and the state changes back to **Stopped**.
