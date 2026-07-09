---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot virtual server instance, 409 conflict, instance action, instance status conflict

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I receive a 409 Conflict error when I create an instance action?
{: #troubleshooting-conflict-create-instance}
{: troubleshoot}
{: support}

You receive a 409 Conflict error when you try to perform an action on a virtual server instance.
{: shortdesc}

You receive a `409 Conflict` error when you try to create an instance action. For example, if your instance status is `stopped` and you try to create a `reboot` action, the system returns a `409 Conflict` error.
{: tsSymptoms}

Certain instance actions are not valid for the current status of the instance. The requested action conflicts with the instance's current state.
{: tsCauses}

Review the following table to identify which actions conflict with which instance statuses, then choose a compatible action.
{: tsResolve}

| Status | Action | Conflict |
| ------ | ------ | -------- |
| Running | start | Yes |
| Stopped | Any action other than start | Yes |
| Not running | pause | Yes |
| Not running | reboot | Yes |
| Not paused | resume | Yes |
| Paused | Any action other than resume | Yes |
{: caption="Instance status conflicts" caption-side="bottom"}
