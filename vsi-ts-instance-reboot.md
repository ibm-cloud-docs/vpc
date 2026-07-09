---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot virtual server instance, instance reboot, instance not responding, instance-reset, hard reset

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my virtual server instance not responding to an `instance-reboot` request?
{: #troubleshooting-instance-not-responding}
{: troubleshoot}
{: support}

Your virtual server instance is not responding to an `instance-reboot` request.
{: shortdesc}

Your virtual server instance is not responding to an `instance-reboot` request.
{: tsSymptoms}

The `instance-reboot` request sends an OS-reboot reqeust to the instance and the virtual server instance is not responding.
{: tsCauses}

Try an `instance-reset` request instead. The `instance-reboot` request sends an OS-reboot request to the instance, while an `instance-reset` request performs a hard reset of the virtual server instance. You can think of the difference as typing "ctrl-alt-delete" on your keyboard versus pressing the reset or power button.

The `instance-reset` request takes longer to complete than the `instance-reboot` request.
{: note}
{: tsResolve}
